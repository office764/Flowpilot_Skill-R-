# B-Roll 결과 조회 + 최종 결과 집계 — 버그 분석 & 수정 가이드 v5
## FlowPilot | 2026.03.05

---

## 문제 요약

| 노드 | 증상 | 심각도 |
|------|------|:------:|
| B-Roll 결과 조회 | OUTPUT의 `response: null`, `completeTime: null`, `successFlag: 0` → 영상 URL이 빈 값 | **치명적** |
| 최종 결과 집계 | `talking_clips`가 5개가 아니라 **11개** 출력됨, 모든 `video_url`이 빈 문자열 `""` | **치명적** |
| 최종 결과 집계 | `broll_clips`는 5개 정상이지만, 모든 `video_url`이 빈 문자열 `""` | **치명적** |

---

## 문제 1: B-Roll 결과 조회 — 빈 URL 원인

### 근본 원인: Wait 시간 부족 (30초 → Veo 3.1은 2~5분 소요)

현재 워크플로우 흐름:

```
B-Roll 영상 생성 (Veo 3.1) → [30초 대기] → B-Roll 결과 조회 (HTTP GET)
```

**Veo 3.1 API (kie.ai)의 동작 방식:**

1. `POST /v1/task` → 즉시 `task_id`를 반환 (영상 생성 시작)
2. 내부적으로 영상 렌더링 진행 (약 **120~300초** 소요)
3. `GET /v1/task/{task_id}` → `status`, `response`, `completeTime` 반환

**30초 후 폴링하면 이런 응답이 온다:**

```json
{
  "status": "processing",
  "response": null,
  "completeTime": null,
  "successFlag": 0
}
```

`response: null` = 아직 영상이 안 만들어졌다.
`completeTime: null` = 완료 시간이 기록되지 않았다.
`successFlag: 0` = 성공 아님 (아직 진행 중).

**120~180초 후 폴링하면 이렇게 온다 (정상):**

```json
{
  "status": "completed",
  "response": {
    "resultUris": ["https://cdn.kie.ai/task/abc123/output.mp4"],
    "resultJson": "{\"video\":{\"url\":\"https://cdn.kie.ai/task/abc123/output.mp4\"}}"
  },
  "completeTime": "2026-03-05T12:34:56Z",
  "successFlag": 1
}
```

### 결론: 30초는 Veo 3.1이 영상을 완성하기에 **절대적으로 부족**하다.

---

### 수정 방법 A: Wait 노드 시간을 180초로 변경 (간단하지만 비효율)

1. B-Roll 영상 생성 (Veo 3.1) 다음에 있는 **Wait 노드**를 연다
2. Wait 값을 **30** → **180** (3분)으로 변경
3. Unit: **Seconds** 확인

**장점**: 수정이 1초만에 끝남
**단점**: 영상이 90초만에 완성되어도 무조건 180초를 기다림 → 5개 B-Roll × 180초 = 최소 15분 소요

---

### 수정 방법 B: 폴링 재시도 루프 구축 (권장 — 효율적)

Wait 시간을 고정하는 대신, **30초마다 결과를 확인하고 완료될 때까지 반복**하는 방식이다.

#### 아키텍처

```
B-Roll 영상 생성 (Veo 3.1)
     ↓
[B-Roll 폴링 대기 30초]  ← Wait 노드 (30초)
     ↓
[B-Roll 결과 조회]  ← HTTP GET /v1/task/{task_id}
     ↓
[B-Roll 완료 체크]  ← IF 노드
     ├── TRUE (response가 있음) → [B-Roll URL 추출] → B-Roll 순차 루프로 돌아감
     └── FALSE (response가 null) → [B-Roll 폴링 대기 30초]로 되돌아감 (재시도)
```

#### 새로 추가/수정할 노드 (3개)

**노드 1: B-Roll 폴링 대기 30초 (기존 Wait 노드 수정)**

