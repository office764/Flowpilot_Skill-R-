# Kinohi AI UGC 컨텐츠 마이닝 C.01 기획서

## FlowPilot x Kinohi | Kie.ai + n8n 자동화 워크플로우

---

## 1. 프로젝트 개요

| 항목 | 내용 |
|------|------|
| 외주명 | 컨텐츠 마이닝 C.01 (AI UGC 컨텐츠) |
| 브랜드 | Kinohi (키노히) |
| 제품 | 키노히 바디미스트 (santal) |
| 판매가 | 30,000원 / 배송비 무료 |
| 용량 | 200ml |
| 향 노트 | cardamon, violet, sandal wood |
| 브랜드 컨셉 | "이른 아침 산속에서 깨어나 첫 숨을 들이 마실 때, 청량한 공기 속에서 살짝 젖은 잔디와 중후한 나무껍질의 향이 어우러져 시원하면서도 성숙한 고요함을 느낄 수 있는 향" |
| 고객만족도 | 4.95/5.0 (스마트스토어 누적) |
| 사용 도구 | Kie.ai API + n8n + Claude AI |
| 데모 단계 | 무료 데모 (수동 일부 허용) → 계약 후 완전 자동화 |

---

## 2. 고객 리뷰 분석 & 인사이트 도출

### 2-1. 리뷰 원문 분석

**리뷰 1** (2025.02.27 / 29CM 구매 / ★★★★★)
- 제목: "로션보다 훨씬 사용하기 간편해요!"
- 핵심: 모공각화증에 바디미스트가 좋다는 입소문 → 로션보다 간편, 흡수 좋음, 모공각화증 개선 체감, 향 만족

**리뷰 2** (2025.05.11 / 스마트스토어 / ★★★★★)
- 제목: "끈적임 없이 바로 스며들어요!"
- 핵심: 건조해서 각질 → 바디미스트 뿌리자마자 수분감 싹 덮음, 촉촉함 오래 유지, 끈적임 없이 바로 스며듦, 샤워 후 바르기 딱 좋음, 바디크림 대체템 추천

**리뷰 3** (2024.05.21 / 자사몰 / ★★★★★)
- 제목: "은은한 향기가 좋아요!"
- 핵심: 분사력 좋음, 은은한 향이 퍼짐, 향수 대신 사용 가능, 데일리 사용 만족, 여름에 딱

**리뷰 4** (2024.09.12 / 스마트스토어 / ★★★★★)
- 제목: "보습감이랑 수분력 유지가 좋아요!"
- 핵심: 보습감 + 수분력 유지, 잠도 잘 오고 편안한 느낌, 심신의 안정, 재구매 의사

### 2-2. 핵심 인사이트 (셀링 포인트 5가지)

| # | 셀링 포인트 | 리뷰 근거 | UGC 영상 훅(Hook) |
|---|-----------|----------|-----------------|
| SP1 | **모공각화증 케어** | 리뷰1: 모공각화증이 좋아졌다 | "팔뚝 닭살피부, 바디미스트 하나로 달라졌어요" |
| SP2 | **바디크림 대체** (간편함) | 리뷰1,2: 로션보다 간편, 바디크림 귀찮을 때 대체 | "바디크림 귀찮은 사람? 이거 뿌리기만 하면 끝" |
| SP3 | **즉각 보습 + 끈적임 ZERO** | 리뷰2: 끈적임 없이 바로 스며듦, 촉촉함 오래 유지 | "뿌리자마자 스며드는 수분감, 근데 안 끈적여" |
| SP4 | **은은한 향 = 향수 대체** | 리뷰3: 향수 안 뿌려도 은은하게 퍼짐 | "향수 대신 이거 뿌리는데 칭찬만 들어요" |
| SP5 | **수면 / 심신 안정** | 리뷰4: 잠도 잘 오고 심신의 안정 | "자기 전에 뿌리면 잠이 솔솔, 산속 힐링 그 자체" |

### 2-3. 타겟 페르소나

| 페르소나 | 특징 | 주 셀링 포인트 |
|---------|------|--------------|
| A. 20대 여성 (모공각화증 고민) | 팔뚝/다리 닭살피부, SNS에서 정보 탐색 | SP1 + SP2 |
| B. 20~30대 직장인 여성 | 바디케어 루틴 귀찮음, 간편한 제품 선호 | SP2 + SP3 |
| C. 향기 덕후 | 데일리 향 아이템 탐색, 은은한 향 선호 | SP4 + SP5 |

---

## 3. AI UGC 컨텐츠 영상 기획 (5개 시나리오)

### 시나리오 1: "모공각화증 케어" (SP1 중심)

| 항목 | 내용 |
|------|------|
| 타겟 | 페르소나 A (20대 여성, 모공각화증 고민) |
| 길이 | 15~30초 (릴스/숏폼) |
| 톤앤무드 | 솔직한 리뷰, 비포/애프터 느낌 |

**장면 구성:**

