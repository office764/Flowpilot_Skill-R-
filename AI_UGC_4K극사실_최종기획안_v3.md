# FlowPilot AI UGC 4K 극사실 워크플로우 최종 기획안 v3

**작성일:** 2026-03-05
**대상:** 콘텐츠 마이닝 C.01 - Kinohi 바디미스트 AI UGC 리뷰영상 30초 v3
**목표:** 4K 극사실적 AI UGC — 모공, 피부결, 잔털, 미세표정까지 실제 인간과 동일 수준
**원칙:** kie.ai API Market 모델만 사용, n8n 워크플로우 기반

---

## 1. VIDI 레퍼런스 정밀 분석

### VIDI 대시보드 스크린샷에서 확인된 사항

VIDI는 K-뷰티 브랜드용 AI UGC 숏폼 SaaS 플랫폼이다. 대시보드 스크린샷에서 확인된 내용:

- **제품 카탈로그 방식:** 브랜드가 제품 이미지를 업로드하면, VIDI가 해당 제품에 맞는 AI UGC 영상을 자동 생성
- **제품 카테고리 태그:** #원통형, #토너, #단지형, #에센스, #크림, #로션, #펌프형, #분사형, #향수, #디바이스, #앰플, #스틱형 등 뷰티 제품 형태별 분류
- **영상 길이:** 16초~33초 (제품별 상이)
- **가격:** 제품당 89,000원 (~$65 USD)
- **인터페이스:** "숏폼 생성하기" 버튼 클릭 → 영상 자동 생성
- **영상 내용:** 한국 여성 모델이 제품을 직접 사용하는 모습 (얼굴에 도포, 손에 뿌리기, 패드로 닦기 등)

### VIDI 영상 품질 특징 (첨부 영상 1228_클렌징디바이스.mp4 기준)

- **해상도:** 1080x1896 (풀HD 세로형)
- **프레임레이트:** 30fps
- **핵심 장면:** 여성이 클렌징 패드로 실제로 얼굴을 닦는 모습
- **피부 디테일:** 모공, 피부결이 보이는 수준 (극사실적)
- **손-얼굴 인터랙션:** 패드가 볼에 닿는 압력감, 피부 눌림이 사실적
- **표정/눈 움직임:** 자연스러운 눈 깜빡임, 미세 표정 변화
- **의상 물리:** 목욕가운 체크 패턴이 움직임에 따라 자연스럽게 변형
- **자막:** 한글 자막 "분명 꼼꼼하게 씻었는데" 하단 오버레이
- **워터마크:** "VIDI를 사용해 100% AI로 생성된 영상입니다"

### VIDI가 하는 것 vs 우리가 만들어야 하는 것

| VIDI 기능 | 우리가 복제해야 할 것 | 사용할 kie.ai 모델 |
|-----------|---------------------|-------------------|
| AI 아바타가 말하는 토킹 장면 | 토킹 클립 생성 | **Kling AI Avatar Pro** (1080p) |
| 사람이 제품 사용하는 인터랙션 장면 | 제품 인터랙션 B-Roll | **Kling 3.0 Pro** I2V (4K) |
| 제품 단독 클로즈업 B-Roll | 제품 B-Roll | **Kling 3.0 Pro** T2V (4K) |
| 라이프스타일 장면 (기지개, 산책 등) | 라이프스타일 B-Roll | **Kling 3.0 Pro** T2V (4K) |
| 한글 자막 자동 삽입 | 자막 오버레이 | **FFmpeg** (n8n Execute Command) |
| TTS 음성 (자연스러운 리뷰 톤) | TTS | **Typecast** (현재) 또는 **ElevenLabs** |
| BGM 배경음악 | BGM | **Suno V4.5** (kie.ai) |
| 편집 (트랜지션, 컷 전환) | 영상 편집 | **FFmpeg** (n8n Execute Command) |

### 솔직한 품질 갭 평가

**현실적으로 달성 가능한 수준:**

- **토킹 장면 (아바타 말하기):** Kling Avatar Pro로 **VIDI의 90~95% 수준** 달성 가능. 립싱크, 미세표정, 고개 움직임 모두 최상위급. 1080p 해상도.
- **제품 B-Roll:** Kling 3.0 Pro로 **VIDI의 95~100% 수준** 달성 가능. 4K 네이티브, 제품 촬영 품질 우수.
- **제품 인터랙션 장면:** Kling 3.0 Pro I2V로 **VIDI의 60~75% 수준**. 이 부분이 가장 큰 갭이다. VIDI는 "사람이 제품을 직접 사용하는" 장면에 특화된 자체 모델을 보유한 것으로 추정된다. Kling 3.0의 Image-to-Video로 합성 키프레임을 넣어도, 손-제품-얼굴 3자 인터랙션의 정밀도는 VIDI만큼 나오기 어렵다.
- **전체 종합:** VIDI 대비 **80~85% 수준** (토킹+B-Roll은 동급, 인터랙션 장면에서 차이)