이미 있는 Wait 노드의 시간을 30초로 유지하되, IF 노드에서 실패 시 다시 이 노드로 돌아오게 연결한다.

**노드 2: B-Roll 완료 체크 (새 IF 노드 추가)**

- 노드 타입: **IF** (`n8n-nodes-base.if`)
- 노드명: `B-Roll 완료 체크`
- 조건 설정:
  - **Value 1**: `{{ $json.response }}` (Expression 모드)
  - **Operation**: `is not empty`

또는 더 정확하게:
  - **Value 1**: `{{ $json.successFlag }}`
  - **Operation**: `equal`
  - **Value 2**: `1`

**노드 3: B-Roll URL 추출 (새 Code 노드 추가 또는 기존 코드 수정)**

IF 노드의 TRUE 출력에서 URL을 안전하게 추출한다.

```javascript
const pollResult = $input.first().json;
let videoUrl = '';

try {
  if (pollResult.response && pollResult.response.resultUris && pollResult.response.resultUris.length > 0) {
    videoUrl = pollResult.response.resultUris[0];
  }

  if (!videoUrl && pollResult.response && pollResult.response.resultJson) {
    const resultJson = typeof pollResult.response.resultJson === 'string'
      ? JSON.parse(pollResult.response.resultJson)
      : pollResult.response.resultJson;
    videoUrl = resultJson.video?.url || resultJson.url || resultJson.output?.[0] || '';
  }
} catch(e) {
  videoUrl = '';
}

if (!videoUrl) {
  throw new Error('B-Roll 영상 URL을 추출할 수 없습니다. response: ' + JSON.stringify(pollResult.response).substring(0, 500));
}

return [{
  json: {
    video_url: videoUrl,
    task_id: pollResult.task_id || pollResult.id || '',
    status: pollResult.status || 'completed',
    broll_id: $('B-Roll 순차 루프').first().json.id || '',
    broll_name: $('B-Roll 순차 루프').first().json.name || '',
    broll_prompt: $('B-Roll 순차 루프').first().json.prompt || ''
  }
}];
```

#### 연결 변경

| 출발 노드 | → | 도착 노드 | 비고 |
|----------|---|----------|------|
| B-Roll 결과 조회 | → | B-Roll 완료 체크 | 기존 연결 끊고 IF 노드로 연결 |
| B-Roll 완료 체크 | → (TRUE) | B-Roll URL 추출 | 성공 시 URL 추출 진행 |
| B-Roll URL 추출 | → | B-Roll 순차 루프 | 루프 반복으로 되돌아감 |
| B-Roll 완료 체크 | → (FALSE) | B-Roll 폴링 대기 30초 | 미완료 시 30초 더 대기 후 재조회 |

**주의: 무한 루프 방지**

최대 재시도 횟수를 제한하고 싶으면, B-Roll URL 추출 코드를 아래처럼 수정:

```javascript
const pollResult = $input.first().json;
const loopItem = $('B-Roll 순차 루프').first().json;
const retryCount = (loopItem._retryCount || 0) + 1;

if (retryCount > 10) {
  // 10번 × 30초 = 5분 넘게 기다렸으면 타임아웃
  return [{
    json: {
      video_url: 'TIMEOUT_ERROR',
      task_id: pollResult.task_id || '',
      status: 'timeout_after_5min',
      broll_id: loopItem.id || '',
      broll_name: loopItem.name || '',
      _retryCount: retryCount
    }
  }];
}

// ... (URL 추출 로직)
```

하지만 이 방식은 SplitInBatches와 함께 사용하면 복잡해질 수 있다. **방법 A(180초 고정 대기)를 먼저 적용하고, 안정화 후 방법 B로 전환하는 것을 권장**한다.

---

## 문제 2: 최종 결과 집계 — talking_clips가 11개인 이유

### 근본 원인: SplitInBatches "done" 포트의 데이터 누적 + 잘못된 데이터 소스 참조

#### n8n SplitInBatches의 데이터 흐름 이해