| 씬 | 시간 | 화면 | 나레이션/텍스트 오버레이 |
|----|------|------|----------------------|
| S1 | 0~3초 | 여성이 팔뚝을 보며 한숨 (클로즈업) | Hook: "팔뚝 닭살 때문에 반팔 못 입는 사람?" |
| S2 | 3~7초 | 키노히 바디미스트 제품 클로즈업 (제품샷) | "이 바디미스트가 입소문 난 이유가 있어요" |
| S3 | 7~15초 | 샤워 후 팔뚝에 미스트 뿌리는 장면 | "샤워하고 뿌리기만 하면 끝. 로션보다 훨씬 간편하고 흡수도 빠름" |
| S4 | 15~22초 | 팔뚝 피부결 클로즈업 (촉촉한 느낌) | "끈적임 없이 바로 스며드는데 수분감이 쫙~" |
| S5 | 22~30초 | 자신감 있게 반팔 입고 나가는 장면 | "모공각화증 고민이라면 한번 써보세요. 키노히 바디미스트 santal" + CTA |

---

### 시나리오 2: "바디크림 대체템" (SP2 + SP3 중심)

| 항목 | 내용 |
|------|------|
| 타겟 | 페르소나 B (직장인, 간편함 추구) |
| 길이 | 15~20초 |
| 톤앤무드 | 일상적, 공감형 |

**장면 구성:**

| 씬 | 시간 | 화면 | 나레이션/텍스트 오버레이 |
|----|------|------|----------------------|
| S1 | 0~3초 | 퇴근 후 지친 모습, 바디크림 보며 귀찮아하는 장면 | Hook: "바디크림 바르기 귀찮은 사람 손!" |
| S2 | 3~8초 | 키노히 미스트 꺼내서 슥슥 뿌리는 장면 | "이제 뿌리기만 하면 끝이에요" |
| S3 | 8~15초 | 팔/다리에 뿌리고 바로 흡수되는 장면 | "끈적임 없이 바로 스며들고 촉촉함이 오래 감" |
| S4 | 15~20초 | 편안하게 침대에 눕는 장면 + 제품 엔딩샷 | "바디크림 졸업. 키노히 바디미스트로 완전 대체!" + CTA |

---

### 시나리오 3: "향수 대체 아이템" (SP4 중심)

| 항목 | 내용 |
|------|------|
| 타겟 | 페르소나 C (향기 덕후) |
| 길이 | 15~25초 |
| 톤앤무드 | 감성적, 시네마틱 |

**장면 구성:**

| 씬 | 시간 | 화면 | 나레이션/텍스트 오버레이 |
|----|------|------|----------------------|
| S1 | 0~3초 | 아침 햇살 속 드레싱 테이블 | Hook: "향수 대신 매일 뿌리는 거 있는데" |
| S2 | 3~10초 | 목, 손목에 미스트 뿌리는 장면 (슬로우 모션) | "뿌리는 순간 산속 공기가 퍼지는 느낌" |
| S3 | 10~18초 | 외출 후 친구가 "무슨 향이야?" 물어보는 장면 | "은은하게 퍼지는 향에 매일 칭찬 받아요" |
| S4 | 18~25초 | 제품 클로즈업 + 브랜드 무드 | "키노히 santal. 카다몬, 바이올렛, 샌달우드" + CTA |

---

### 시나리오 4: "수면 루틴" (SP5 중심)

| 항목 | 내용 |
|------|------|
| 타겟 | 수면/힐링 관심층 |
| 길이 | 15~25초 |
| 톤앤무드 | ASMR 느낌, 차분한 |

**장면 구성:**

| 씬 | 시간 | 화면 | 나레이션/텍스트 오버레이 |
|----|------|------|----------------------|
| S1 | 0~3초 | 밤, 조명 어두운 침실 | Hook: "잠이 안 올 때 이거 뿌려봐" |
| S2 | 3~10초 | 가운 입고 목/팔에 미스트 뿌리는 장면 | "산속 새벽 공기 같은 향이 퍼지면서 마음이 차분해져요" |
| S3 | 10~18초 | 침대에 누워 편안한 표정 | "보습도 되면서 심신 안정까지. 잠이 솔솔 와요" |
| S4 | 18~25초 | 아침에 상쾌하게 일어나는 장면 + 엔딩 | "키노히 바디미스트, 나만의 수면 루틴" + CTA |

---

### 시나리오 5: "올인원 데일리 루틴" (SP2+SP3+SP4 종합)

| 항목 | 내용 |
|------|------|
| 타겟 | 전체 타겟 (광범위) |
| 길이 | 20~30초 |
| 톤앤무드 | 밝고 에너지틱한 일상 |

**장면 구성:**

| 씬 | 시간 | 화면 | 나레이션/텍스트 오버레이 |
|----|------|------|----------------------|
| S1 | 0~3초 | 샤워기에서 물이 떨어지는 장면 | Hook: "샤워 후 딱 하나만 쓰는 루틴" |
| S2 | 3~10초 | 샤워 후 온몸에 미스트 뿌리는 장면 | "바디크림 대신 뿌리기만 하면 끝" |
| S3 | 10~18초 | 옷 입고 나가면서 손목에 한 번 더 | "보습 + 향까지 한 번에 해결" |
| S4 | 18~25초 | 하루 종일 활동하는 몽타주 | "하루 종일 은은한 향이 유지돼요" |
| S5 | 25~30초 | 제품 + 브랜드 로고 엔딩 | "키노히 바디미스트 santal. 30,000원 / 무료배송" + CTA |

