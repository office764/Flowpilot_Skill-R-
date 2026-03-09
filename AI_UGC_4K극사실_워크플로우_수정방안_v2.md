# FlowPilot AI UGC 4K 극사실 워크플로우 수정방안

**작성일:** 2026-03-05
**대상:** 콘텐츠 마이닝 C.01 - Kinohi 바디미스트 AI UGC 리뷰영상 30초 v3
**목표:** 4K 극사실적 AI UGC (모공, 피부결, 잔털까지 보이는 실제 인간 수준)
**원칙:** kie.ai API Market 모델만 사용, n8n 워크플로우 기반, 외부 서비스(fal.ai 등) 불필요

---

## kie.ai API Market 사용 가능 모델 총정리

### 아바타/립싱크 모델

| 모델 | 모델 ID | 해상도 | 가격/초 | 최대 길이 | 립싱크 | 미세표정 | 특징 |
|------|---------|--------|---------|----------|--------|---------|------|
| InfiniteTalk | `infinitalk/from-audio` | 480p/720p | $0.015(480p), $0.06(720p) | 15초 | 기본 | 약함 | 현재 사용 중 |
| **Kling AI Avatar Standard** | `kling/ai-avatar-standard` | 720p | $0.04 | 15초 | 정밀 | 있음 | 감정+제스처 |
| **Kling AI Avatar Pro** | `kling/ai-avatar-pro` | **1080p** | $0.08 | 15초 | 최상위 | 풍부 | 고개+상체+표정 |

### 비디오 생성 모델 (B-Roll / 제품 인터랙션)

| 모델 | 모델 ID | 해상도 | 가격/초 | 최대 길이 | 특징 |
|------|---------|--------|---------|----------|------|
| Veo 3.1 (현재 사용) | `veo/generate` | 1080p | ~$0.09 | 8초 | 텍스트→비디오, 오디오 동기화 |
| **Kling 3.0 Standard** | `kling-3.0/video` (std) | 1080p | $0.10 | 15초 | 이미지→비디오, 텍스트→비디오 |
| **Kling 3.0 Pro** | `kling-3.0/video` (pro) | **4K** | $0.135 | 15초 | **4K 네이티브**, 최고 품질 |
| Seedance 2.0 I2V | `bytedance/seedance-2-image-to-video` | 1080p~2K | $0.10~0.80 | 15초 | 레퍼런스 기반, 물리 사실적 |
| Seedance 2.0 T2V | `bytedance/seedance-2-text-to-video` | 1080p~2K | $0.10~0.80 | 15초 | 텍스트→비디오, 오디오 동기화 |

### 이미지 생성 모델

| 모델 | 용도 | 해상도 | 가격 |
|------|------|--------|------|
| Flux Kontext (현재) | 캐릭터 이미지 | 1024px+ | ~$0.05/장 |
| Seedream 4.0 | **4K 이미지** | 4096px | 미확인 |
| Midjourney V7 | 스타일라이즈 | 고해상도 | 미확인 |

---

# 방안 A: 기존 워크플로우 수정 (구조 유지)

## 수정 개요

현재 워크플로우의 노드 연결 구조를 그대로 유지하면서, 핵심 API 호출 부분만 교체하는 방식.

## 수정 대상 노드 목록 (총 4곳)

### 수정 1: `토킹 클립 데이터 정리` Code 노드 — model ID 변경

**위치:** 워크플로우 중간부 (이미지+세그먼트 합치기 → 토킹 클립 데이터 정리 → 토킹 클립 순차 생성 루프)

**현재 코드 (infinitetalk_body 생성 부분):**
```javascript
const infinitetalkBody = {
  model: "infinitalk/from-audio",
  input: {
    image_url: modelImageUrl || "",
    audio_url: audioMap[i] || "",
    prompt: dir.infinitetalk_prompt || "",
    resolution: "720p"
  }
};
```

**변경 후 코드:**
```javascript
const infinitetalkBody = {
  model: "kling/ai-avatar-pro",
  input: {
    image_url: modelImageUrl || "",
    audio_url: audioMap[i] || "",
    prompt: dir.infinitetalk_prompt || ""
  }
};
```