```
[5개 items 입력]
     ↓
[토킹 클립 순차 루프 (SplitInBatches, batch=1)]
     ├── Port 1 (loop): item 1 → InfiniteTalk → Wait → Poll → 결과 돌아옴
     ├── Port 1 (loop): item 2 → InfiniteTalk → Wait → Poll → 결과 돌아옴
     ├── Port 1 (loop): item 3 → ...
     ├── Port 1 (loop): item 4 → ...
     ├── Port 1 (loop): item 5 → ...
     └── Port 0 (done): 루프 완료 신호 → B-Roll 프롬프트 5개 생성으로 전달
```

**핵심 문제: Port 0 (done)이 전달하는 데이터가 뭔가?**

n8n의 SplitInBatches v3에서:
- Port 0 (done)은 루프가 완료된 후 **마지막으로 SplitInBatches에 입력된 데이터**를 전달한다
- 각 루프 반복에서 Poll 결과가 SplitInBatches로 돌아오는데, 이 데이터가 **원본 5개 item과 다른 구조**를 갖고 있다
- Poll HTTP Request의 응답은 `{ status, response, successFlag, ... }` 형태이고, 원본 item의 `{ segment_id, section, infinitetalk_prompt, ... }` 구조와 완전히 다르다

**11개가 나오는 원리:**

가능성 1: `B-Roll 프롬프트 5개 생성` 노드의 `$input.all()`이 SplitInBatches done 포트에서 누적된 item들을 받음
- SplitInBatches가 내부적으로 처리 과정의 중간 데이터를 누적
- 5개 원본 + 5개 Poll 응답 + 1개 루프 종료 신호 = 11개

가능성 2: 루프 내에서 Wait 노드나 다른 노드가 추가 item을 생성
- Wait 노드가 여러 item을 passthrough 하면서 중복이 발생

**어느 쪽이든 결론은 동일: `$input.all()`이 예상과 다른 데이터를 받고 있다.**

---

### 최종 결과 집계에서 video_url이 전부 비어있는 이유

**이유 1: B-Roll URL이 애초에 빈 값 (문제 1의 연쇄 효과)**

B-Roll 결과 조회에서 `response: null`이 나오면 → B-Roll URL이 빈 문자열 → 최종 집계에서도 당연히 빈 문자열.

**이유 2: 토킹 클립(InfiniteTalk) URL 추출 경로가 잘못됨**

현재 `최종 결과 집계` 코드가 토킹 클립 video_url을 추출할 때, SplitInBatches done 포트에서 넘어온 데이터의 구조를 잘못 읽고 있다.

Poll 결과의 실제 구조 (InfiniteTalk 결과 조회 HTTP GET 응답):

```json
{
  "data": {
    "status": "completed",
    "response": {
      "resultUris": ["https://cdn.kie.ai/infinitetalk/abc123.mp4"]
    },
    "resultJson": "{\"video\":{\"url\":\"https://cdn.kie.ai/infinitetalk/abc123.mp4\"}}"
  }
}
```

하지만 최종 결과 집계 코드가 이 구조를 제대로 파싱하지 못하면 `video_url: ""` 가 된다.

**이유 3: 토킹 클립 결과가 원본 segment 정보와 매칭 안 됨**

11개 item 중에서 어떤 것이 segment 1의 결과이고 어떤 것이 segment 2의 결과인지 구분할 수 없다. Poll 응답에는 `segment_id`가 없고 `task_id`만 있기 때문이다.

---

## 수정 방안: 최종 결과 집계 코드 전체 재작성

### 핵심 전략: `$input.all()` 대신 명시적 노드 참조 사용

`$input.all()`은 SplitInBatches done 포트의 불안정한 데이터에 의존한다.
대신 **`$('노드명').all()`을 사용해서 특정 노드의 출력을 직접 참조**한다.

### 수정된 전체 코드 (최종 결과 집계 노드에 복사)