---

## 4. AI UGC 제작 파이프라인 (Kie.ai 활용)

### 4-1. 사용 AI 모델 (Kie.ai API 경유)

| 용도 | 모델 | Kie.ai 엔드포인트 | 비고 |
|------|------|-----------------|------|
| AI 캐릭터 이미지 생성 | Flux Kontext Pro/Max | `POST https://api.kie.ai/api/v1/flux/kontext/generate` | 일관된 캐릭터 생성 |
| 제품 이미지 생성/편집 | Flux Kontext Pro | 동일 | 제품 배경 교체, 스타일링 |
| 키프레임 → 영상 변환 | Veo 3.1 (Fast/Quality) | `POST https://api.kie.ai/api/v1/veo/generate` | Image-to-Video |
| 영상 생성 (텍스트 기반) | Sora 2 Pro | `POST https://api.kie.ai/api/v1/jobs/createTask` | Text-to-Video |
| 영상 생성 (이미지 기반) | Sora 2 Pro Image-to-Video | `POST https://api.kie.ai/api/v1/jobs/createTask` | 키프레임 기반 |
| 나레이션/보이스 | 외부 TTS (ElevenLabs 등) | 별도 | AI 음성 생성 |
| 배경음악 | Suno V4 (Kie.ai 경유) | Kie.ai Suno API | 브랜드 무드 BGM |

### 4-2. 제작 프로세스 (씬 단위)

```
[Step 1] 시나리오 선택 & 프롬프트 생성 (Claude AI)
    ↓
[Step 2] AI 캐릭터 키프레임 이미지 생성 (Flux Kontext via Kie.ai)
    ↓
[Step 3] 제품 이미지 생성/합성 (Flux Kontext via Kie.ai)
    ↓
[Step 4] 키프레임 → 5초 영상 클립 생성 (Veo 3.1 / Sora 2 via Kie.ai)
    ↓
[Step 5] 나레이션 생성 (TTS API)
    ↓
[Step 6] BGM 생성 (Suno via Kie.ai)
    ↓
[Step 7] 영상 합성 & 텍스트 오버레이 (수동: Premiere Pro / CapCut)
    ↓
[Step 8] 최종 QC & 납품
```

---

## 5. n8n 워크플로우 노드 기획 (상세)

### 5-0. 전체 워크플로우 아키텍처

```
[Webhook Trigger] → [시나리오 데이터 로드] → [프롬프트 생성 (AI Agent)]
    → [이미지 생성 Loop] → [영상 생성 Loop] → [결과 저장] → [알림 발송]
```

### 워크플로우 1: 마스터 컨트롤러 (메인 오케스트레이터)

---

#### Node 1: Webhook (트리거)

- **노드 타입**: Webhook
- **역할**: 외부에서 시나리오 번호를 지정하여 워크플로우를 트리거
- **설정**:
  - HTTP Method: `POST`
  - Path: `kinohi-ugc-trigger`
  - Authentication: Header Auth
  - Response Mode: `Last Node`

**수신 JSON 예시:**
```json
{
  "scenario_id": 1,
  "brand": "Kinohi",
  "product": "바디미스트 santal",
  "target_platform": "instagram_reels",
  "aspect_ratio": "9:16"
}
```

---

#### Node 2: Set 노드 (시나리오 데이터 매핑)

- **노드 타입**: Set
- **역할**: Webhook 수신 데이터를 기반으로 시나리오별 장면 데이터를 세팅
- **설정**:
  - Mode: `Manual Mapping`
  - 필드 목록:
    - `scenario_id` → `{{ $json.scenario_id }}`
    - `brand` → `{{ $json.brand }}`
    - `product` → `{{ $json.product }}`
    - `aspect_ratio` → `{{ $json.aspect_ratio }}`
    - `scenes_count` → 시나리오별 장면 수 (예: 시나리오1 = 5)

---

#### Node 3: Switch 노드 (시나리오 분기)

- **노드 타입**: Switch
- **역할**: scenario_id 값에 따라 각 시나리오별 프롬프트 세트로 분기
- **설정**:
  - Mode: `Rules`
  - Data Type: `Number`
  - Value: `{{ $json.scenario_id }}`
  - Rules:
    - Output 0: `scenario_id` equals `1` → "모공각화증 케어"
    - Output 1: `scenario_id` equals `2` → "바디크림 대체"
    - Output 2: `scenario_id` equals `3` → "향수 대체"
    - Output 3: `scenario_id` equals `4` → "수면 루틴"
    - Output 4: `scenario_id` equals `5` → "올인원 루틴"

---

#### Node 4: Code 노드 (장면별 프롬프트 배열 생성)

- **노드 타입**: Code
- **역할**: 선택된 시나리오의 각 씬별 이미지/영상 프롬프트를 배열로 생성
- **설정**:
  - Language: `JavaScript`

**코드 예시 (시나리오 1 - 모공각화증 케어):**

