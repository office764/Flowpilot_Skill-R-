# TTS 파이프라인 수정 가이드 — 세그먼트 분할 + Typecast 음성 정상 출력
## FlowPilot | 2026.03.05

---

## 수정 대상 노드 3개 + 새로 추가할 노드 2개

```
[Code in JavaScript1] ← 5개 items (현재 정상 작동 확인됨)
     ↓
[스크립트 세그먼트 분할] ← 코드 전체 교체 (수정 1)
     ↓
[TTS 세그먼트 순차 루프] ← 기존 유지
     ↓
[세그먼트별 TTS 생성 (Typecast)] ← JSON Body + responseFormat 수정 (수정 2)
     ↓
[TTS 생성 대기 12초] ← 새로 추가 (추가 1)
     ↓
[TTS 오디오 폴링 조회] ← 새로 추가 (추가 2)
     ↓
[세그먼트 오디오 업로드 (tmpfiles)] ← 코드 수정 (수정 3)
     ↓
[세그먼트 오디오 URL 추출] ← 코드 전체 교체 (수정 4)
     ↓
(루프 반복 → TTS 세그먼트 순차 루프로 되돌아감)
```

---

## 수정 1: 스크립트 세그먼트 분할 — 코드 전체 교체

### 왜 교체해야 하는가

현재 코드가 `$input.first().json.script.lines[]` 를 읽는다. 이것은 `대본 파싱` 노드의 출력 형식이다.
하지만 이제 이 노드의 입력은 `Code in JavaScript1`의 출력(5개 timeline items)이다.
필드명이 완전히 다르기 때문에 코드를 전체 교체해야 한다.

### 현재 코드 (삭제할 것)

```javascript
const scriptData = $input.first().json;
const lines = scriptData.script.lines || [];

return lines.map(line => ({
  json: {
    segment_id: line.segment_index + 1,
    section: line.section,
    text: line.text,
    est_seconds: line.duration_sec,
    direction: line.direction,
    brand: scriptData.brand,
    product: scriptData.product,
    model_image_url: scriptData.model_image_url,
    product_image_main: scriptData.product_image_main,
    product_image_sub: scriptData.product_image_sub
  }
}));
```

### 새 코드 (전체 교체 — 복사용)

`스크립트 세그먼트 분할` 노드를 열고 → JavaScript 필드의 기존 코드를 **전체 선택(Ctrl+A)** → 아래 코드를 **붙여넣기(Ctrl+V)**:

```javascript
const items = $input.all();
const scriptData = $('대본 파싱').first().json;

return items.map((item, i) => ({
  json: {
    segment_id: item.json.timeline_id,
    section: item.json.section,
    text: item.json.tts_text,
    tts_text: item.json.tts_text,
    tts_emotion: item.json.tts_emotion,
    time_range: item.json.time_range,
    infinitetalk_prompt: item.json.infinitetalk_prompt,
    broll_prompt: item.json.broll_prompt,
    broll_name: item.json.broll_name,
    model_image_prompt: item.json.model_image_prompt,
    brand: item.json.brand || scriptData.brand || '',
    product: item.json.product || scriptData.product || '',
    price: item.json.price || scriptData.price || '',
    sensory: item.json.sensory || '',
    full_script: item.json.full_script || '',
    model_image_url: scriptData.model_image_url || '',
    product_image_main: scriptData.product_image_main || '',
    product_image_sub: scriptData.product_image_sub || ''
  }
}));
```

### 필드 매핑 설명

| 새 코드 필드 | 데이터 출처 | 용도 |
|-------------|-----------|------|
| `segment_id` | `item.json.timeline_id` | 하위 노드들이 segment_id로 참조하므로 timeline_id를 segment_id로 매핑 |
| `text` | `item.json.tts_text` | **핵심**: Typecast 노드가 `$json.text`를 참조하므로 tts_text를 text로도 매핑 |
| `tts_text` | `item.json.tts_text` | 원본 필드 유지 (이중 매핑) |
| `tts_emotion` | `item.json.tts_emotion` | Typecast emotion 파라미터에 사용 |
| `infinitetalk_prompt` | `item.json.infinitetalk_prompt` | InfiniteTalk 노드에서 사용 |
| `broll_prompt` | `item.json.broll_prompt` | Veo 3.1 노드에서 사용 |
| `broll_name` | `item.json.broll_name` | B-Roll 파일명 식별 |
| `model_image_url` | `scriptData.model_image_url` | 대본 파싱에서 가져온 기존 모델 이미지 URL (있으면) |
| `product_image_main` | `scriptData.product_image_main` | 제품 이미지 URL |
| `product_image_sub` | `scriptData.product_image_sub` | 제품 서브 이미지 URL |

