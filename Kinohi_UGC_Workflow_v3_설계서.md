# Kinohi AI UGC 리뷰 영상 생성기 v3 — 전체 워크플로우 재설계

## 핵심 변경사항 요약

| 항목 | v2 (현재) | v3 (개선) |
|------|----------|----------|
| 제품 이미지 | URL 입력만 가능 | **파일 업로드 + URL 둘 다 지원** |
| TTS 생성 | 풀스크립트 1번 생성 (28초) | **5개 세그먼트별 개별 생성 (각 ≤15초)** |
| InfiniteTalk | 1번 호출 (15초 제한으로 실패) | **5회 루프 호출 → 각 클립 성공** |
| 토킹 영상 | 단일 클립 | **5개 클립 이어붙이기 (30초 완성)** |
| B-Roll | 5개 생성 (유지) | 5개 생성 (유지) |
| 최종 출력 | 토킹 영상 + B-Roll 개별 전달 | **토킹 영상 5편 + B-Roll 5편 + 전체 URL 리스트** |

---

## 전체 워크플로우 아키텍처

```
[Form Trigger] ─→ [제품 데이터 정리] ─→ [상세페이지 URL 체크]
                                              │
                          ┌───────────────────┴──────────────────┐
                          ▼                                      ▼
                   [상세페이지 크롤링]                    [URL 없음 - 패스스루]
                          │                                      │
                          └───────────────┬──────────────────────┘
                                          ▼
                                [페이지+데이터 합치기]
                                          │
                                          ▼
                                   [제품 분석 AI]
                                          │
                                          ▼
                                  [분석 결과 파싱]
                                          │
                                          ▼
                              [나레이션 대본 작성 AI]
                                          │
                                          ▼
                                   [대본 파싱]
                                          │
                    ┌─────────────────────┼─────────────────────┐
                    ▼                     ▼                     ▼
          [모델 이미지 체크]    [TTS 5분할 생성]        [B-Roll 프롬프트 5개]
                    │             (NEW!)                        │
                    ▼                     │                     ▼
          [이미지 생성/기존]               ▼              [B-Roll 순차 루프]
                    │         [5개 세그먼트 루프]               │
                    │             │                             ▼
                    │             ▼                      [Veo 3.1 생성]
                    │    [세그먼트별 TTS]                       │
                    │             │                             ▼
                    │             ▼                      [B-Roll 대기]
                    │    [오디오 업로드]                        │
                    │             │                             ▼
                    │             ▼                      [B-Roll 결과]
                    │    [InfiniteTalk 호출]
                    │             │
                    │             ▼
                    │    [토킹 영상 대기]
                    │             │
                    │             ▼
                    │    [토킹 영상 결과]
                    │             │
                    │             ▼
                    │    [루프 반복 (5회)]
                    │             │
                    └──────┬──────┘
                           ▼
                    [최종 결과 집계]
```

---

## PHASE 1: Form Trigger 수정

### 추가할 필드: 제품 이미지 업로드

현재 Form Trigger 필드:
1. 브랜드명 (text, required)
2. 제품명 (text, required)
3. 가격 (text, required)
4. 제품 설명 (textarea, required)
5. 고객 리뷰 요약 (textarea, required)
6. 핵심 셀링포인트 (textarea, optional)
7. 상세페이지 URL (text, optional)
8. AI 모델 이미지 URL (text, optional)

**추가할 필드:**
9. **제품 이미지** (file upload, optional) — 제품 실물 사진 업로드. B-Roll 프롬프트에 제품 외형 묘사 반영용

### Form Trigger 노드 설정
- Field Type: **File**
- Field Label: **제품 이미지**
- Required: No
- Accept File Types: image/jpeg, image/png, image/webp

---

## PHASE 2: TTS 5분할 생성 (핵심 변경)

### 문제점
InfiniteTalk는 한 번에 최대 15초 오디오만 처리 가능.
30초 나레이션을 한번에 보내면 거부당함.

### 해결: 스크립트를 5개 세그먼트로 나눠서 각각 TTS 생성

#### Node: 스크립트 세그먼트 분할 (Code 노드)

나레이션 대본의 7개 라인을 5개 세그먼트로 묶는다:

| 세그먼트 | 라인 | 섹션 | 예상 시간 |
|---------|------|------|----------|
| 1 | line 1 | HOOK | ~3.3초 |
| 2 | line 2 | PROBLEM | ~3.8초 |
| 3 | line 3-4 | DISCOVERY | ~4.7초 |
| 4 | line 5-6 | SOLUTION | ~8.7초 |
| 5 | line 7 | CTA | ~6.0초 |

세그먼트 4가 8.7초로 가장 길지만 15초 이내이므로 OK.

#### Code 노드 JavaScript:

```javascript
const lines = $input.first().json.script.lines;

const segments = [
  { id: 1, section: "HOOK", text: lines.slice(0, 1).map(l => l.text).join(' ') },
  { id: 2, section: "PROBLEM", text: lines.slice(1, 2).map(l => l.text).join(' ') },
  { id: 3, section: "DISCOVERY", text: lines.slice(2, 4).map(l => l.text).join(' ') },
  { id: 4, section: "SOLUTION", text: lines.slice(4, 6).map(l => l.text).join(' ') },
  { id: 5, section: "CTA", text: lines.slice(6, 7).map(l => l.text).join(' ') }
];

return segments.map(seg => ({ json: seg }));
```

이 Code 노드는 **5개의 아이템**을 출력한다. 각 아이템이 하나의 세그먼트.

#### Node: TTS 세그먼트 순차 생성 루프 (SplitInBatches)
- Batch Size: 1
- 5개 세그먼트를 하나씩 순차 처리

#### Node: 세그먼트별 TTS (HTTP Request - Typecast)
- URL: `https://api.typecast.ai/v1/text-to-speech`
- Auth: Header Auth (X-API-KEY)
- Body (Expression):

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

- Response Format: **File** (바이너리 MP3 반환)

#### Node: 세그먼트 오디오 업로드 (HTTP Request - tmpfiles.org)
- 각 MP3를 tmpfiles.org에 업로드하여 공개 URL 획득
- URL 변환: `tmpfiles.org/` → `tmpfiles.org/dl/`

---

## PHASE 3: InfiniteTalk 5회 루프

### 루프 구조

```
[TTS 세그먼트 루프] → [오디오 업로드] → [InfiniteTalk POST] → [대기 120초] → [폴링] → [결과 저장] → [루프 반복]
```

#### Node: InfiniteTalk POST (HTTP Request)
- URL: `https://api.kie.ai/api/v1/jobs/createTask`
- Body (Expression):

```json
={
  "model": "infinitalk/from-audio",
  "input": {
    "image_url": "{{ $('이미지 URL 추출').item.json.image_url }}",
    "audio_url": "{{ $json.audio_url }}",
    "prompt": "A natural-looking young Korean woman aged 24 casually talking in her bathroom, wearing a white cotton t-shirt, slightly damp hair loosely tied back, soft natural daylight, realistic subtle facial micro-expressions and natural lip sync, natural eye blinks and head micro-movements",
    "resolution": "720p"
  }
}
```

**핵심:** `image_url`은 모든 5개 클립에서 **동일한 이미지** 사용 (일관성 유지).
`audio_url`만 세그먼트별로 다르다.

#### Node: 폴링 (GET)
- URL: `=https://api.kie.ai/api/v1/jobs/recordInfo?taskId={{ $json.data.taskId }}`

#### Node: 결과 URL 수집 (Code 노드)
각 루프에서 나온 영상 URL을 배열에 축적.

---

## PHASE 4: 최종 결과 집계

### 출력 데이터 구조

```json
{
  "brand": "Kinohi",
  "product": "키노히 바디미스트 santal",
  "talking_avatar_clips": [
    {
      "segment": 1,
      "section": "HOOK",
      "video_url": "https://tempfile.aiquickdraw.com/v/xxx_1.mp4"
    },
    {
      "segment": 2,
      "section": "PROBLEM",
      "video_url": "https://tempfile.aiquickdraw.com/v/xxx_2.mp4"
    },
    {
      "segment": 3,
      "section": "DISCOVERY",
      "video_url": "https://tempfile.aiquickdraw.com/v/xxx_3.mp4"
    },
    {
      "segment": 4,
      "section": "SOLUTION",
      "video_url": "https://tempfile.aiquickdraw.com/v/xxx_4.mp4"
    },
    {
      "segment": 5,
      "section": "CTA",
      "video_url": "https://tempfile.aiquickdraw.com/v/xxx_5.mp4"
    }
  ],
  "broll_clips": [
    { "scene": 1, "video_url": "https://tempfile.aiquickdraw.com/v/broll_1.mp4" },
    { "scene": 2, "video_url": "https://tempfile.aiquickdraw.com/v/broll_2.mp4" },
    { "scene": 3, "video_url": "https://tempfile.aiquickdraw.com/v/broll_3.mp4" },
    { "scene": 4, "video_url": "https://tempfile.aiquickdraw.com/v/broll_4.mp4" },
    { "scene": 5, "video_url": "https://tempfile.aiquickdraw.com/v/broll_5.mp4" }
  ],
  "model_image_url": "https://i.imgur.com/xxx.jpg",
  "total_duration_sec": 28.54,
  "narration_script": "와 나 이거 쓴지 2주 됐는데..."
}
```

---

## 노드 총 목록 (v3)