`최종 결과 집계` 노드를 열고 → JavaScript 필드의 기존 코드를 **전체 선택 (Ctrl+A)** → 아래 코드를 **붙여넣기 (Ctrl+V)**:

```javascript
// ===== 1. 디렉터 AI 원본 데이터 (가장 신뢰할 수 있는 소스) =====
const directorItems = $('Code in JavaScript1').all();
const scriptData = $('대본 파싱').first().json;

// ===== 2. 토킹 클립 영상 URL 수집 =====
// 토킹 클립 결과 조회 노드의 출력에서 video URL을 추출한다
// 주의: 이 노드명은 실제 워크플로우의 "토킹 클립 결과 조회" 또는 "InfiniteTalk 결과 조회" 노드명과 정확히 일치해야 한다
let talkingPollItems = [];
try {
  talkingPollItems = $('토킹 클립 결과 조회').all();
} catch(e) {
  try {
    talkingPollItems = $('InfiniteTalk 결과 조회').all();
  } catch(e2) {
    talkingPollItems = [];
  }
}

const talkingClips = directorItems.map((dirItem, index) => {
  const dir = dirItem.json;
  let videoUrl = '';

  // 같은 인덱스의 Poll 결과가 있으면 URL 추출 시도
  if (talkingPollItems[index]) {
    const pollData = talkingPollItems[index].json;
    try {
      // 경로 1: pollData.data.response.resultUris[0]
      if (pollData.data && pollData.data.response && pollData.data.response.resultUris) {
        videoUrl = pollData.data.response.resultUris[0] || '';
      }
      // 경로 2: pollData.response.resultUris[0]
      if (!videoUrl && pollData.response && pollData.response.resultUris) {
        videoUrl = pollData.response.resultUris[0] || '';
      }
      // 경로 3: pollData.data.resultJson 파싱
      if (!videoUrl && pollData.data && pollData.data.resultJson) {
        const parsed = typeof pollData.data.resultJson === 'string'
          ? JSON.parse(pollData.data.resultJson)
          : pollData.data.resultJson;
        videoUrl = parsed.video?.url || parsed.url || parsed.output?.[0] || '';
      }
      // 경로 4: pollData.resultJson 파싱
      if (!videoUrl && pollData.resultJson) {
        const parsed = typeof pollData.resultJson === 'string'
          ? JSON.parse(pollData.resultJson)
          : pollData.resultJson;
        videoUrl = parsed.video?.url || parsed.url || parsed.output?.[0] || '';
      }
    } catch(e) {
      videoUrl = '';
    }
  }

  return {
    segment_id: dir.timeline_id,
    section: dir.section,
    time_range: dir.time_range,
    tts_text: dir.tts_text,
    video_url: videoUrl,
    status: videoUrl ? 'completed' : 'missing_url'
  };
});

// ===== 3. B-Roll 영상 URL 수집 =====
// B-Roll 결과 조회 노드의 출력에서 video URL을 추출한다
let brollPollItems = [];
try {
  brollPollItems = $('B-Roll 결과 조회').all();
} catch(e) {
  brollPollItems = [];
}

const brollClips = directorItems.map((dirItem, index) => {
  const dir = dirItem.json;
  let videoUrl = '';

  if (brollPollItems[index]) {
    const pollData = brollPollItems[index].json;
    try {
      // 경로 1: pollData.data.response.resultUris[0]
      if (pollData.data && pollData.data.response && pollData.data.response.resultUris) {
        videoUrl = pollData.data.response.resultUris[0] || '';
      }
      // 경로 2: pollData.response.resultUris[0]
      if (!videoUrl && pollData.response && pollData.response.resultUris) {
        videoUrl = pollData.response.resultUris[0] || '';
      }
      // 경로 3: pollData.data.resultJson 파싱
      if (!videoUrl && pollData.data && pollData.data.resultJson) {
        const parsed = typeof pollData.data.resultJson === 'string'
          ? JSON.parse(pollData.data.resultJson)
          : pollData.data.resultJson;
        videoUrl = parsed.video?.url || parsed.url || parsed.output?.[0] || '';
      }
      // 경로 4: pollData.resultJson 파싱
      if (!videoUrl && pollData.resultJson) {
        const parsed = typeof pollData.resultJson === 'string'
          ? JSON.parse(pollData.resultJson)
          : pollData.resultJson;
        videoUrl = parsed.video?.url || parsed.url || parsed.output?.[0] || '';
      }
    } catch(e) {
      videoUrl = '';
    }
  }

  return {
    broll_id: 'broll_' + (index + 1),
    broll_name: dir.broll_name || ('B-Roll ' + (index + 1)),
    section: dir.section,
    time_range: dir.time_range,
    video_url: videoUrl,
    status: videoUrl ? 'completed' : 'missing_url'
  };
});

// ===== 4. 최종 집계 =====
const talkingCompleted = talkingClips.filter(c => c.status === 'completed').length;
const brollCompleted = brollClips.filter(c => c.status === 'completed').length;
const allDone = (talkingCompleted === 5) && (brollCompleted === 5);

return [{
  json: {
    brand: scriptData.brand || directorItems[0]?.json.brand || '',
    product: scriptData.product || directorItems[0]?.json.product || '',
    price: scriptData.price || directorItems[0]?.json.price || '',
    full_script: scriptData.script?.full_script || directorItems[0]?.json.full_script || '',
    model_image_prompt: directorItems[0]?.json.model_image_prompt || '',
    talking_clips: talkingClips,
    broll_clips: brollClips,
    totals: {
      talking_total: 5,
      talking_completed: talkingCompleted,
      talking_missing: 5 - talkingCompleted,
      broll_total: 5,
      broll_completed: brollCompleted,
      broll_missing: 5 - brollCompleted
    },
    status: allDone ? 'ALL_COMPLETE' : 'PARTIAL',
    timestamp: new Date().toISOString(),
    next_step: allDone
      ? '모든 영상 생성 완료. Video Concatenator로 최종 편집 진행 가능.'
      : '일부 영상 URL이 누락됨. 누락된 항목의 Wait 시간을 늘려서 재실행 필요.'
  }
}];
```