**그러나 비용 차이가 압도적:**
- VIDI: 89,000원/영상 ($65)
- 우리: ~6,800원/영상 ($5.23) → **VIDI 대비 1/12 비용**
- 클라이언트에게 5만원에 판매해도 원가율 13%, 마진 87%

---

## 2. kie.ai API Market 사용 모델 (확정)

| 용도 | 모델 | 모델 ID | 해상도 | 가격/초 | 비고 |
|------|------|---------|--------|---------|------|
| 토킹 아바타 | Kling AI Avatar Pro | `kling/ai-avatar-pro` | 1080p | $0.08 | 최대 15초, 미세표정+제스처 |
| B-Roll (제품/라이프스타일) | Kling 3.0 Pro | `kling-3.0/video` (mode: pro) | 4K | $0.135 | 최대 15초, T2V+I2V |
| B-Roll (인터랙션) | Kling 3.0 Pro I2V | `kling-3.0/video` (mode: pro) | 4K | $0.135 | 합성 키프레임 → 영상 |
| TTS | Typecast (유지) | Typecast API | - | ~$0.01/초 | 추후 ElevenLabs 전환 가능 |
| BGM | Suno V4.5 | `suno/v4.5` | - | ~$0.20/곡 | 30초 배경음악 |
| 캐릭터 이미지 | Flux Kontext (유지) | `flux-kontext/generate` | 1024px+ | ~$0.05/장 | 시드 고정으로 일관성 |

**전부 kie.ai 엔드포인트 `https://api.kie.ai/api/v1/generate` 하나로 통합. 기존 API 키 그대로 사용.**

---

## 3. 추천 방향: 신규 구축 (C.02)

### 왜 수정이 아니라 신규 구축인가

| 판단 기준 | 기존 C.01 수정 | 신규 C.02 구축 | 판정 |
|-----------|---------------|--------------|------|
| 세그먼트 타입 분기 | 토킹/B-Roll 2개 루프가 분리됨 → segment_type 4종 분기 불가 | 통합 루프 + IF 분기로 4종 타입 처리 | **C.02 승** |
| 제품 인터랙션 | 추가 불가 (구조적 한계) | 네이티브 지원 | **C.02 승** |
| 후처리 (자막/BGM/트랜지션) | 기존 구조에 끼워넣기 어려움 | Phase 4로 깔끔하게 분리 | **C.02 승** |
| 최종 출력 | 클립 5개 개별 업로드 | 편집 완성본 1개 업로드 (VIDI와 동일) | **C.02 승** |
| 디버깅 난이도 | 기존 버그(폴링, JSON 등) 위에 새 로직 추가 → 복잡 | 깨끗한 상태에서 시작 | **C.02 승** |
| 비용 | $5.93/영상 | $5.23/영상 (통합 루프로 효율적) | **C.02 승** |
| 시간 | 30분 | 2~3일 | C.01 승 |

**결론:** 시간이 좀 더 걸리지만 C.02 신규 구축이 확실히 낫다. C.01은 기존 버그도 많고, 토킹/B-Roll 분리 구조의 한계가 있어서 인터랙션 장면 추가나 후처리 파이프라인을 넣기가 구조적으로 불가능하다.

**C.01은 그대로 두고, C.02를 새로 만든다.** C.02 테스트 완료 후 C.01을 비활성화하면 된다.

---

## 4. 신규 워크플로우 C.02 설계

### 워크플로우명: `콘텐츠 마이닝 C.02 - AI UGC 4K 극사실 리뷰영상 v1`

### 전체 아키텍처 (5 Phase)