| # | 노드 이름 | 타입 | 상태 |
|---|----------|------|------|
| 1 | Form Trigger | formTrigger | 수정 (이미지 업로드 추가) |
| 2 | 제품 데이터 정리 | Set | 유지 |
| 3 | 상세페이지 URL 체크 | IF | 유지 |
| 4 | 상세페이지 크롤링 | HTTP Request | 유지 |
| 5 | URL 없음 - 데이터 전달 | Code | 유지 |
| 6 | 페이지+데이터 합치기 | Code | 유지 |
| 7 | 제품 분석 AI | Agent | 유지 |
| 8 | 분석 결과 파싱 | Code | 유지 |
| 9 | 나레이션 대본 작성 AI | Agent | 유지 |
| 10 | 대본 파싱 | Code | 유지 |
| 11 | 모델 이미지 체크 | IF | 유지 |
| 12 | AI 모델 이미지 생성 (Flux Kontext) | HTTP Request | 유지 |
| 13 | 기존 이미지 세팅 | Set | 유지 |
| 14 | 이미지 생성 대기 40초 | Wait | 유지 |
| 15 | 이미지 결과 조회 | HTTP Request | 유지 |
| 16 | 이미지 URL 추출 | Code | 유지 |
| 17 | **스크립트 세그먼트 분할** | Code | **신규** |
| 18 | **TTS 세그먼트 순차 루프** | SplitInBatches | **신규** |
| 19 | **세그먼트별 TTS 생성** | HTTP Request (Typecast) | **신규** (기존 TTS 노드 대체) |
| 20 | **세그먼트 오디오 업로드** | HTTP Request (tmpfiles) | **신규** (기존 업로드 노드 대체) |
| 21 | **이미지+오디오 합치기** | Code | **수정** |
| 22 | **InfiniteTalk 호출** | HTTP Request (Kie.ai) | **루프 내 유지** |
| 23 | **토킹 영상 대기 120초** | Wait | **루프 내 유지** |
| 24 | **토킹 영상 결과 조회** | HTTP Request | **루프 내 유지** |
| 25 | **토킹 클립 URL 수집** | Code | **신규** |
| 26 | **TTS 루프 종료 체크** | SplitInBatches (done 분기) | **신규** |
| 27 | B-Roll 프롬프트 5개 생성 | Code | 유지 |
| 28 | B-Roll 순차 생성 루프 | SplitInBatches | 유지 |
| 29 | B-Roll 생성 (Veo 3.1) | HTTP Request | 유지 |
| 30 | B-Roll 대기 120초 | Wait | 유지 |
| 31 | B-Roll 결과 조회 | HTTP Request | 유지 |
| 32 | **최종 결과 집계** | Code | **신규** |

**신규 노드: 6개** / 수정 노드: 2개 / 유지: 24개

---

## 실행 순서 타임라인 (예상)

| 시간 | 작업 |
|------|------|
| 0:00 | Form 제출 |
| 0:01 | 데이터 정리 + URL 체크 |
| 0:05 | 크롤링 완료 |
| 0:10 | 제품 분석 AI 완료 |
| 0:15 | 나레이션 대본 AI 완료 |
| 0:16 | 병렬 시작: 이미지 생성 + TTS 5분할 시작 |
| 0:20 | TTS 세그먼트 1 완료 → 업로드 → InfiniteTalk #1 시작 |
| 0:25 | TTS 세그먼트 2 완료 → 업로드 → InfiniteTalk #2 시작 |
| 0:30 | TTS 세그먼트 3~5 순차 완료 |
| 2:30 | InfiniteTalk #1 결과 회수 |
| 4:00 | InfiniteTalk #2~5 결과 회수 |
| 4:10 | B-Roll 5개 생성 시작 |
| 6:10 | B-Roll 5개 결과 회수 |
| 6:15 | **최종 결과 출력** |

**총 예상 소요: 약 6~8분**

---

## 제품 이미지 업로드 활용 방안

Form에서 업로드된 제품 이미지는:
1. **B-Roll 프롬프트에 반영**: 제품 외형(색상, 형태, 질감)을 AI가 묘사에 활용
2. **향후 확장**: Flux Kontext에 제품 이미지를 Reference로 넣어 제품이 포함된 장면 생성

---

## 즉시 실행 가능한 액션 플랜

### Step 1: 스크립트 세그먼트 분할 Code 노드 추가
- 대본 파싱 노드 뒤에 연결
- 5개 아이템 출력

### Step 2: TTS 루프 구조 변경
- 기존 단일 TTS 노드 → SplitInBatches + 세그먼트별 TTS + 업로드 + InfiniteTalk

### Step 3: InfiniteTalk를 루프 안으로 이동
- 각 세그먼트마다: TTS → 업로드 → InfiniteTalk POST → 대기 → 폴링 → 결과 수집

### Step 4: 최종 결과 집계 노드 추가
- 토킹 클립 5개 URL + B-Roll 5개 URL을 하나의 JSON으로 집계

### Step 5: Form Trigger에 파일 업로드 필드 추가
- Field Type: File, Label: 제품 이미지