**변경 내용:**
- `model`: `"infinitalk/from-audio"` → `"kling/ai-avatar-pro"` (1080p 고품질 아바타)
- `resolution: "720p"` 삭제 (Kling Avatar Pro는 기본 1080p 출력, 해상도 파라미터 없음)

**나머지 코드:** 전부 그대로 유지. `infinitetalkBody`를 JSON.stringify 하는 부분, `results.push` 하는 부분 전부 동일.

---

### 수정 2: `토킹 아바타 생성 (InfiniteTalk)` HTTP Request 노드 — 변경 없음

**현재 설정:**
- Method: POST
- URL: `https://api.kie.ai/api/v1/generate`
- Header: `Authorization: Bearer {kie.ai API Key}`
- Body: `={{ $json.infinitetalk_body }}`

**변경 사항: 없음.** kie.ai API 엔드포인트는 모든 모델이 동일하게 `https://api.kie.ai/api/v1/generate`를 사용한다. Body 안의 `model` 필드만 달라지면 되고, 그건 수정 1에서 이미 처리했다.

다만, 노드 이름을 `토킹 아바타 생성 (Kling Avatar Pro)`로 변경하면 가독성이 좋아진다. (선택사항)

---

### 수정 3: B-Roll 생성 모델 업그레이드 (Veo 3.1 → Kling 3.0 Pro 4K)

**위치:** B-Roll 생성 파이프라인 (B-Roll 세그먼트 순차 루프 → B-Roll 비디오 생성)

**현재 B-Roll 생성 코드에서 `veo_body` 생성 부분:**
```javascript
const veoBody = {
  model: "veo/generate",
  input: {
    prompt: brollPrompt,
    resolution: "720p"
  }
};
```

**변경 후 코드:**
```javascript
const veoBody = {
  model: "kling-3.0/video",
  input: {
    prompt: brollPrompt,
    mode: "pro",
    aspect_ratio: "9:16",
    duration: 5
  }
};
```

**변경 내용:**
- `model`: `"veo/generate"` → `"kling-3.0/video"` (4K 네이티브 지원)
- `mode: "pro"` 추가 (4K 출력 모드)
- `aspect_ratio: "9:16"` 추가 (인스타 릴스 세로형)
- `duration: 5` 추가 (5초 클립)
- `resolution` 제거 (mode: pro로 자동 최고 해상도)

**주의:** B-Roll 결과 폴링 노드의 응답 JSON 구조가 Veo 3.1과 Kling 3.0에서 다를 수 있다. 테스트 후 `extractVideoUrl` 함수에서 Kling 3.0 응답 경로 추가 필요.

---

### 수정 4: Director AI 서브에이전트 프롬프트 — 극사실주의 강화

**위치:** `Code in JavaScript1` (Director AI 메인 에이전트) 안의 서브에이전트 System Prompt

**현재 `infinitetalk_prompt` 생성 지시:**
기존 프롬프트는 일반적인 감정/동작 묘사 수준.

**변경 후 — 모든 `infinitetalk_prompt`에 아래 극사실주의 블록 필수 삽입 지시 추가:**

서브에이전트 System Prompt의 `infinitetalk_prompt` 작성 규칙에 다음 내용 추가:

```
MANDATORY HYPER-REALISM BLOCK FOR EVERY infinitetalk_prompt:
Every infinitetalk_prompt MUST end with the following block, no exceptions:

"hyper-realistic footage indistinguishable from real camera capture, ultra-detailed skin with visible pores and micro expressions and natural skin texture movement during motion, realistic hair physics with individual strands responding to movement and gravity naturally, natural subtle body micro-movements like breathing pulse and muscle tension shifts, realistic eye movement with natural blink rate and eye moisture and pupil dilation, natural motion blur consistent with real camera shutter speed, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, ZERO robotic stiff movement, must pass as genuine footage from professional cinema camera"
```