### 수정 후 예상 OUTPUT (5 items)

```
Item 1:
  segment_id: 1
  section: "hook"
  text: "야 나 살냄새 좋다는 얘기 진짜 많이 듣거든?"  ← Typecast가 읽는 필드
  tts_text: "야 나 살냄새 좋다는 얘기 진짜 많이 듣거든?"  ← 원본 유지
  tts_emotion: "excited"
  time_range: "0-6s"
  infinitetalk_prompt: "Camera: iPhone 15 Pro, Lens: 26mm f/1.6..."
  broll_prompt: "Camera: RED Komodo 6K, Lens: Canon RF 35mm..."
  broll_name: "제품 클로즈업"
  model_image_prompt: "UGC selfie of a natural Korean woman..."
  brand: "KINOHI"
  product: "키노히 바디미스트(santal)"
  ...
```

---

## 수정 2: 세그먼트별 TTS 생성 (Typecast) — JSON Body + responseFormat 수정

### 현재 문제

1. `responseFormat: "file"` → Typecast API는 비동기(async)이므로 JSON 응답을 반환한다. "file"로 설정하면 이 JSON을 바이너리 파일로 오인해서 깨진 데이터가 만들어진다. **이것이 TTS 음성이 안 나오는 근본 원인이다.**

2. `"emotion_type": "smart"` → 하드코딩. 디렉터 AI가 생성한 `tts_emotion`을 사용하지 않고 있다.

### 수정 방법 (2단계)

#### Step 2-1: responseFormat 변경

1. `세그먼트별 TTS 생성 (Typecast)` 노드를 연다
2. 하단의 **Options** 섹션을 연다
3. **Response** → **Response Format** 드롭다운을 클릭한다
4. 현재 **"File"**로 되어 있는 것을 **"JSON"**으로 변경한다

#### Step 2-2: JSON Body 수정

현재 JSON Body:
```json
={
  "text": "{{ $json.text }}",
  "model": "ssfm-v30",
  "voice_id": "tc_646f0dac2465b38f37787f64",
  "language": "kor",
  "prompt": {
    "emotion_type": "smart",
    "previous_text": "",
    "next_text": ""
  },
  "output": {
    "audio_format": "mp3",
    "audio_tempo": 1.15,
    "volume": 75
  }
}
```

아래로 **전체 교체**:
```
={
  "text": "{{ $json.text }}",
  "model": "ssfm-v30",
  "voice_id": "tc_646f0dac2465b38f37787f64",
  "language": "kor",
  "prompt": {
    "emotion_type": "{{ $json.tts_emotion || 'smart' }}",
    "previous_text": "",
    "next_text": ""
  },
  "output": {
    "audio_format": "mp3",
    "audio_tempo": 1.15,
    "volume": 75
  }
}
```

변경점:
- `"emotion_type": "smart"` → `"emotion_type": "{{ $json.tts_emotion || 'smart' }}"` 로 변경
- `$json.text`는 그대로 유지 (수정 1에서 `tts_text`를 `text`로도 매핑했기 때문에 정상 작동)

**중요**: Typecast API의 `emotion_type`이 `excited`, `empathetic` 같은 값을 지원하지 않을 수 있다. 만약 에러가 나면 아래 매핑 방식으로 변경:

```
={{ ({'excited':'happy','empathetic':'sad','surprised':'happy','confident':'smart','warm':'smart','curious':'smart','urgent':'smart'})[$json.tts_emotion] || 'smart' }}
```