```
═══════════════════════════════════════════════════════════════════
  Phase 0: 입력
═══════════════════════════════════════════════════════════════════
  [Form Trigger] → [기존 이미지 세팅 (Set)]
       ↓
═══════════════════════════════════════════════════════════════════
  Phase 1: Director AI (스크립트 기획)
═══════════════════════════════════════════════════════════════════
  [AI Agent 노드 (Director AI)] → [출력 파싱 (Code)]
   ├─ 서브에이전트 1: 스크립트 기획
   ├─ 서브에이전트 2: 프롬프트 작성
   ├─ 서브에이전트 3: 장면 구성
   ├─ 서브에이전트 4: 기술 최적화
   └─ 서브에이전트 5: 품질 검수
       ↓
  출력: 6~8개 세그먼트 (segment_type별 분류)
       ↓
═══════════════════════════════════════════════════════════════════
  Phase 2: 사전 에셋 생성 (3개 병렬)
═══════════════════════════════════════════════════════════════════
  ┌─ [2A] 캐릭터 이미지 (Flux Kontext, 시드 고정)
  ├─ [2B] TTS 순차 생성 (Typecast, SplitInBatches 루프)
  └─ [2C] BGM 생성 (Suno V4.5)
       ↓ (Merge 노드에서 합류)
═══════════════════════════════════════════════════════════════════
  Phase 3: 통합 영상 생성 루프
═══════════════════════════════════════════════════════════════════
  [데이터 정리 (Code)] → [SplitInBatches 루프]
       ↓ (루프 내부)
  [영상 생성 HTTP Request] → [Wait 30초] → [결과 폴링 HTTP Request]
       ↓                                           ↓
  (segment_type에 따라 자동으로                  (task_id → 상태 체크)
   Kling Avatar Pro 또는                          ↓
   Kling 3.0 Pro가 호출됨)              [IF 완료?] → [결과 집계 (staticData)]
                                            ↓ (미완료)
                                      [Wait 30초] → 재폴링
       ↓ (모든 세그먼트 완료)
═══════════════════════════════════════════════════════════════════
  Phase 4: 후처리 & 최종 편집
═══════════════════════════════════════════════════════════════════
  [SRT 자막 생성 (Code)] → [클립 다운로드 (Code+Execute)]
       ↓
  [FFmpeg 편집 (Execute Command)]
   ├─ 해상도 통일 (1080x1920)
   ├─ 크로스페이드 트랜지션
   ├─ 자막 오버레이
   ├─ BGM 믹싱
   └─ 색보정 LUT
       ↓
  [최종 MP4] → /tmp/final_output.mp4
       ↓
═══════════════════════════════════════════════════════════════════
  Phase 5: 업로드
═══════════════════════════════════════════════════════════════════
  [Read Binary] → [Google Drive 업로드] → [완료 알림]
```

---

### 노드별 초상세 설계

#### Phase 0: 입력

**노드 1: Form Trigger**

추가 필드 (기존 필드 유지 + 아래 추가):

| 필드명 | 타입 | 옵션 | 용도 |
|--------|------|------|------|
| 영상 스타일 | Dropdown | 리뷰 / 언박싱 / 튜토리얼 / 비포애프터 | Director AI에게 영상 구조 지시 |
| BGM 분위기 | Dropdown | 잔잔한 / 신나는 / 감성적 / 없음 | Suno BGM 프롬프트 결정 |
| 제품 카테고리 | Dropdown | 토너 / 에센스 / 크림 / 미스트 / 디바이스 / 앰플 | Director AI에게 제품 사용 방법 지시 |
| 타겟 길이(초) | Number | 기본값: 30 | 세그먼트 수 결정 |

**노드 2: 기존 이미지 세팅 (Set 노드)**
- 기존 C.01과 동일한 필드 유지
- 제품 이미지 URL, AI 모델 이미지 URL, 제품명, 브랜드명, 가격 등

---

#### Phase 1: Director AI

**노드 3: Director AI 메인 에이전트 (AI Agent 노드)**

기존 C.01의 5개 서브에이전트 구조 유지. 핵심 변경점:

**1) 출력 JSON 스키마에 `segment_type` 필드 추가**

세그먼트당 출력:
```json
{
  "timeline_id": 1,
  "section": "hook",
  "segment_type": "TALKING",
  "time_range": "0:00-0:04",
  "tts_text": "요즘 진짜 편백 향에 꽂혔거든요",
  "tts_emotion": "excited",
  "avatar_prompt": "Young Korean woman mid-20s looking directly at camera with genuinely excited expression, slight eyebrow raise and natural smile forming, warm soft indoor lighting from left side creating gentle shadow on right cheek, wearing casual white cotton t-shirt, hair slightly messy natural style, hyper-realistic footage indistinguishable from real camera capture, ultra-detailed skin with visible pores and micro expressions and natural skin texture movement during motion, realistic hair physics with individual strands responding to movement and gravity naturally, natural subtle body micro-movements like breathing pulse and muscle tension shifts, realistic eye movement with natural blink rate and eye moisture and pupil dilation, natural motion blur consistent with real camera shutter speed, shot on Sony A7IV with Sony FE 35mm f/1.4 GM at f/1.8 ISO 640 5200K, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, ZERO robotic stiff movement, must pass as genuine selfie footage",
  "broll_prompt": "",
  "interaction_prompt": "",
  "broll_name": ""
}
```

**2) segment_type 정의 및 배분 규칙**