---

## 코드 핵심 변경점 상세 설명

### 변경 1: `$input.all()` 제거 → `$('노드명').all()` 사용

| 기존 (문제) | 수정 후 (정상) |
|------------|--------------|
| `const items = $input.all();` — SplitInBatches done 포트의 누적 데이터 (11개) | `$('Code in JavaScript1').all()` — 디렉터 AI 출력 원본 (정확히 5개) |
| `$input.all()`에서 talking clip video URL 추출 시도 | `$('토킹 클립 결과 조회').all()`에서 직접 참조 |
| `$input.all()`에서 broll video URL 추출 시도 | `$('B-Roll 결과 조회').all()`에서 직접 참조 |

### 변경 2: 인덱스 기반 매칭

SplitInBatches는 item을 **순서대로** 처리한다 (batch=1).
따라서 `directorItems[0]`의 Poll 결과는 `talkingPollItems[0]`이고, `directorItems[1]`의 Poll 결과는 `talkingPollItems[1]`이다.

이 순서 의존성이 깨질 가능성은 매우 낮다 (SplitInBatches는 반드시 순차 실행하므로).

### 변경 3: 4단계 URL 추출 (안전한 파싱)

kie.ai API 응답 구조가 버전이나 상태에 따라 다를 수 있으므로, 4가지 경로를 순차적으로 시도한다:

```
경로 1: pollData.data.response.resultUris[0]         ← 가장 일반적
경로 2: pollData.response.resultUris[0]               ← data 래핑 없는 경우
경로 3: pollData.data.resultJson → parsed.video.url   ← resultJson 문자열인 경우
경로 4: pollData.resultJson → parsed.video.url        ← 플랫 구조인 경우
```

### 변경 4: `talking_clips` 정확히 5개 보장