이 expression을 `emotion_type` 값에 넣으면, 디렉터 AI의 감정 태그가 Typecast가 지원하는 값으로 자동 변환된다.

---

## 수정 2 이후: Typecast API 응답 구조 (responseFormat: "json" 변경 후)

`responseFormat`을 **"json"**으로 바꾸면, Typecast API는 다음과 같은 JSON을 반환한다:

```json
{
  "result": {
    "speak_v2_id": "sp_64a7b8c9d0e1f2g3h4i5j6k7",
    "status": "progress",
    "audio_download_url": null
  }
}
```

`status`가 `"progress"`이고 `audio_download_url`이 `null`이다.
즉, **아직 오디오 파일이 준비되지 않았다**.

10~15초 후에 `GET /v1/text-to-speech/{speak_v2_id}`를 호출하면:

```json
{
  "result": {
    "speak_v2_id": "sp_64a7b8c9d0e1f2g3h4i5j6k7",
    "status": "done",
    "audio_download_url": "https://cdn.typecast.ai/audio/abc123.mp3"
  }
}
```

`status`가 `"done"`이고 `audio_download_url`에 실제 MP3 URL이 들어온다.

**따라서**: Typecast POST 후 바로 tmpfiles에 업로드할 수 없다 (아직 오디오가 없으니까). 중간에 **대기 + 폴링** 노드를 넣어야 한다.

---

## 추가 1: TTS 생성 대기 12초 — 새 노드 추가

### 추가할 노드

- **노드 타입**: Wait (n8n-nodes-base.wait)
- **노드명**: `TTS 생성 대기 12초`
- **설정**:
  - Wait 시간: **12** (초)

### 추가 방법

1. n8n 캔버스에서 `세그먼트별 TTS 생성 (Typecast)` 노드와 `세그먼트 오디오 업로드 (tmpfiles)` 노드 사이의 **연결선을 끊는다** (연결선 위에서 우클릭 → Delete 또는 연결선을 클릭하고 Delete 키)
2. 캔버스의 빈 공간을 클릭하고 **+** 버튼 또는 **Tab** 키를 누른다
3. 검색창에 **Wait** 를 입력한다
4. **Wait** 노드를 선택하여 추가한다
5. 노드 이름을 `TTS 생성 대기 12초`로 변경한다
6. 노드를 열고 **Resume** → **After Time Interval** 선택
7. **Wait** 값을 **12** 로 입력
8. **Unit**을 **Seconds**로 설정

### 연결

```
세그먼트별 TTS 생성 (Typecast) → TTS 생성 대기 12초
```

- `세그먼트별 TTS 생성 (Typecast)` 노드의 출력 포트를 `TTS 생성 대기 12초` 노드의 입력 포트에 연결

---

## 추가 2: TTS 오디오 폴링 조회 — 새 노드 추가

### 추가할 노드

- **노드 타입**: HTTP Request (n8n-nodes-base.httpRequest)
- **노드명**: `TTS 오디오 폴링 조회`

### 설정

1. **Method**: GET
2. **URL**: 아래 expression을 URL 필드에 붙여넣기:
```
=https://api.typecast.ai/v1/text-to-speech/{{ $('세그먼트별 TTS 생성 (Typecast)').item.json.result.speak_v2_id }}
```
3. **Authentication**: Generic Credential Type 선택
4. **Generic Auth Type**: Header Auth 선택
5. **Credential for Header Auth**: 드롭다운에서 **Typecast TTS** (기존에 만들어둔 크리덴셜) 선택
6. **Options** → **Response** → **Response Format** → **JSON** (기본값 유지)

### 연결

```
TTS 생성 대기 12초 → TTS 오디오 폴링 조회
```

### 이 노드의 예상 응답

```json
{
  "result": {
    "speak_v2_id": "sp_64a7b8c9d0e1f2g3h4i5j6k7",
    "status": "done",
    "audio_download_url": "https://cdn.typecast.ai/audio/abc123.mp3"
  }
}
```

---

## 수정 3: 세그먼트 오디오 업로드 (tmpfiles) — 연결 변경 + 방식 변경

### 현재 문제