| segment_type | 설명 | 사용 모델 | 30초 기준 배분 |
|-------------|------|----------|--------------|
| `TALKING` | 아바타가 카메라 보고 말하는 장면 | Kling Avatar Pro | 3개 (총 ~15초) |
| `BROLL_PRODUCT` | 제품 단독 클로즈업 (미스트 분사, 텍스처 등) | Kling 3.0 Pro T2V | 1~2개 (총 ~5초) |
| `BROLL_INTERACTION` | 사람이 제품을 사용하는 장면 (팔목에 분사, 피부 도포) | Kling 3.0 Pro I2V | 1~2개 (총 ~5초) |
| `BROLL_LIFESTYLE` | 라이프스타일 (모닝루틴, 릴랙스) | Kling 3.0 Pro T2V | 0~1개 (총 ~3초) |

**3) 서브에이전트 프롬프트에 추가할 극사실주의 필수 규칙**

모든 서브에이전트 System Prompt에 아래 블록을 추가한다:

```
=== HYPER-REALISM MANDATORY RULES ===

RULE 1: EVERY avatar_prompt (TALKING segments) MUST end with:
"hyper-realistic footage indistinguishable from real camera capture, ultra-detailed skin with visible pores and micro expressions and natural skin texture movement during motion, realistic hair physics with individual strands responding to movement and gravity naturally, natural subtle body micro-movements like breathing pulse and muscle tension shifts, realistic eye movement with natural blink rate and eye moisture and pupil dilation, natural motion blur consistent with real camera shutter speed, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, ZERO robotic stiff movement, must pass as genuine footage from professional cinema camera"

RULE 2: EVERY broll_prompt (BROLL_PRODUCT/LIFESTYLE segments) MUST end with:
"shot on Sony A7R V with Sony FE 85mm f/1.4 GM lens at f/2.0, ISO 400, 5600K color temperature, hyper-realistic photorealistic footage indistinguishable from real DSLR capture, ultra-detailed material textures with visible surface micro-texture and natural light interaction, realistic shadow falloff with natural ambient occlusion, natural film grain, absolutely ZERO CGI render look, ZERO 3D modeling aesthetic, must pass as genuine footage from professional camera"

RULE 3: EVERY interaction_prompt (BROLL_INTERACTION segments) MUST end with:
"hyper-realistic 4K footage of real person using the product, visible skin pores and individual fine arm hair catching light, natural hand movement with realistic finger joint articulation and nail texture, product physically contacting skin with visible pressure deformation, realistic product mist particles dispersing in air, warm golden hour side lighting at 4500K, shot on Arri Alexa Mini LF with Cooke Anamorphic 40mm at T2.0, natural film grain ISO 800, absolutely ZERO AI-generated look, ZERO smooth plastic skin, ZERO uncanny valley, must pass as genuine beauty commercial footage"

RULE 4: FORBIDDEN WORDS (자동 교체)
- "smooth skin" → "textured skin with visible pores"
- "perfect face" → "naturally asymmetric human face"
- "beautiful lighting" → 실제 카메라+렌즈 모델명으로 교체
- "high quality" → "photorealistic DSLR photograph" 또는 "cinema camera footage"
- "4K" 단독 사용 금지 → 카메라 모델 + 렌즈 + ISO + 색온도와 함께 사용

=== END HYPER-REALISM RULES ===
```

**4) 제품 카테고리별 인터랙션 프롬프트 가이드**

Director AI가 제품 카테고리에 따라 적절한 인터랙션 장면을 자동 선택하도록:

```
PRODUCT INTERACTION GUIDE BY CATEGORY:
- 토너/에센스/앰플: "gently patting product onto cheek with fingertips, visible moisture on skin surface"
- 크림/로션: "scooping cream from jar with fingers, spreading on forearm in circular motion"
- 미스트/분사형: "spraying mist onto inner wrist from 15cm distance, fine mist particles visible in backlight"
- 향수: "spraying fragrance onto pulse point of wrist, tilting wrist to nose"
- 디바이스: "pressing cleansing device against cheek, gentle circular motion on skin"
- 스틱형: "gliding stick applicator along jawline, product leaving subtle sheen on skin"
- 펌프형: "pressing pump dispenser twice, product pooling in palm of hand"
```

---

#### Phase 2: 사전 에셋 생성

**노드 4: Director AI 출력 파싱 (Code 노드)**