기존: `$input.all().map(...)` → 입력 item 수에 따라 11개, 7개 등 불안정
수정: `directorItems.map(...)` → **항상 정확히 5개** (디렉터 AI가 5개 timeline을 출력하므로)

---

## 수정 후 예상 OUTPUT

### 정상 상태 (모든 영상 완성)

```json
{
  "brand": "KINOHI",
  "product": "키노히 바디미스트(santal)",
  "price": "30,000원",
  "full_script": "야 나 살냄새 좋다는 애기 진짜 많이 듣거든?...",
  "model_image_prompt": "UGC selfie of a natural Korean woman...",
  "talking_clips": [
    {
      "segment_id": 1,
      "section": "hook",
      "time_range": "0-6s",
      "tts_text": "야 나 살냄새 좋다는 애기 진짜 많이 듣거든?",
      "video_url": "https://cdn.kie.ai/infinitetalk/abc123.mp4",
      "status": "completed"
    },
    {
      "segment_id": 2,
      "section": "problem",
      "time_range": "6-12s",
      "tts_text": "근데 바디로션은 금방 날아가잖아...",
      "video_url": "https://cdn.kie.ai/infinitetalk/def456.mp4",
      "status": "completed"
    },
    {
      "segment_id": 3,
      "section": "discovery",
      "time_range": "12-18s",
      "tts_text": "...",
      "video_url": "https://cdn.kie.ai/infinitetalk/ghi789.mp4",
      "status": "completed"
    },
    {
      "segment_id": 4,
      "section": "solution",
      "time_range": "18-24s",
      "tts_text": "...",
      "video_url": "https://cdn.kie.ai/infinitetalk/jkl012.mp4",
      "status": "completed"
    },
    {
      "segment_id": 5,
      "section": "cta",
      "time_range": "24-30s",
      "tts_text": "...",
      "video_url": "https://cdn.kie.ai/infinitetalk/mno345.mp4",
      "status": "completed"
    }
  ],
  "broll_clips": [
    {
      "broll_id": "broll_1",
      "broll_name": "제품 클로즈업",
      "section": "hook",
      "time_range": "0-6s",
      "video_url": "https://cdn.kie.ai/veo3/pqr678.mp4",
      "status": "completed"
    },
    {
      "broll_id": "broll_2",
      "broll_name": "...",
      "section": "problem",
      "time_range": "6-12s",
      "video_url": "https://cdn.kie.ai/veo3/stu901.mp4",
      "status": "completed"
    },
    {
      "broll_id": "broll_3",
      "broll_name": "...",
      "section": "discovery",
      "time_range": "12-18s",
      "video_url": "https://cdn.kie.ai/veo3/vwx234.mp4",
      "status": "completed"
    },
    {
      "broll_id": "broll_4",
      "broll_name": "...",
      "section": "solution",
      "time_range": "18-24s",
      "video_url": "https://cdn.kie.ai/veo3/yza567.mp4",
      "status": "completed"
    },
    {
      "broll_id": "broll_5",
      "broll_name": "...",
      "section": "cta",
      "time_range": "24-30s",
      "video_url": "https://cdn.kie.ai/veo3/bcd890.mp4",
      "status": "completed"
    }
  ],
  "totals": {
    "talking_total": 5,
    "talking_completed": 5,
    "talking_missing": 0,
    "broll_total": 5,
    "broll_completed": 5,
    "broll_missing": 0
  },
  "status": "ALL_COMPLETE",
  "timestamp": "2026-03-05T14:30:00.000Z",
  "next_step": "모든 영상 생성 완료. Video Concatenator로 최종 편집 진행 가능."
}
```

### Wait 부족으로 일부 누락된 상태

```json
{
  "totals": {
    "talking_total": 5,
    "talking_completed": 5,
    "talking_missing": 0,
    "broll_total": 5,
    "broll_completed": 2,
    "broll_missing": 3
  },
  "status": "PARTIAL",
  "next_step": "일부 영상 URL이 누락됨. 누락된 항목의 Wait 시간을 늘려서 재실행 필요."
}
```