현재 이 노드는 **바이너리 데이터(data)**를 tmpfiles에 업로드한다.
하지만 Typecast 폴링 결과는 **audio_download_url**(문자열 URL)을 반환하므로, 바이너리가 아니라 URL이다.

### 2가지 옵션

**옵션 A: tmpfiles 노드를 삭제하고 audio_download_url을 직접 사용** (권장)

Typecast의 `audio_download_url`이 공개 URL이라면, tmpfiles에 업로드할 필요 없이 이 URL을 InfiniteTalk에 직접 전달하면 된다.

이 경우:
1. `세그먼트 오디오 업로드 (tmpfiles)` 노드를 **우회(bypass)**한다
2. `TTS 오디오 폴링 조회` → 직접 `세그먼트 오디오 URL 추출`로 연결한다

**옵션 B: audio_download_url에서 파일을 다운받아 tmpfiles에 재업로드**

Typecast의 URL이 인증이 필요하거나 짧은 시간 후 만료된다면, 이 방법이 필요하다.
하지만 이 방식은 복잡하므로 **먼저 옵션 A를 시도**하고, InfiniteTalk에서 오디오 URL 접근이 안 되면 옵션 B로 전환한다.

### 권장: 옵션 A 적용

`TTS 오디오 폴링 조회` 노드의 출력을 직접 `세그먼트 오디오 URL 추출` 노드에 연결한다.

```
TTS 오디오 폴링 조회 → 세그먼트 오디오 URL 추출
```

(`세그먼트 오디오 업로드 (tmpfiles)` 노드는 연결을 끊어두되, 삭제하지 말고 캔버스에 남겨둔다. 나중에 필요하면 다시 쓸 수 있다.)

---

## 수정 4: 세그먼트 오디오 URL 추출 — 코드 전체 교체

### 현재 코드 (삭제할 것)

```javascript
const result = $input.first().json;
const segData = $('TTS 세그먼트 순차 루프').first().json;
let audioUrl = '';
try {
  const tmpUrl = result.data?.url || result.url || '';
  audioUrl = tmpUrl.replace('tmpfiles.org/', 'tmpfiles.org/dl/');
} catch(e) {
  audioUrl = 'ERROR: ' + e.message;
}
return [{ json: { audio_url: audioUrl, segment_id: segData.segment_id, section: segData.section, text: segData.text } }];
```

이 코드는 tmpfiles의 응답에서 URL을 추출한다. 하지만 이제 tmpfiles를 우회하고 Typecast 폴링 결과에서 직접 `audio_download_url`을 가져오므로 코드를 교체해야 한다.

### 새 코드 (전체 교체 — 복사용)

`세그먼트 오디오 URL 추출` 노드를 열고 → JavaScript 필드의 기존 코드를 **전체 선택(Ctrl+A)** → 아래 코드를 **붙여넣기(Ctrl+V)**:

```javascript
const pollResult = $input.first().json;
const segData = $('TTS 세그먼트 순차 루프').first().json;

let audioUrl = '';
let status = '';

try {
  if (pollResult.result) {
    status = pollResult.result.status || '';
    audioUrl = pollResult.result.audio_download_url || '';
  }

  if (!audioUrl && pollResult.audio_download_url) {
    audioUrl = pollResult.audio_download_url;
  }

  if (!audioUrl && pollResult.data && pollResult.data.audio_download_url) {
    audioUrl = pollResult.data.audio_download_url;
  }
} catch(e) {
  audioUrl = 'ERROR: ' + e.message;
}

if (!audioUrl || audioUrl === '') {
  throw new Error('TTS 오디오 URL을 찾을 수 없습니다. status: ' + status + ', 전체 응답: ' + JSON.stringify(pollResult).substring(0, 500));
}

return [{
  json: {
    audio_url: audioUrl,
    tts_status: status,
    segment_id: segData.segment_id,
    section: segData.section,
    text: segData.text,
    tts_text: segData.tts_text || segData.text,
    tts_emotion: segData.tts_emotion || '',
    infinitetalk_prompt: segData.infinitetalk_prompt || '',
    broll_prompt: segData.broll_prompt || '',
    broll_name: segData.broll_name || '',
    model_image_prompt: segData.model_image_prompt || '',
    brand: segData.brand || '',
    product: segData.product || '',
    price: segData.price || '',
    model_image_url: segData.model_image_url || '',
    product_image_main: segData.product_image_main || '',
    product_image_sub: segData.product_image_sub || ''
  }
}];
```