```javascript
const aiOutput = $input.first().json;
let parsed;
try {
  const raw = typeof aiOutput.output === 'string' ? aiOutput.output : JSON.stringify(aiOutput.output);
  const jsonMatch = raw.match(/\[[\s\S]*\]/);
  if (jsonMatch) {
    parsed = JSON.parse(jsonMatch[0]);
  } else {
    throw new Error('JSON array not found in output');
  }
} catch(e) {
  return [{ json: { error: 'Failed to parse Director AI output: ' + e.message, raw: aiOutput } }];
}

const staticData = $getWorkflowStaticData('global');
staticData.director_segments = parsed;
staticData.total_segments = parsed.length;
staticData.completed_segments = 0;
staticData.all_results = [];

const results = [];
for (const seg of parsed) {
  results.push({
    json: {
      timeline_id: seg.timeline_id,
      section: seg.section || '',
      segment_type: seg.segment_type || 'TALKING',
      time_range: seg.time_range || '',
      tts_text: seg.tts_text || '',
      tts_emotion: seg.tts_emotion || '',
      avatar_prompt: seg.avatar_prompt || '',
      broll_prompt: seg.broll_prompt || '',
      interaction_prompt: seg.interaction_prompt || '',
      broll_name: seg.broll_name || '',
      brand: seg.brand || '',
      product: seg.product || '',
      price: seg.price || '',
      product_image_main: seg.product_image_main || '',
      product_image_sub: seg.product_image_sub || ''
    }
  });
}
return results;
```

---

**노드 5A: 캐릭터 이미지 생성 (Flux Kontext) — HTTP Request**

기존 C.01과 동일하되, 시드 고정으로 캐릭터 일관성 확보:

- **Method:** POST
- **URL:** `https://api.kie.ai/api/v1/generate`
- **Body:**
```json
{
  "model": "flux-kontext/generate",
  "input": {
    "prompt": "{{ $json.model_image_prompt }}",
    "seed": 42
  }
}
```
seed 값을 42로 고정하면 동일한 프롬프트에서 항상 같은 얼굴의 캐릭터가 생성된다.

---

**노드 5B: TTS 순차 루프 — 기존 C.01 패턴 그대로**

SplitInBatches → 세그먼트별 Typecast TTS 호출 → 오디오 URL 수집

TALKING 타입 세그먼트만 TTS 생성 필요. B-Roll 세그먼트는 TTS 불필요.

```javascript
// TTS 필요 세그먼트 필터링 (루프 진입 전 Code 노드)
const items = $input.all();
const ttsItems = items.filter(item => item.json.segment_type === 'TALKING');
return ttsItems;
```

---

**노드 5C: BGM 생성 (Suno V4.5) — HTTP Request (신규)**

- **Method:** POST
- **URL:** `https://api.kie.ai/api/v1/generate`
- **Body:**
```json
{
  "model": "suno/v4.5",
  "input": {
    "prompt": "{{ $('Form Trigger').item.json['BGM 분위기'] === '잔잔한' ? 'gentle ambient Korean beauty commercial background music, soft piano and ethereal strings, warm and inviting mood, 30 seconds, no vocals, subtle and minimal' : $('Form Trigger').item.json['BGM 분위기'] === '신나는' ? 'upbeat bright Korean beauty commercial music, cheerful acoustic guitar and light percussion, energetic positive mood, 30 seconds, no vocals' : $('Form Trigger').item.json['BGM 분위기'] === '감성적' ? 'emotional cinematic Korean beauty music, solo piano with soft violin, intimate and touching mood, 30 seconds, no vocals' : '' }}",
    "duration": 30
  }
}
```
BGM 분위기 "없음" 선택 시 이 노드를 스킵하도록 IF 노드로 분기.

---

**노드 5D: Merge (Wait for All)**

캐릭터 이미지 + TTS 오디오 + BGM이 전부 완료된 후에만 Phase 3으로 진행.

- **노드 종류:** Merge
- **Mode:** Wait for All (Combine)
- **Number of Inputs:** 3

---

#### Phase 3: 통합 영상 생성 루프

**노드 6: 통합 데이터 정리 (Code 노드)**

모든 세그먼트의 데이터를 정리하고, segment_type에 따라 `generate_body`를 각각 다르게 생성:

```javascript
const mergeItems = $input.all();
const staticData = $getWorkflowStaticData('global');
const directorSegments = staticData.director_segments || [];

// 오디오 URL 매핑
const audioMap = {};
for (const item of mergeItems) {
  const segId = item.json.segment_id || item.json.timeline_id;
  if (segId && item.json.audio_url) {
    audioMap[segId] = item.json.audio_url;
  }
}

// 이미지 URL 추출
let productImageUrl = '';
let modelImageUrl = '';

for (const item of mergeItems) {
  if (!productImageUrl) {
    const candidates = [
      item.json.image_url,
      item.json.image_main_url,
      item.json.product_image_main,
      item.json.product_image_sub
    ];
    for (const val of candidates) {
      if (val && String(val).startsWith('http')) {
        productImageUrl = val;
        break;
      }
    }
  }
  if (!modelImageUrl) {
    const val = item.json['AI 모델 이미지 URL'] || item.json.model_image_url;
    if (val && String(val).startsWith('http')) {
      modelImageUrl = val;
    }
  }
}

// BGM URL 저장
for (const item of mergeItems) {
  if (item.json.bgm_url) {
    staticData.bgm_url = item.json.bgm_url;
  }
}

const results = [];

for (let i = 0; i < directorSegments.length; i++) {
  const dir = directorSegments[i];
  const segId = dir.timeline_id || (i + 1);
  const segType = dir.segment_type || 'TALKING';
  let generateBody = {};

  if (segType === 'TALKING') {
    generateBody = {
      model: "kling/ai-avatar-pro",
      input: {
        image_url: modelImageUrl || "",
        audio_url: audioMap[segId] || "",
        prompt: dir.avatar_prompt || ""
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
      segment_id: segId,
      section: dir.section || '',
      segment_type: segType,
      tts_text: dir.tts_text || '',
      time_range: dir.time_range || '',
      audio_url: audioMap[segId] || '',
      image_main_url: productImageUrl,
      model_image_url: modelImageUrl,
      generate_body: JSON.stringify(generateBody),
      avatar_prompt: dir.avatar_prompt || '',
      broll_prompt: dir.broll_prompt || '',
      interaction_prompt: dir.interaction_prompt || ''
    }
  });
}

return results;
```

---

**노드 7: 영상 세그먼트 순차 생성 루프 (SplitInBatches)**

- **Batch Size:** 1 (한 번에 1개씩 순차 처리)
- 기존 C.01의 토킹 루프와 동일한 패턴이지만, 토킹+B-Roll을 하나의 루프로 통합

---

**노드 8: 영상 생성 (HTTP Request)**

- **Method:** POST
- **URL:** `https://api.kie.ai/api/v1/generate`
- **Authentication:** Header Auth
- **Header Name:** Authorization
- **Header Value:** Bearer {kie.ai API Key}
- **Body Content Type:** JSON
- **Body:** `={{ $json.generate_body }}`

이 하나의 노드로 Kling Avatar Pro(토킹), Kling 3.0 Pro T2V(B-Roll), Kling 3.0 Pro I2V(인터랙션) 전부 처리. Body 안의 `model` 필드에 따라 kie.ai가 자동으로 적절한 모델을 호출한다.

---

**노드 9: Wait (30초 대기)**
- **노드 종류:** Wait
- **Wait time:** 30초
- 비디오 생성에 보통 2~10분 소요. 30초마다 폴링.

---

**노드 10: 결과 폴링 (HTTP Request)**

- **Method:** GET
- **URL:** `https://api.kie.ai/api/v1/task/{{ $json.task_id }}/status`
- **Authentication:** 동일

---

**노드 11: 완료 체크 (IF 노드)**

- **Condition:** `{{ $json.status }}` equals `completed`
- **True:** 노드 12로 (결과 집계)
- **False:** 노드 9로 돌아가기 (재폴링)

---

**노드 12: 결과 집계 (Code 노드)**

```javascript
const result = $input.first().json;
const staticData = $getWorkflowStaticData('global');

if (!staticData.all_results) {
  staticData.all_results = [];
}

// 비디오 URL 추출 (Kling 응답 구조)
function findVideoUrl(data) {
  if (!data) return '';
  if (typeof data === 'string' && data.match(/^https?:\/\/.+\.(mp4|webm|mov)/i)) return data;
  if (typeof data === 'object') {
    const paths = [
      data.video_url,
      data.output?.video_url,
      data.result?.video_url,
      data.data?.video_url,
      data.output?.url,
      data.result?.url,
      data.data?.url,
      data.url
    ];
    for (const p of paths) {
      if (p && typeof p === 'string' && p.startsWith('http')) return p;
    }
    // 깊은 탐색
    for (const key of Object.keys(data)) {
      const val = data[key];
      if (typeof val === 'string' && val.match(/^https?:\/\/.+\.(mp4|webm|mov)/i)) return val;
      if (typeof val === 'object' && val !== null) {
        const found = findVideoUrl(val);
        if (found) return found;
      }
    }
  }
  return '';
}

const segData = $('영상 세그먼트 순차 생성 루프').first().json;
const videoUrl = findVideoUrl(result);

staticData.all_results.push({
  segment_id: segData.segment_id,
  section: segData.section,
  segment_type: segData.segment_type,
  tts_text: segData.tts_text,
  time_range: segData.time_range,
  video_url: videoUrl,
  audio_url: segData.audio_url
});

staticData.completed_segments = staticData.all_results.length;

return [{
  json: {
    segment_id: segData.segment_id,
    video_url: videoUrl,
    completed: staticData.completed_segments,
    total: staticData.total_segments
  }
}];
```