```javascript
const scenarioId = $input.first().json.scenario_id;
const aspectRatio = $input.first().json.aspect_ratio || "9:16";

// 시나리오 1: 모공각화증 케어
const scenes = [
  {
    scene_id: "S1",
    scene_name: "Hook - 팔뚝 고민",
    duration: "3s",
    image_prompt: "Close-up shot of a young Korean woman in her mid-20s looking down at her upper arm with a slightly worried expression in a bright modern bathroom, soft natural window light from the left side, wearing a simple white tank top, her skin showing subtle texture on the upper arm area, shot on Sony A7R V with Sony FE 85mm f/1.4 GM lens at f/2.0, ISO 400, color temperature 5200K, shallow depth of field with background softly blurred, hyper-realistic photorealistic photograph indistinguishable from real DSLR photo, ultra-detailed skin texture with visible pores and micro skin imperfections and fine peach fuzz hair and natural skin blemishes and subtle freckles, real human skin subsurface scattering with natural blood vessel undertones, individual hair strands visible with natural flyaway hairs and split ends, eyes with detailed iris fibers and natural eye moisture reflection and visible blood vessels in sclera, natural asymmetric facial features as real humans have, visible fingernail texture with natural nail ridges and cuticle detail, natural body hair on arms visible under light, realistic ear canal detail and ear lobe texture, natural lip texture with visible lip lines and slight dryness, teeth with natural slight discoloration and realistic enamel reflection, shot on real camera sensor with natural lens aberration and chromatic fringing at edges, realistic depth of field with optical bokeh circles showing cat-eye effect at frame edges, natural film grain matching ISO setting, absolutely ZERO artificial smooth airbrushed plastic skin, ZERO uncanny valley effect, ZERO CGI render look, ZERO digital art style, ZERO perfect symmetry, must pass as genuine unedited photograph taken by professional photographer",
    video_prompt: "A young Korean woman in her mid-20s in a bright modern bathroom, soft natural window light, wearing a white tank top, looking at her upper arm with a slightly worried expression, she gently touches her upper arm and sighs softly, subtle natural movements like breathing and blinking, camera holds steady close-up on her face and arm, hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin with visible pores and micro expressions and natural skin texture movement during motion, realistic hair physics with individual strands responding to movement and gravity and breeze naturally, natural subtle body micro-movements like breathing pulse and muscle tension shifts and weight distribution changes, realistic eye movement with natural blink rate and eye moisture and pupil dilation, fabric physics responding realistically to body movement and air and gravity, natural motion blur consistent with real camera shutter speed, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, ZERO robotic stiff movement, must pass as genuine footage from professional cinema camera",
    text_overlay: "팔뚝 닭살 때문에 반팔 못 입는 사람?",
    narration: "팔뚝 닭살 때문에 여름마다 스트레스 받는 사람?"
  },
  {
    scene_id: "S2",
    scene_name: "제품 소개",
    duration: "4s",
    image_prompt: "Elegant product photography of Kinohi body mist santal bottle on a natural stone surface with morning dew drops, soft sage green and cream color palette matching the brand aesthetic, the frosted glass bottle with minimalist KINOHI branding clearly visible, surrounded by subtle cardamom pods and sandalwood chips as decorative elements, golden hour morning sunlight streaming from the right creating warm highlights and soft shadows, shot on Hasselblad X2D 100C with XCD 120mm f/3.5 macro lens at f/5.6, ISO 200, color temperature 4800K, hyper-realistic product photography indistinguishable from real studio photograph, ultra-detailed material textures with visible surface micro-texture and natural light interaction, realistic glass refraction and reflection with visible internal light caustics, natural shadow falloff with realistic ambient occlusion and contact shadows, visible label printing texture and slight label edge detail, dust particles visible on surface under studio light, absolutely ZERO CGI render look, ZERO 3D modeling aesthetic, must pass as genuine product photo from commercial studio shoot",
    video_prompt: "Smooth cinematic dolly shot slowly moving toward a Kinohi body mist santal bottle placed on a natural stone surface with morning dew, golden hour sunlight creating warm highlights, subtle steam or mist particles floating in the air around the bottle, the frosted glass catching and refracting light beautifully, sage green and cream color palette, cardamom pods and sandalwood chips as props, camera slowly dollies in from medium shot to close-up over 4 seconds, hyper-realistic footage indistinguishable from real cinema camera capture, natural motion blur consistent with real camera shutter speed, absolutely ZERO AI-generated look, must pass as genuine footage from professional cinema camera",
    text_overlay: "이 바디미스트가 입소문 난 이유",
    narration: "이 바디미스트가 모공각화증 커뮤니티에서 입소문 난 이유가 있어요"
  },
  {
    scene_id: "S3",
    scene_name: "사용 장면",
    duration: "8s",
    image_prompt: "Young Korean woman in her mid-20s spraying body mist on her upper arm after shower in a bright clean bathroom, wearing a white bathrobe, fine mist particles visible in the air catching the light, her skin slightly damp from shower with water droplets visible, natural relaxed expression, soft bathroom lighting from above and window, shot on Sony A7R V with Sony FE 50mm f/1.2 GM lens at f/2.8, ISO 640, color temperature 4500K, hyper-realistic photorealistic photograph indistinguishable from real DSLR photo, ultra-detailed skin texture with visible pores and micro skin imperfections and fine peach fuzz hair and natural skin blemishes, real human skin subsurface scattering with natural blood vessel undertones, individual hair strands visible with natural flyaway hairs, natural asymmetric facial features as real humans have, visible fingernail texture with natural nail ridges and cuticle detail, natural body hair on arms visible under light, shot on real camera sensor with natural lens aberration, realistic depth of field with optical bokeh, natural film grain matching ISO setting, absolutely ZERO artificial smooth airbrushed plastic skin, ZERO uncanny valley effect, ZERO CGI render look, must pass as genuine unedited photograph taken by professional photographer",
    video_prompt: "A young Korean woman in a white bathrobe in a bright modern bathroom, she picks up the Kinohi body mist bottle and sprays it onto her upper arm in a smooth natural motion, fine mist particles catch the light creating a beautiful spray effect, she then gently rubs her arm with a satisfied expression, her damp hair has natural movement, camera follows the spray action from medium shot, hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin with visible pores and natural skin texture movement during motion, realistic hair physics, natural subtle body micro-movements like breathing, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, must pass as genuine footage from professional cinema camera",
    text_overlay: "샤워 후 뿌리기만 하면 끝",
    narration: "샤워하고 나서 뿌리기만 하면 끝이에요. 로션보다 훨씬 간편하고 흡수도 엄청 빨라요"
  },
  {
    scene_id: "S4",
    scene_name: "효과 클로즈업",
    duration: "7s",
    image_prompt: "Extreme close-up of smooth hydrated Korean female arm skin after applying body mist, the skin has a natural healthy glow with subtle dewy moisture visible on the surface, you can see the fine skin texture and natural pores, soft warm lighting highlighting the moisture on the skin, shot on Canon EOS R5 with Canon RF 100mm f/2.8L Macro IS USM lens at f/4.0, ISO 320, color temperature 5000K, macro photography showing incredible skin detail, hyper-realistic photorealistic photograph indistinguishable from real DSLR photo, ultra-detailed skin texture with visible pores and micro skin imperfections and fine peach fuzz hair, real human skin subsurface scattering with natural blood vessel undertones, natural body hair on arms visible under light, absolutely ZERO artificial smooth airbrushed plastic skin, ZERO CGI render look, must pass as genuine macro photograph",
    video_prompt: "Extreme close-up macro shot of a Korean woman's arm skin, the camera slowly glides along the arm surface showing the dewy hydrated texture, subtle light reflections dance on the moisturized skin, you can see fine skin texture and natural pores in incredible detail, the moisture from the body mist creates a healthy natural glow, camera movement is slow and smooth, hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin visible pores and natural texture, absolutely ZERO artificial smooth plastic skin, ZERO AI-generated look, must pass as genuine footage",
    text_overlay: "끈적임 없이 바로 스며드는 수분감",
    narration: "끈적임 없이 바로 스며드는데, 수분감이 쫙~ 오래 유지돼요"
  },
  {
    scene_id: "S5",
    scene_name: "엔딩 (자신감)",
    duration: "8s",
    image_prompt: "Confident young Korean woman in her mid-20s walking out through a bright doorway wearing a casual short-sleeve summer outfit, natural sunlight illuminating her from behind creating a beautiful rim light on her arms and shoulders, she has a bright natural smile and confident posture, her bare arms visible and looking healthy, urban street background softly blurred, shot on Sony A7R V with Sony FE 35mm f/1.4 GM lens at f/2.0, ISO 200, color temperature 5600K outdoor daylight, hyper-realistic photorealistic photograph indistinguishable from real DSLR photo, ultra-detailed skin texture with visible pores and micro skin imperfections, natural asymmetric facial features, individual hair strands visible with natural flyaway hairs, realistic depth of field with optical bokeh, natural film grain, absolutely ZERO artificial smooth airbrushed plastic skin, ZERO uncanny valley effect, ZERO CGI render look, must pass as genuine unedited photograph",
    video_prompt: "A confident young Korean woman in a casual short-sleeve summer outfit walks through a bright doorway into outdoor sunlight, natural rim light illuminating her bare arms and shoulders, she smiles naturally and walks with confident posture, her hair moves naturally in a light breeze, the camera tracks her movement smoothly, urban background with natural bokeh, hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin with visible pores, realistic hair physics with individual strands responding to breeze, natural subtle body movements, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, must pass as genuine footage from professional cinema camera",
    text_overlay: "키노히 바디미스트 santal",
    narration: "모공각화증 고민이라면 꼭 한번 써보세요. 키노히 바디미스트 santal"
  }
];

return scenes.map(scene => ({ json: scene }));
```