**추가로, `broll_prompt` 작성 규칙에도 극사실주의 블록 삽입:**

```
MANDATORY HYPER-REALISM BLOCK FOR EVERY broll_prompt:
Every broll_prompt MUST end with:

"shot on Sony A7R V with Sony FE 85mm f/1.4 GM lens at f/2.0, ISO 400, 5600K color temperature, hyper-realistic photorealistic footage indistinguishable from real DSLR capture, ultra-detailed material textures with visible surface micro-texture and natural light interaction, realistic shadow falloff with natural ambient occlusion, natural film grain, absolutely ZERO CGI render look, ZERO 3D modeling aesthetic, must pass as genuine footage from professional camera"
```

---

## 수정 전/후 비용 비교

### 영상 1개 (30초) 기준

| 항목 | 현재 (AS-IS) | 수정 후 (TO-BE) | 차이 |
|------|-------------|----------------|------|
| **아바타 (토킹 클립)** | InfiniteTalk 720p × 5클립 × 5초 = $1.50 | Kling Avatar Pro 1080p × 5클립 × 5초 = **$2.00** | +$0.50 |
| **B-Roll 영상** | Veo 3.1 × 5클립 × 5초 = ~$2.25 | Kling 3.0 Pro 4K × 5클립 × 5초 = **$3.38** | +$1.13 |
| **TTS** | Typecast = ~$0.30 | Typecast (유지) = ~$0.30 | $0 |
| **이미지** | Flux Kontext = ~$0.25 | Flux Kontext (유지) = ~$0.25 | $0 |
| **총 비용** | **~$4.30** | **~$5.93** | **+$1.63** |

**영상 1개당 약 2,100원 추가.** 클라이언트 청구 대비 원가율 변동은 미미.

---

## 수정 작업 순서

1. `토킹 클립 데이터 정리` Code 노드에서 model ID 1줄 변경
2. B-Roll 생성 Code 노드에서 veo_body → kling_body로 변경
3. Director AI 서브에이전트 프롬프트에 극사실주의 블록 추가
4. 테스트 실행 1회 → 결과 폴링 응답 구조 확인 → extractVideoUrl 수정 (필요시)
5. 최종 영상 품질 확인

**예상 소요 시간:** 워크플로우 수정 자체는 30분 이내. 테스트 및 디버깅 포함 2~3시간.

---

## 방안 A의 한계

이 방식으로는 다음을 달성할 수 없다:

1. **제품 인터랙션 장면 (사람이 제품 사용하는 모습):** 현재 워크플로우 구조상 "토킹 클립"과 "B-Roll"로 이원화되어 있어, "사람이 미스트를 뿌리는" 장면을 생성할 파이프라인이 없음
2. **캐릭터 일관성:** Flux Kontext으로 매번 이미지를 새로 생성하면 같은 사람이 아닌 다른 사람이 나올 수 있음
3. **자막/트랜지션/BGM:** 영상 후처리 파이프라인이 없음
4. **진짜 4K 극사실주의:** Kling Avatar Pro는 1080p까지만 지원. B-Roll만 4K. 토킹 클립은 1080p가 최대.

이 한계를 극복하려면 **방안 B (신규 워크플로우)**가 필요하다.

---
---
---

# 방안 B: 신규 워크플로우 완전 새로 구축

## 워크플로우명: `콘텐츠 마이닝 C.02 - AI UGC 4K 극사실 리뷰영상 v1`

## 핵심 차별점 (C.01 대비)