---

## 주의: `$('노드명').all()` 사용 시 노드명이 정확해야 한다

아래 3개 노드명이 실제 워크플로우의 노드명과 **글자 하나라도 다르면 에러**가 난다:

| 코드에서 참조하는 노드명 | 역할 | 확인 필요 |
|------------------------|------|----------|
| `Code in JavaScript1` | 디렉터 AI 출력 (5개 timeline) | n8n에서 노드 더블클릭 → 상단 이름 확인 |
| `토킹 클립 결과 조회` | InfiniteTalk Poll HTTP GET | n8n에서 노드 더블클릭 → 상단 이름 확인 |
| `B-Roll 결과 조회` | Veo 3.1 Poll HTTP GET | n8n에서 노드 더블클릭 → 상단 이름 확인 |
| `대본 파싱` | 대본 파싱 Code 노드 | n8n에서 노드 더블클릭 → 상단 이름 확인 |

**만약 이름이 다르면**, 코드 안의 `$('노드명')`을 실제 이름으로 변경해야 한다.

---

## 적용 순서 (2단계)

### Step 1: B-Roll Wait 시간 변경 (즉시 적용)

1. B-Roll 순차 루프 안에 있는 **Wait 노드**를 연다
2. Wait 값을 **30** → **180**으로 변경 (3분)
3. Unit: **Seconds** 확인

### Step 2: 최종 결과 집계 코드 교체

1. `최종 결과 집계` 노드를 더블클릭한다
2. JavaScript 필드의 기존 코드를 **전체 선택 (Ctrl+A)**
3. 위 "수정된 전체 코드" 섹션의 코드를 **붙여넣기 (Ctrl+V)**
4. 저장

### Step 3: 테스트 실행

1. 워크플로우를 저장하고 실행한다
2. `최종 결과 집계` 노드의 OUTPUT에서:
   - `talking_clips` 배열이 **정확히 5개**인지 확인
   - `broll_clips` 배열이 **정확히 5개**인지 확인
   - 각 clip의 `video_url`이 `https://cdn.kie.ai/...` 형태의 실제 URL인지 확인
   - `totals.talking_completed`와 `totals.broll_completed`가 **5**인지 확인
   - `status`가 **ALL_COMPLETE**인지 확인

---

## 트러블슈팅

### 만약 `$('토킹 클립 결과 조회').all()`에서 에러가 나면

**에러 메시지**: `Referenced node "토킹 클립 결과 조회" doesn't exist`

→ 노드 이름이 다른 것이다. n8n 캔버스에서 InfiniteTalk Poll 노드의 **정확한 이름**을 확인하고, 코드의 `$('토킹 클립 결과 조회')`를 그 이름으로 변경한다.

### 만약 talkingPollItems가 5개가 아니라 다른 수라면

SplitInBatches가 순차 실행이므로, 이 노드의 `.all()` 결과도 5개여야 한다. 5개가 아니면:
1. 해당 Poll 노드를 **Execute step**으로 단독 실행한다
2. OUTPUT의 item 수를 확인한다
3. item 수가 맞지 않으면, SplitInBatches와 Poll 노드 사이의 연결을 확인한다

### 만약 video_url은 있는데 재생이 안 되면

kie.ai CDN URL의 만료 시간 문제일 수 있다. 보통 24~72시간 유효하다.
최종 결과 집계 후 즉시 영상 다운로드/편집을 진행해야 한다.

### 만약 B-Roll video_url만 비어있고 talking video_url은 있다면

InfiniteTalk(120초 대기)은 충분하지만 Veo 3.1(30초 대기)은 부족한 것이다.
→ Step 1에서 Wait 시간을 180초로 바꿨는지 재확인한다.

---

*끝. B-Roll Wait 시간 변경(30초→180초) + 최종 결과 집계 코드 전체 교체, 이 2가지만 적용하면 두 문제가 모두 해결된다.*