---

#### Node 5: Loop Over Items (씬 반복 처리)

- **노드 타입**: Loop Over Items (Split In Batches)
- **역할**: 각 씬을 1개씩 순차 처리 (API Rate Limit 고려)
- **설정**:
  - Batch Size: `1`

---

#### Node 6: HTTP Request — 이미지 생성 (Flux Kontext via Kie.ai)

- **노드 타입**: HTTP Request
- **역할**: 각 씬의 키프레임 이미지를 Flux Kontext API로 생성
- **설정**:
  - Method: `POST`
  - URL: `https://api.kie.ai/api/v1/flux/kontext/generate`
  - Authentication: Generic Credential Type → Header Auth
    - Header Name: `Authorization`
    - Header Value: `Bearer YOUR_KIE_AI_API_KEY` (Credential에서 관리)
  - Send Headers: `Content-Type` = `application/json`
  - Send Body: JSON
  - Body JSON:

```json
{
  "prompt": "{{ $json.image_prompt }}",
  "model": "flux-kontext-pro",
  "aspectRatio": "{{ $('Set').item.json.aspect_ratio === '9:16' ? '9:16' : '16:9' }}",
  "outputFormat": "png",
  "enableTranslation": false,
  "safetyTolerance": 2,
  "callBackUrl": ""
}
```