| 항목 | C.01 (현재) | C.02 (신규) |
|------|-----------|-----------|
| 세그먼트 타입 | 토킹 / B-Roll (2종) | 토킹 / B-Roll 제품 / B-Roll 인터랙션 / B-Roll 라이프스타일 (4종) |
| 아바타 모델 | InfiniteTalk 720p | **Kling Avatar Pro 1080p** |
| B-Roll 모델 | Veo 3.1 720p | **Kling 3.0 Pro 4K** |
| 제품 인터랙션 | 불가능 | **Kling 3.0 Pro I2V (합성 키프레임 → 영상)** |
| 캐릭터 일관성 | 없음 (매번 새 생성) | **Flux Kontext 시드 고정 + 레퍼런스 이미지 재사용** |
| 자막 | 없음 | **FFmpeg 자동 자막 삽입** |
| 트랜지션 | 단순 연결 | **크로스페이드 0.3초** |
| BGM | 없음 | **Suno V4.5 자동 BGM + FFmpeg 믹싱** |
| 최종 해상도 | 720p | **토킹: 1080p / B-Roll: 4K → 최종 출력: 1080p (4K 다운스케일)** |
| 후처리 | 없음 | **색보정 LUT + 인트로/아웃트로 CTA** |

---

## 전체 워크플로우 아키텍처

```
[Phase 0] 입력
  Form Trigger → 기존 이미지 세팅

[Phase 1] Director AI (스크립트 기획)
  Director AI 메인 에이전트 + 5개 서브에이전트
  → 출력: 6~8개 세그먼트 (타입별 분류)

[Phase 2] 사전 에셋 생성 (병렬)
  ├─ [2A] 캐릭터 이미지 생성 (Flux Kontext, 시드 고정)
  ├─ [2B] TTS 음성 생성 (Typecast 또는 ElevenLabs)
  └─ [2C] BGM 생성 (Suno V4.5, 30초, 장르 자동 선택)

[Phase 3] 세그먼트별 영상 생성 (순차 루프)
  SplitInBatches 루프 {
    IF 세그먼트 타입 분기:
    ├─ TYPE = "TALKING" → [3A] Kling Avatar Pro (1080p 토킹 클립)
    ├─ TYPE = "BROLL_PRODUCT" → [3B] Kling 3.0 Pro T2V (4K 제품 B-Roll)
    ├─ TYPE = "BROLL_INTERACTION" → [3C] Kling 3.0 Pro I2V (4K 인터랙션)
    └─ TYPE = "BROLL_LIFESTYLE" → [3D] Kling 3.0 Pro T2V (4K 라이프스타일)
  }
  → 결과 폴링 → 결과 집계 (staticData)

[Phase 4] 후처리 & 최종 편집
  ├─ [4A] 자막 SRT 생성 (Code 노드, TTS 텍스트 기반)
  ├─ [4B] 클립 다운로드 (HTTP Request × N)
  ├─ [4C] FFmpeg 영상 편집 (Code 노드 → Execute Command)
  │   ├─ 클립 해상도 통일 (4K → 1080p 다운스케일)
  │   ├─ 클립 간 크로스페이드 트랜지션 (0.3초)
  │   ├─ 자막 오버레이 (ASS 포맷)
  │   ├─ BGM 믹싱 (배경 -15dB, 보이스 0dB)
  │   ├─ 색보정 LUT 적용 (시네마틱 통일감)
  │   └─ 인트로(1초) + 아웃트로 CTA(3초) 삽입
  └─ [4D] 최종 MP4 출력

[Phase 5] 업로드
  Google Drive 업로드 → 완료 알림
```

---

## 노드별 상세 설계

### Phase 0: 입력

**노드 1: Form Trigger**
- 기존과 동일
- 추가 필드: `영상 스타일` (드롭다운: 리뷰/언박싱/튜토리얼/비포애프터)
- 추가 필드: `BGM 분위기` (드롭다운: 잔잔한/신나는/감성적/없음)

**노드 2: 기존 이미지 세팅 (Set 노드)**
- 기존과 동일 (제품 이미지 URL, AI 모델 이미지 URL 등)

---

### Phase 1: Director AI

**노드 3: Director AI 메인 에이전트 (AI Agent 노드)**
- 기존 C.01과 동일한 5개 서브에이전트 구조
- **변경점:** 출력 JSON 스키마에 `segment_type` 필드 추가