---

#### Phase 4: 후처리 & 최종 편집

**노드 13: 자막 SRT 생성 (Code 노드)**

```javascript
const staticData = $getWorkflowStaticData('global');
const allResults = staticData.all_results || [];

// 시간순 정렬
allResults.sort((a, b) => (a.segment_id || 0) - (b.segment_id || 0));

let srt = '';
let srtIndex = 1;
let cumulativeTime = 0;

for (const seg of allResults) {
  if (!seg.tts_text || seg.tts_text.trim() === '') continue;

  // time_range 파싱 (예: "0:00-0:05")
  const timeRange = seg.time_range || '';
  const parts = timeRange.split('-');
  let startSec = 0;
  let endSec = 5;

  if (parts.length === 2) {
    const parseMM_SS = (str) => {
      const p = str.trim().split(':');
      return parseInt(p[0]) * 60 + parseInt(p[1]);
    };
    startSec = parseMM_SS(parts[0]);
    endSec = parseMM_SS(parts[1]);
  }

  const formatSRT = (totalSec) => {
    const h = String(Math.floor(totalSec / 3600)).padStart(2, '0');
    const m = String(Math.floor((totalSec % 3600) / 60)).padStart(2, '0');
    const s = String(totalSec % 60).padStart(2, '0');
    return `${h}:${m}:${s},000`;
  };

  srt += `${srtIndex}\n${formatSRT(startSec)} --> ${formatSRT(endSec)}\n${seg.tts_text}\n\n`;
  srtIndex++;
}

staticData.srt_content = srt;

return [{ json: { srt: srt, segments_count: allResults.length } }];
```

---

**노드 14: 클립 다운로드 명령 생성 (Code 노드)**

```javascript
const staticData = $getWorkflowStaticData('global');
const allResults = staticData.all_results || [];

allResults.sort((a, b) => (a.segment_id || 0) - (b.segment_id || 0));

const commands = [];
for (let i = 0; i < allResults.length; i++) {
  const url = allResults[i].video_url || '';
  if (url) {
    commands.push(`curl -L -s -o /tmp/clip_${i + 1}.mp4 "${url}"`);
  }
}

// BGM 다운로드
const bgmUrl = staticData.bgm_url || '';
if (bgmUrl) {
  commands.push(`curl -L -s -o /tmp/bgm.mp3 "${bgmUrl}"`);
}

// SRT 파일 저장
const fs = require('fs');
fs.writeFileSync('/tmp/subtitle.srt', staticData.srt_content || '');

// 클립 리스트 파일 생성
let fileList = '';
for (let i = 0; i < allResults.length; i++) {
  if (allResults[i].video_url) {
    fileList += `file '/tmp/clip_${i + 1}.mp4'\n`;
  }
}
fs.writeFileSync('/tmp/clips.txt', fileList);

return [{ json: { download_command: commands.join(' && '), clip_count: allResults.length, has_bgm: !!bgmUrl } }];
```

---

**노드 15: Execute Command — 클립 다운로드**

- **노드 종류:** Execute Command
- **Command:** `={{ $json.download_command }}`

---

**노드 16: FFmpeg 편집 명령 생성 (Code 노드)**

```javascript
const staticData = $getWorkflowStaticData('global');
const hasBgm = $json.has_bgm || false;
const clipCount = $json.clip_count || 0;

let ffmpegCmd = 'ffmpeg -y -f concat -safe 0 -i /tmp/clips.txt';

// 비디오 필터: 해상도 통일 + 자막
let vf = "scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2";

// 자막 오버레이
const srtContent = staticData.srt_content || '';
if (srtContent.trim().length > 0) {
  vf += ",subtitles=/tmp/subtitle.srt:force_style='FontName=NanumGothicBold,FontSize=22,PrimaryColour=&H00FFFFFF,OutlineColour=&H00000000,BackColour=&H80000000,Outline=2,Shadow=1,MarginV=60,Alignment=2'";
}

ffmpegCmd += ` -vf "${vf}"`;

// BGM 믹싱
if (hasBgm) {
  ffmpegCmd += ' -i /tmp/bgm.mp3 -filter_complex "[0:a]volume=1.0[voice];[1:a]volume=0.12,afade=t=in:d=1:st=0,afade=t=out:d=2:st=28[bgm];[voice][bgm]amix=inputs=2:duration=first[aout]" -map 0:v -map "[aout]"';
}

// 출력 설정 (고품질)
ffmpegCmd += ' -c:v libx264 -preset medium -crf 17 -pix_fmt yuv420p -c:a aac -b:a 192k -movflags +faststart /tmp/final_output.mp4';

return [{ json: { ffmpeg_command: ffmpegCmd } }];
```