- **응답 처리**: `data.taskId` 추출

---

#### Node 7: Wait 노드 (이미지 생성 대기)

- **노드 타입**: Wait
- **역할**: Kie.ai 비동기 작업 완료 대기
- **설정**:
  - Resume: `After Time Interval`
  - Wait Amount: `30` (초)

> **참고**: 데모 단계에서는 고정 Wait 사용. 고도화 시 Callback URL + Webhook 수신으로 전환하여 대기 시간 최적화.

---

#### Node 8: HTTP Request — 이미지 결과 조회 (Polling)

- **노드 타입**: HTTP Request
- **역할**: 생성된 이미지의 결과 URL을 조회
- **설정**:
  - Method: `GET`
  - URL: `https://api.kie.ai/api/v1/task/{{ $('HTTP Request - 이미지 생성').item.json.data.taskId }}`
  - Authentication: 동일 (Bearer Token)
- **응답에서 추출**: `data.info.resultUrls` → 이미지 URL

> **참고**: Kie.ai 공식 문서에서 task 결과 조회 엔드포인트를 확인해야 함. 위 URL은 일반적인 패턴이며, 실제 엔드포인트는 `https://docs.kie.ai` 에서 확인 필요.

---

#### Node 9: HTTP Request — 영상 생성 (Veo 3.1 via Kie.ai)

- **노드 타입**: HTTP Request
- **역할**: 키프레임 이미지를 기반으로 5초 영상 클립 생성
- **설정**:
  - Method: `POST`
  - URL: `https://api.kie.ai/api/v1/veo/generate`
  - Authentication: Bearer Token (Header Auth)
  - Send Body: JSON
  - Body JSON:

```json
{
  "prompt": "{{ $json.video_prompt }}",
  "imageUrls": ["{{ $('HTTP Request - 이미지 결과 조회').item.json.data.info.resultUrls[0] }}"],
  "model": "veo3_fast",
  "generationType": "REFERENCE_2_VIDEO",
  "aspect_ratio": "{{ $('Set').item.json.aspect_ratio === '9:16' ? '9:16' : '16:9' }}",
  "enableTranslation": false
}
```

- **응답 처리**: `data.taskId` 추출

---

#### Node 10: Wait 노드 (영상 생성 대기)

- **노드 타입**: Wait
- **역할**: 영상 생성 완료 대기 (Veo 3.1은 보통 60~120초)
- **설정**:
  - Resume: `After Time Interval`
  - Wait Amount: `120` (초)

---

#### Node 11: HTTP Request — 영상 결과 조회

- **노드 타입**: HTTP Request
- **역할**: 생성된 영상 URL 조회
- **설정**:
  - Method: `GET`
  - URL: `https://api.kie.ai/api/v1/task/{{ $('HTTP Request - 영상 생성').item.json.data.taskId }}`
  - Authentication: Bearer Token
- **응답에서 추출**: `data.info.resultUrls` → 영상 MP4 URL

---

#### Node 12: Set 노드 (결과 집계)

- **노드 타입**: Set
- **역할**: 각 씬의 이미지 URL + 영상 URL + 메타데이터를 하나로 집계
- **설정**:
  - `scene_id` → `{{ $json.scene_id }}`
  - `image_url` → 이미지 결과 URL
  - `video_url` → 영상 결과 URL
  - `text_overlay` → `{{ $json.text_overlay }}`
  - `narration` → `{{ $json.narration }}`

---

#### Node 13: Google Sheets 노드 (결과 저장)

- **노드 타입**: Google Sheets
- **Operation**: `Append Row` (행 추가)
- **역할**: 생성된 모든 에셋(이미지 URL, 영상 URL, 텍스트)을 구글시트에 기록
- **설정**:
  - Spreadsheet: `Kinohi UGC Asset Tracker` (미리 생성 필요)
  - Sheet: `시나리오1_모공각화증`
  - Columns Mapping:
    - A: `scene_id`
    - B: `scene_name`
    - C: `image_url`
    - D: `video_url`
    - E: `text_overlay`
    - F: `narration`
    - G: `created_at` (현재 시각)
    - H: `status` → "generated"

---

#### Node 14: Slack 노드 (or Telegram) — 완료 알림

- **노드 타입**: Slack (또는 Telegram)
- **Operation**: `Send Message` (메시지 보내기)
- **역할**: 전체 씬 생성 완료 후 담당자에게 알림
- **설정**:
  - Channel: `#kinohi-ugc-production`
  - Message:

```
🎬 Kinohi AI UGC 생성 완료!

시나리오: {{ $('Set').item.json.scenario_id }}번
생성된 씬: {{ $('Loop Over Items').item.json.scenes_count }}개
에셋 시트: [Google Sheet 링크]

✅ 이미지 + 영상 클립 생성 완료
⏳ 다음 단계: 수동 편집 (텍스트 오버레이 + 나레이션 + BGM 합성)
```

---

### 워크플로우 2: TTS 나레이션 생성 (보조 워크플로우)

> **참고**: 이 워크플로우는 별도로 수동 트리거하거나 메인 워크플로우 완료 후 실행

#### Node 1: Manual Trigger

- **노드 타입**: Manual Trigger
- **역할**: 수동 실행

#### Node 2: Google Sheets — 나레이션 텍스트 읽기

- **노드 타입**: Google Sheets
- **Operation**: `Read Rows` (행 읽기)
- **설정**:
  - Spreadsheet: `Kinohi UGC Asset Tracker`
  - Sheet: 해당 시나리오 시트
  - Column: `F` (narration 컬럼)

#### Node 3: HTTP Request — TTS 생성

- **노드 타입**: HTTP Request
- **역할**: ElevenLabs 또는 기타 TTS API로 나레이션 음성 생성
- **설정** (ElevenLabs 예시):
  - Method: `POST`
  - URL: `https://api.elevenlabs.io/v1/text-to-speech/{voice_id}`
  - Authentication: Header Auth (`xi-api-key`)
  - Body JSON:

```json
{
  "text": "{{ $json.narration }}",
  "model_id": "eleven_multilingual_v2",
  "voice_settings": {
    "stability": 0.5,
    "similarity_boost": 0.75,
    "style": 0.3
  }
}
```

#### Node 4: Move Binary Data + 저장

- 생성된 오디오 파일을 Google Drive 또는 로컬에 저장

---

## 6. 데모 vs 고도화 비교

### 6-1. 현재 데모 (무료)

| 항목 | 방식 |
|------|------|
| 트리거 | 수동 (Webhook 또는 Manual Trigger) |
| 시나리오 선택 | 수동 번호 지정 |
| 이미지 생성 | n8n → Kie.ai API (자동) |
| 영상 생성 | n8n → Kie.ai API (자동) |
| 결과 대기 | 고정 Wait (30~120초) |
| 나레이션 | 별도 워크플로우 or 수동 |
| 텍스트 오버레이 | 수동 (CapCut/Premiere) |
| 최종 편집 | 수동 (영상 합성 + BGM) |
| 에셋 관리 | Google Sheets 자동 기록 |
| 알림 | Slack/Telegram 자동 알림 |

**수동 작업 비중: 약 30~40%** (최종 편집, 텍스트 오버레이, BGM 합성)

### 6-2. 계약 후 고도화 (완전 자동화)

| 항목 | 방식 |
|------|------|
| 트리거 | 스케줄 (매주 자동) 또는 Airtable/노션 연동 |
| 시나리오 선택 | AI 자동 선택 (성과 데이터 기반) |
| 이미지 생성 | Callback URL로 즉시 다음 스텝 진행 |
| 영상 생성 | Callback URL 기반 비동기 처리 |
| 결과 대기 | Webhook 수신 (Wait 제거 → 속도 2~3배 향상) |
| 나레이션 | 자동 (ElevenLabs API 파이프라인 내 통합) |
| 텍스트 오버레이 | FFmpeg 자동 처리 (n8n Execute Command) |
| 최종 편집 | FFmpeg 자동 합성 (클립 + 나레이션 + BGM + 자막) |
| 에셋 관리 | Airtable + CDN 자동 업로드 |
| 알림 | 완성 영상 미리보기 포함 Slack 알림 |
| 배포 | Instagram/TikTok/YouTube Shorts 자동 업로드 (Later API 등) |
| A/B 테스트 | 동일 시나리오 다른 훅으로 2개 버전 자동 생성 |
| 성과 추적 | Meta Pixel + GA4 연동 → 컨버전 데이터 역주입 |

**수동 작업 비중: 0~5%** (최종 QC 승인만)

---

## 7. 비용 예측 (씬당 / 시나리오당)

### Kie.ai API 크레딧 기준 (1 크레딧 ≈ $0.005)

| 항목 | 모델 | 예상 크레딧 | 예상 비용 (USD) |
|------|------|-----------|---------------|
| 이미지 1장 (Flux Kontext Pro) | flux-kontext-pro | ~20 크레딧 | ~$0.10 |
| 영상 5초 (Veo 3.1 Fast) | veo3_fast | ~200 크레딧 | ~$1.00 |
| 영상 5초 (Veo 3.1 Quality) | veo3 | ~400 크레딧 | ~$2.00 |
| 영상 5초 (Sora 2 Pro) | sora-2-pro | ~300 크레딧 | ~$1.50 |

**시나리오 1개 (5씬) 예상 비용:**