**Director AI 출력 JSON (세그먼트당):**
```json
{
  "timeline_id": 1,
  "section": "hook",
  "segment_type": "TALKING",
  "time_range": "0:00-0:05",
  "tts_text": "요즘 진짜 편백 향에 꽂혔거든요",
  "tts_emotion": "excited",
  "infinitetalk_prompt": "Young Korean woman looking at camera with excited expression, slight head tilt, natural smile forming, warm indoor lighting, hyper-realistic footage indistinguishable from real camera capture, ultra-detailed skin with visible pores and micro expressions...",
  "broll_prompt": "",
  "broll_name": "",
  "interaction_prompt": "",
  "model_image_prompt": "consistent Korean woman, same face as reference..."
}
```

**segment_type 정의:**
- `TALKING`: 아바타가 카메라를 보고 말하는 장면 → Kling Avatar Pro
- `BROLL_PRODUCT`: 제품 단독 B-Roll (클로즈업, 미스트 분사 등) → Kling 3.0 Pro T2V
- `BROLL_INTERACTION`: 사람이 제품을 사용하는 장면 → Kling 3.0 Pro I2V (합성 키프레임 입력)
- `BROLL_LIFESTYLE`: 라이프스타일 장면 (기지개, 산책 등) → Kling 3.0 Pro T2V

**서브에이전트 프롬프트 핵심 추가 규칙:**

```
SEGMENT TYPE ALLOCATION RULE:
For a 30-second UGC review video, you MUST allocate segments as follows:
- TALKING: 3 segments (total ~15 seconds) — segments 1, 3, 5 or 1, 4, 6
- BROLL_PRODUCT: 1-2 segments (total ~5-8 seconds) — product close-up, mist spray
- BROLL_INTERACTION: 1-2 segments (total ~5-8 seconds) — person applying product to skin
- BROLL_LIFESTYLE: 0-1 segment (total ~3-5 seconds) — morning routine, relaxation

Total segments: 6-8 (flexible, NOT fixed at 5)

HYPER-REALISM MANDATORY FOR ALL PROMPTS:
Every prompt (infinitetalk_prompt, broll_prompt, interaction_prompt) MUST include:
"hyper-realistic 4K footage indistinguishable from real cinema camera capture,
ultra-detailed skin with visible pores and individual hair strands and natural skin imperfections,
realistic hair physics responding to movement and gravity,
natural micro-movements like breathing and weight shifting,
shot on Arri Alexa Mini LF with Cooke Anamorphic 40mm lens,
ISO 800 natural film grain, 4500K color temperature,
absolutely ZERO AI-generated look, ZERO smooth plastic skin, ZERO uncanny valley"
```

---

### Phase 2: 사전 에셋 생성 (병렬)

**노드 4: Code in JavaScript1 (Director AI 출력 파싱)**
- 기존과 동일하지만 `segment_type` 필드 추가 파싱

**노드 5A: 캐릭터 이미지 생성 (Flux Kontext)**
- 기존과 동일
- **추가:** 시드(seed) 값을 고정하여 동일 캐릭터 반복 생성
- staticData에 `character_seed` 저장하여 전체 워크플로우에서 재사용

**노드 5B: TTS 세그먼트 순차 루프 (기존과 동일)**
- SplitInBatches → Typecast TTS 호출 → 오디오 URL 수집
- (Phase 2에서 ElevenLabs로 교체할 경우 URL/Body만 변경)

**노드 5C: BGM 생성 (Suno V4.5) — 신규 추가**
- **노드 종류:** HTTP Request
- **위치:** TTS 루프와 병렬로 실행
- **Method:** POST
- **URL:** `https://api.kie.ai/api/v1/generate`
- **Body:**
```json
{
  "model": "suno/v4.5",
  "input": {
    "prompt": "gentle ambient Korean beauty commercial background music, soft piano and strings, warm and inviting mood, 30 seconds, no vocals",
    "duration": 30
  }
}
```
- **출력:** BGM 오디오 URL → staticData에 저장

---

### Phase 3: 세그먼트별 영상 생성 (순차 루프)

**노드 6: 토킹+B-Roll 데이터 정리 (Code 노드) — 기존 `토킹 클립 데이터 정리` 확장**

기존 코드에 `segment_type`별 body를 각각 다르게 생성하는 로직 추가:

```javascript
const mergeItems = $input.all();
const directorItems = $('Code in JavaScript1').all();
const directorMap = {};
for (const item of directorItems) {
  directorMap[item.json.timeline_id] = item.json;
}

const audioMap = {};
let imageUrl = '';
let modelImageUrl = '';

for (const item of mergeItems) {
  if (!imageUrl) {
    const candidates = [
      item.json.image_url,
      item.json.image_main_url,
      item.json.product_image_sub,
      item.json.product_image_main
    ];
    for (const val of candidates) {
      if (val && String(val).startsWith('http')) {
        imageUrl = val;
        break;
      }
    }
  }
  if (!modelImageUrl) {
    const val = item.json['AI 모델 이미지 URL'];
    if (val && String(val).startsWith('http')) {
      modelImageUrl = val;
    }
  }
  const segId = item.json.segment_id;
  if (segId && item.json.audio_url) {
    audioMap[segId] = item.json.audio_url;
  }
}

const totalSegments = Object.keys(directorMap).length;
const results = [];

for (let i = 1; i <= totalSegments; i++) {
  const dir = directorMap[i] || {};
  const segType = dir.segment_type || 'TALKING';
  let generateBody = {};

  if (segType === 'TALKING') {
    generateBody = {
      model: "kling/ai-avatar-pro",
      input: {
        image_url: modelImageUrl || "",
        audio_url: audioMap[i] || "",
        prompt: dir.infinitetalk_prompt || ""
      }
    };
  } else if (segType === 'BROLL_PRODUCT') {
    generateBody = {
      model: "kling-3.0/video",
      input: {
        prompt: dir.broll_prompt || "",
        mode: "pro",
        aspect_ratio: "9:16",
        duration: 5
      }
    };
  } else if (segType === 'BROLL_INTERACTION') {
    generateBody = {
      model: "kling-3.0/video",
      input: {
        prompt: dir.interaction_prompt || "",
        image_url: modelImageUrl || "",
        mode: "pro",
        aspect_ratio: "9:16",
        duration: 5
      }
    };
  } else if (segType === 'BROLL_LIFESTYLE') {
    generateBody = {
      model: "kling-3.0/video",
      input: {
        prompt: dir.broll_prompt || "",
        mode: "pro",
        aspect_ratio: "9:16",
        duration: 5
      }
    };
  }

  results.push({
    json: {
      segment_id: i,
      section: dir.section || '',
      segment_type: segType,
      text: dir.tts_text || '',
      tts_text: dir.tts_text || '',
      tts_emotion: dir.tts_emotion || '',
      time_range: dir.time_range || '',
      audio_url: audioMap[i] || '',
      image_main_url: imageUrl,
      model_image_url: modelImageUrl,
      generate_body: JSON.stringify(generateBody),
      broll_prompt: dir.broll_prompt || '',
      broll_name: dir.broll_name || '',
      infinitetalk_prompt: dir.infinitetalk_prompt || '',
      interaction_prompt: dir.interaction_prompt || '',
      brand: dir.brand || '',
      product: dir.product || '',
      price: dir.price || '',
      product_image_main: dir.product_image_main || '',
      product_image_sub: dir.product_image_sub || ''
    }
  });
}

return results;
```

**핵심 변경:** 기존에는 `infinitetalk_body`와 `veo_body`를 별도로 만들었지만, 신규에서는 `segment_type`에 따라 `generate_body` 하나로 통합. 이로써 순차 루프에서 하나의 HTTP Request 노드로 모든 타입의 영상을 생성할 수 있다.

---

**노드 7: 영상 세그먼트 순차 생성 루프 (SplitInBatches)**
- 기존 토킹 루프 + B-Roll 루프를 **하나의 루프로 통합**
- 매 세그먼트마다 `generate_body`에 따라 적절한 모델이 자동으로 호출됨