### 코드 설명

1. `pollResult`에서 `audio_download_url`을 3가지 경로로 탐색한다 (Typecast API 응답 구조가 버전에 따라 다를 수 있으므로):
   - `pollResult.result.audio_download_url` (표준)
   - `pollResult.audio_download_url` (플랫 구조)
   - `pollResult.data.audio_download_url` (래핑된 구조)

2. URL을 찾지 못하면 **에러를 던진다** (throw). 이렇게 하면 n8n에서 어디서 실패했는지 명확히 볼 수 있다. 에러 메시지에 status와 응답 원문(500자까지)을 포함하여 디버깅이 쉽다.

3. `segData`에서 `infinitetalk_prompt`, `broll_prompt`, `broll_name` 등 **디렉터 AI의 모든 필드를 하위 노드까지 전달**한다. 이것들이 나중에 InfiniteTalk와 B-Roll 노드에서 필요하다.

### 수정 후 예상 OUTPUT (item 1개씩, 루프마다)

```
{
  audio_url: "https://cdn.typecast.ai/audio/abc123.mp3",
  tts_status: "done",
  segment_id: 1,
  section: "hook",
  text: "야 나 살냄새 좋다는 얘기 진짜 많이 듣거든?",
  tts_text: "야 나 살냄새 좋다는 얘기 진짜 많이 듣거든?",
  tts_emotion: "excited",
  infinitetalk_prompt: "Camera: iPhone 15 Pro, Lens: 26mm f/1.6...",
  broll_prompt: "Camera: RED Komodo 6K, Lens: Canon RF 35mm...",
  broll_name: "제품 클로즈업",
  model_image_prompt: "UGC selfie of a natural Korean woman...",
  brand: "KINOHI",
  product: "키노히 바디미스트(santal)",
  price: "30,000원",
  model_image_url: "(기존 모델 이미지 URL 또는 빈 값)",
  product_image_main: "(제품 이미지 URL)",
  product_image_sub: "(제품 서브 이미지 URL)"
}
```

---

## 전체 연결 순서 (최종)

### 변경 전 (현재)

```
스크립트 세그먼트 분할 → TTS 순차 루프 → Typecast TTS (file) → tmpfiles 업로드 → 오디오 URL 추출 → 루프 반복
```

### 변경 후

```
스크립트 세그먼트 분할 → TTS 순차 루프 → Typecast TTS (json) → TTS 대기 12초 → TTS 오디오 폴링 조회 → 오디오 URL 추출 → 루프 반복
```

### 노드별 연결 상세

| 출발 노드 | → | 도착 노드 | 연결 포트 |
|----------|---|----------|----------|
| Code in JavaScript1 | → | 스크립트 세그먼트 분할 | main[0] → main[0] |
| 스크립트 세그먼트 분할 | → | TTS 세그먼트 순차 루프 | main[0] → main[0] |
| TTS 세그먼트 순차 루프 | → (루프 완료) | 이미지+세그먼트 합치기 | main[0] → main[1] |
| TTS 세그먼트 순차 루프 | → (루프 진행) | 세그먼트별 TTS 생성 (Typecast) | main[1] → main[0] |
| 세그먼트별 TTS 생성 (Typecast) | → | TTS 생성 대기 12초 | main[0] → main[0] |
| TTS 생성 대기 12초 | → | TTS 오디오 폴링 조회 | main[0] → main[0] |
| TTS 오디오 폴링 조회 | → | 세그먼트 오디오 URL 추출 | main[0] → main[0] |
| 세그먼트 오디오 URL 추출 | → | TTS 세그먼트 순차 루프 | main[0] → main[0] (루프 반복) |

### 연결 끊을 곳