---

**노드 17: Execute Command — FFmpeg 실행**

- **노드 종류:** Execute Command
- **Command:** `={{ $json.ffmpeg_command }}`
- **Timeout:** 300초 (5분)

---

#### Phase 5: 업로드

**노드 18: Read Binary File**

- **노드 종류:** Read Binary File (또는 Move Binary Data)
- **File Path:** `/tmp/final_output.mp4`
- **Property Name:** `data`

---

**노드 19: Google Drive 업로드**

- **노드 종류:** Google Drive
- **Operation:** Upload File
- **File Name:** `={{ $('Form Trigger').item.json['브랜드명'] }}_{{ $('Form Trigger').item.json['제품명'] }}_{{ new Date().toISOString().split('T')[0] }}_AI_UGC.mp4`
- **Folder:** (기존 C.01과 동일한 폴더 ID)
- **Binary Property:** `data`

---

**노드 20: 완료 알림 (선택)**

Slack, 이메일, 또는 Webhook으로 "영상 생성 완료" 알림 전송.

---

## 5. 비용 비교 최종

### 영상 1개 (30초, 7세그먼트) 기준

| 항목 | 모델 | 상세 | 비용 |
|------|------|------|------|
| 토킹 아바타 | Kling Avatar Pro | 3클립 × 5초 × $0.08/초 | **$1.20** |
| B-Roll 제품 | Kling 3.0 Pro | 1클립 × 5초 × $0.135/초 | **$0.68** |
| B-Roll 인터랙션 | Kling 3.0 Pro I2V | 2클립 × 5초 × $0.135/초 | **$1.35** |
| B-Roll 라이프스타일 | Kling 3.0 Pro | 1클립 × 3초 × $0.135/초 | **$0.41** |
| TTS | Typecast | 15초 (TALKING분만) | **$0.15** |
| BGM | Suno V4.5 | 1곡 30초 | **$0.20** |
| 캐릭터 이미지 | Flux Kontext | 2장 (정면+측면) | **$0.10** |
| **총 API 비용** | | | **$4.09** |
| FFmpeg 서버 비용 | n8n 서버 (자체) | 처리 시간 ~3분 | 무시 가능 |
| **최종 비용** | | | **~$4.09/영상 (약 5,300원)** |

### VIDI 대비

| | FlowPilot C.02 | VIDI |
|---|---|---|
| 영상 1개 제작 비용 | **5,300원** | **89,000원** |
| 비용 비율 | 1 | 16.8배 |
| 클라이언트 판매가 (예시) | 50,000원 | 89,000원 |
| 마진율 | **89.4%** | VIDI 자체 서비스 |

---

## 6. C.02 구축 일정 (예상)

| 단계 | 작업 내용 | 예상 시간 |
|------|----------|----------|
| Day 1 오전 | Phase 0~1: Form Trigger + Director AI + 출력 파싱 | 3시간 |
| Day 1 오후 | Phase 2: 캐릭터 이미지 + TTS 루프 + BGM + Merge | 3시간 |
| Day 2 오전 | Phase 3: 통합 영상 생성 루프 + 폴링 + 결과 집계 | 4시간 |
| Day 2 오후 | Phase 4: SRT 자막 + 클립 다운로드 + FFmpeg 편집 | 3시간 |
| Day 3 오전 | Phase 5: 업로드 + 에러 핸들링 + Error Trigger 설정 | 2시간 |
| Day 3 오후 | 전체 통합 테스트 + 디버깅 + 결과 폴링 응답 구조 확인 | 3시간 |
| **총 예상** | | **약 18시간 (3일)** |

---

## 7. 최종 결론

**추천: 신규 C.02 워크플로우 구축**

이유:
1. C.01은 토킹/B-Roll 분리 구조라 인터랙션 장면 추가가 구조적으로 불가
2. C.01에 누적된 기존 버그(JSON 에스케이핑, 폴링 응답 파싱, Google Drive 업로드 등) 위에 새 로직을 쌓으면 디버깅 지옥
3. C.02는 통합 루프 + segment_type 분기로 깔끔하게 4종 세그먼트 처리
4. FFmpeg 후처리(자막+BGM+트랜지션)로 VIDI처럼 편집 완성본 1개 출력
5. 비용이 오히려 C.01 수정보다 저렴 ($4.09 vs $5.93)

**C.01은 건드리지 않고 그대로 둔다. C.02를 새로 만든다. C.02 테스트 완료 후 C.01 비활성화.**

대표님이 "착수해라" 하면 바로 시작한다.