**노드 8: 영상 생성 (HTTP Request) — 통합 노드**
- **Method:** POST
- **URL:** `https://api.kie.ai/api/v1/generate`
- **Header:** `Authorization: Bearer {{ $credentials.kieApiKey }}`
- **Body:** `={{ $json.generate_body }}`
- 이 하나의 노드로 Kling Avatar Pro(토킹), Kling 3.0 Pro(B-Roll), Kling 3.0 Pro I2V(인터랙션) 모두 처리

**노드 9: 결과 폴링 (Wait + HTTP Request)**
- 기존 폴링 패턴과 동일
- `task_id`를 받아서 상태 체크 후 완료 시 비디오 URL 추출

**노드 10: 결과 집계 (Code 노드)**
- 기존 `최종 결과 집계`와 동일한 패턴
- `$getWorkflowStaticData('global')`에 세그먼트별 결과 누적
- 모든 세그먼트 완료 후 다음 Phase로 이동

---

### Phase 4: 후처리 & 최종 편집 (신규 파이프라인)

**노드 11: 자막 SRT 생성 (Code 노드) — 신규**
- Director AI가 생성한 `tts_text`와 `time_range`를 기반으로 SRT 포맷 자막 파일 생성
- staticData에 SRT 문자열 저장

```javascript
const segments = $input.all();
let srt = '';
let index = 1;
for (const seg of segments) {
  const timeRange = seg.json.time_range || '0:00-0:05';
  const parts = timeRange.split('-');
  const start = parts[0].trim().replace(':', ':00:') + ',000';
  const end = parts[1].trim().replace(':', ':00:') + ',000';
  srt += `${index}\n00:${start} --> 00:${end}\n${seg.json.tts_text || ''}\n\n`;
  index++;
}
const staticData = $getWorkflowStaticData('global');
staticData.srt_content = srt;
return [{ json: { srt: srt, status: 'srt_generated' } }];
```

**노드 12: 클립 다운로드 (Code 노드 → Execute Command) — 신규**
- staticData에서 각 세그먼트의 비디오 URL을 추출
- n8n 서버의 임시 디렉토리에 다운로드

```javascript
const staticData = $getWorkflowStaticData('global');
const allResults = staticData.all_results || [];
const downloadCommands = [];
for (let i = 0; i < allResults.length; i++) {
  const url = allResults[i].video_url || '';
  if (url) {
    downloadCommands.push(`curl -L -o /tmp/clip_${i+1}.mp4 "${url}"`);
  }
}
return [{ json: { commands: downloadCommands.join(' && '), clip_count: allResults.length } }];
```

**노드 13: Execute Command (다운로드 실행)**
- **노드 종류:** Execute Command
- **Command:** `={{ $json.commands }}`

**노드 14: FFmpeg 최종 편집 (Execute Command) — 신규**
- 모든 클립을 1080p로 통일, 크로스페이드 트랜지션, 자막 삽입, BGM 믹싱

**FFmpeg 명령어 (Code 노드에서 생성):**
```javascript
const staticData = $getWorkflowStaticData('global');
const clipCount = staticData.all_results.length;
const srt = staticData.srt_content || '';
const bgmUrl = staticData.bgm_url || '';

// SRT 파일 저장
const fs = require('fs');
fs.writeFileSync('/tmp/subtitle.srt', srt);

// 클립 리스트 파일 생성
let fileList = '';
for (let i = 1; i <= clipCount; i++) {
  fileList += `file '/tmp/clip_${i}.mp4'\n`;
}
fs.writeFileSync('/tmp/clips.txt', fileList);

// BGM 다운로드 명령
const bgmCmd = bgmUrl ? `curl -L -o /tmp/bgm.mp3 "${bgmUrl}" && ` : '';

// FFmpeg 명령어 조합
const ffmpegCmd = `${bgmCmd}ffmpeg -y -f concat -safe 0 -i /tmp/clips.txt -vf "scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2,subtitles=/tmp/subtitle.srt:force_style='FontName=NanumGothic,FontSize=24,PrimaryColour=&H00FFFFFF,OutlineColour=&H00000000,Outline=2,MarginV=80'" ${bgmUrl ? '-i /tmp/bgm.mp3 -filter_complex "[0:a]volume=1.0[voice];[1:a]volume=0.15[bgm];[voice][bgm]amix=inputs=2:duration=first[aout]" -map 0:v -map "[aout]"' : ''} -c:v libx264 -preset medium -crf 18 -c:a aac -b:a 192k /tmp/final_output.mp4`;

return [{ json: { command: ffmpegCmd } }];
```