| 출발 노드 | → | 도착 노드 | 동작 |
|----------|---|----------|------|
| 세그먼트별 TTS 생성 (Typecast) | → | 세그먼트 오디오 업로드 (tmpfiles) | **연결 끊기** |
| 세그먼트 오디오 업로드 (tmpfiles) | → | 세그먼트 오디오 URL 추출 | **연결 끊기** |

(`세그먼트 오디오 업로드 (tmpfiles)` 노드는 삭제하지 말고 캔버스에 남겨둔다)

---

## 실행 순서 체크리스트

### Phase 1: 코드/설정 변경

- [ ] 1. `스크립트 세그먼트 분할` 노드 열기 → JavaScript 코드 전체 교체 (수정 1)
- [ ] 2. `세그먼트별 TTS 생성 (Typecast)` 노드 열기 → Options → Response Format을 **JSON**으로 변경
- [ ] 3. 같은 노드의 JSON Body에서 `"emotion_type": "smart"` 를 `"emotion_type": "{{ $json.tts_emotion || 'smart' }}"` 로 변경
- [ ] 4. `세그먼트 오디오 URL 추출` 노드 열기 → JavaScript 코드 전체 교체 (수정 4)

### Phase 2: 새 노드 추가

- [ ] 5. `TTS 생성 대기 12초` Wait 노드 추가 (Wait: 12초)
- [ ] 6. `TTS 오디오 폴링 조회` HTTP Request 노드 추가 (GET, URL에 speak_v2_id expression)

### Phase 3: 연결 변경

- [ ] 7. `Typecast TTS` → `tmpfiles 업로드` 연결 끊기
- [ ] 8. `tmpfiles 업로드` → `오디오 URL 추출` 연결 끊기
- [ ] 9. `Typecast TTS` → `TTS 생성 대기 12초` 연결
- [ ] 10. `TTS 생성 대기 12초` → `TTS 오디오 폴링 조회` 연결
- [ ] 11. `TTS 오디오 폴링 조회` → `세그먼트 오디오 URL 추출` 연결

### Phase 4: 테스트

- [ ] 12. 워크플로우 저장
- [ ] 13. 테스트 실행
- [ ] 14. `세그먼트 오디오 URL 추출` 노드의 출력에서 `audio_url`이 실제 MP3 URL인지 확인
- [ ] 15. 해당 URL을 브라우저에 직접 붙여넣어서 음성이 재생되는지 확인

---

## 트러블슈팅

### 만약 TTS 폴링 결과의 status가 "progress"(아직 미완료)라면

12초가 부족한 것이다. `TTS 생성 대기 12초` 노드의 대기 시간을 **20초**로 늘린다.

### 만약 Typecast API 응답에 result.speak_v2_id가 없다면

Typecast API 버전이 다를 수 있다. `세그먼트별 TTS 생성 (Typecast)` 노드를 **Execute step**으로 단독 실행하고, OUTPUT의 JSON 탭에서 실제 응답 구조를 확인한다. 그 구조에 맞게 폴링 URL을 조정해야 한다.

### 만약 audio_download_url에 접근이 안 된다면 (InfiniteTalk에서 오디오 로드 실패)

이 경우 Typecast의 audio_download_url이 인증이 필요하거나 만료되는 URL이다.
그때는:
1. `TTS 오디오 폴링 조회` 후 HTTP Request(GET)로 audio_download_url에서 파일 다운로드
2. 다운받은 바이너리를 `세그먼트 오디오 업로드 (tmpfiles)`에 전달
3. tmpfiles URL을 InfiniteTalk에 전달

이 추가 작업이 필요하면 말해달라.

### 만약 Typecast emotion_type에서 에러가 나면

Typecast가 지원하지 않는 emotion 값이 전달된 것이다. JSON Body의 emotion_type 값을 아래로 교체:

```
={{ ({'excited':'happy','empathetic':'sad','surprised':'happy','confident':'smart','warm':'smart','curious':'smart','urgent':'smart'})[$json.tts_emotion] || 'smart' }}
```

---

*끝. 위 4개 수정 + 2개 추가를 순서대로 적용하면 TTS 파이프라인이 정상 작동한다.*