| 항목 | 수량 | 단가 | 합계 |
|------|------|------|------|
| 키프레임 이미지 | 5장 | $0.10 | $0.50 |
| 영상 클립 (Veo 3.1 Fast) | 5개 | $1.00 | $5.00 |
| TTS 나레이션 | 5개 | ~$0.05 | $0.25 |
| BGM (Suno) | 1곡 | ~$0.50 | $0.50 |
| **합계** | | | **~$6.25 (약 ₩8,500)** |

> 시나리오 5개 전체 제작 시: 약 **$31.25 (약 ₩42,500)**
> 실제 크레딧 소모는 모델/해상도에 따라 변동 가능

---

## 8. 납품물 & 타임라인

### 납품물

| # | 납품물 | 형식 | 비고 |
|---|--------|------|------|
| 1 | 리뷰 분석 & 인사이트 리포트 | 본 문서 | 완료 |
| 2 | 시나리오 5개 (장면별 상세 기획) | 본 문서 섹션 3 | 완료 |
| 3 | n8n 워크플로우 (메인 오케스트레이터) | n8n JSON | 데모 구현 후 제공 |
| 4 | AI 생성 키프레임 이미지 (시나리오별 5장) | PNG | 데모 1개 시나리오 |
| 5 | AI 생성 영상 클립 (시나리오별 5개 × 5초) | MP4 | 데모 1개 시나리오 |
| 6 | 최종 편집 영상 (시나리오별 1개) | MP4 15~30초 | 수동 편집 |

### 타임라인 (데모)

| 단계 | 작업 | 소요일 |
|------|------|--------|
| D+0 | 기획서 전달 (본 문서) | 완료 |
| D+1 | n8n 워크플로우 세팅 + Kie.ai API 연동 테스트 | 1일 |
| D+2 | 시나리오 1 이미지 + 영상 생성 (데모) | 1일 |
| D+3 | 나레이션 + 수동 편집 | 1일 |
| D+4 | 최종 QC + 납품 | 0.5일 |
| **합계** | | **약 3.5일** |

---

## 9. CTO 인사이트: 고도화 시 Antigravity + n8n 업셀 포인트

계약 후 고도화 단계에서 Antigravity를 결합하면 다음과 같은 프리미엄 가치를 제공할 수 있습니다:

**1. Antigravity 프론트엔드 대시보드**
- 키노히 마케팅팀이 직접 시나리오를 선택하고 실시간으로 생성 진행률을 볼 수 있는 커스텀 대시보드
- n8n은 백엔드 오케스트레이터, Antigravity는 클라이언트 인터페이스

**2. A/B 테스트 자동화 엔진**
- 동일 시나리오의 Hook 변형 (텍스트, 인물, 톤)을 자동 생성
- 인스타그램/틱톡 퍼포먼스 데이터를 역수집하여 위닝 소재 자동 판별
- Antigravity가 데이터 수집 + 시각화, n8n이 자동 재생성 트리거

**3. MRR 구조**
- 월 구독: 시나리오 N개 × 주 M회 자동 생성
- 예: 월 ₩500,000~₩1,500,000 (시나리오 5개 × 주 2회 = 월 40개 영상)

---

## 10. 프롬프트 한국어 해설 (시나리오 1 - S1 이미지 프롬프트)

위 프롬프트의 각 키워드가 왜 포함되었는지 한국어로 해설합니다:

**원문 핵심 파트별 해설:**

- **"Close-up shot"**: 얼굴과 팔뚝의 디테일을 잡기 위해 클로즈업 지정. UGC 숏폼에서는 클로즈업이 시선을 잡는 핵심.
- **"young Korean woman in her mid-20s"**: 타겟 페르소나 A(20대 여성)와 동일한 인물 설정. 공감대 형성의 핵심.
- **"looking down at her upper arm with a slightly worried expression"**: 모공각화증 고민을 표정으로 전달. "완전 걱정"이 아니라 "살짝 걱정"으로 자연스러움 유지.
- **"bright modern bathroom"**: 한국 20대 여성의 실제 생활 공간 반영. 너무 럭셔리하면 비현실적, 너무 허름하면 브랜드 이미지 하락.
- **"soft natural window light from the left side"**: 자연광이 왼쪽에서 들어오도록 지정하여 피부 질감과 입체감을 극대화.
- **"shot on Sony A7R V with Sony FE 85mm f/1.4 GM"**: 실제 카메라 + 렌즈 모델을 지정. AI에게 "이 카메라로 찍은 것 같은 룩"의 정확한 기준점을 제공. 85mm f/1.4는 인물 포트레이트의 표준 황금 조합.
- **"ISO 400, color temperature 5200K"**: 실내 자연광 환경에 맞는 ISO와 색온도. 너무 높은 ISO는 노이즈 과다, 5200K는 자연스러운 일광 톤.
- **극사실주의 블록 전체**: CLAUDE.md의 Weavy.ai 극사실주의 규칙에 따라 필수 삽입. "visible pores", "natural asymmetric facial features", "ZERO artificial smooth skin" 등은 AI가 "인형 같은 매끈한 피부"를 만드는 것을 강력하게 차단.

---

*본 기획서는 FlowPilot에서 작성되었습니다.*
*문의: office@flowpilots.org*