**노드 15: Execute Command (FFmpeg 실행)**
- **Command:** `={{ $json.command }}`
- 출력: `/tmp/final_output.mp4`

---

### Phase 5: 업로드

**노드 16: 최종 영상 Read Binary (Move Binary Data)**
- `/tmp/final_output.mp4` 파일을 바이너리 데이터로 읽기

**노드 17: Google Drive 업로드**
- 기존 C.01과 동일
- 단, 이제 **1개 파일만 업로드** (클립이 아니라 편집 완료된 최종 영상)
- 파일명: `{브랜드}_{제품}_{날짜}_AI_UGC_4K.mp4`

---

## 신규 워크플로우 비용 분석

### 영상 1개 (30초, 8세그먼트) 기준

| 항목 | 모델 | 수량 | 단가 | 소계 |
|------|------|------|------|------|
| **토킹 클립** | Kling Avatar Pro (1080p) | 3클립 × 5초 | $0.08/초 | $1.20 |
| **B-Roll 제품** | Kling 3.0 Pro (4K) | 2클립 × 5초 | $0.135/초 | $1.35 |
| **B-Roll 인터랙션** | Kling 3.0 Pro I2V (4K) | 2클립 × 5초 | $0.135/초 | $1.35 |
| **B-Roll 라이프스타일** | Kling 3.0 Pro (4K) | 1클립 × 5초 | $0.135/초 | $0.68 |
| **TTS** | Typecast | 30초 | ~$0.01/초 | $0.30 |
| **BGM** | Suno V4.5 | 1곡 30초 | ~$0.20 | $0.20 |
| **이미지** | Flux Kontext | 3장 | ~$0.05/장 | $0.15 |
| **총 비용** | | | | **$5.23** |

**현재 C.01: ~$4.30/영상 → 신규 C.02: ~$5.23/영상 (+$0.93, +22%)**

$0.93 추가(약 1,200원)로 4K B-Roll + 1080p 토킹 + 제품 인터랙션 + 자막 + BGM + 트랜지션이 전부 포함된다.

---

## 방안 A vs 방안 B 비교

| 항목 | 방안 A (수정) | 방안 B (신규) |
|------|-------------|-------------|
| **작업량** | 코드 4곳 수정 (30분) | 워크플로우 신규 구축 (2~3일) |
| **아바타 품질** | Kling Avatar Pro 1080p (대폭 향상) | 동일 |
| **B-Roll 품질** | Kling 3.0 Pro 4K (대폭 향상) | 동일 |
| **제품 인터랙션** | 불가능 | **가능 (I2V 파이프라인)** |
| **자막** | 없음 | **자동 삽입** |
| **BGM** | 없음 | **Suno V4.5 자동 생성** |
| **트랜지션** | 단순 연결 | **크로스페이드** |
| **색보정** | 없음 | **LUT 적용** |
| **최종 업로드** | 클립 5개 개별 업로드 | **편집 완성본 1개 업로드** |
| **비용** | $5.93/영상 | $5.23/영상 (더 저렴) |
| **리스크** | 낮음 (검증된 구조) | 중간 (FFmpeg 디버깅 필요) |

---

## 추천

**즉시 실행:** 방안 A 먼저 적용 (30분 소요, 리스크 낮음)
**이후 병행:** 방안 B를 별도 워크플로우로 구축 (C.01은 유지, C.02를 새로 만듦)
**테스트 후:** C.02 품질이 확인되면 C.01을 비활성화하고 C.02로 전환

대표님이 "A 먼저 해라" 또는 "B부터 해라" 또는 "둘 다 해라" 결정만 내려주면 바로 착수한다.
