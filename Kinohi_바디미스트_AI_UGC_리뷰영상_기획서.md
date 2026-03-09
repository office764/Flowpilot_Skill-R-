# 키노히 바디미스트(santal) — AI UGC 리뷰 영상 기획서

## FlowPilot 컨텐츠 마이닝 C.01 | 제품 리뷰 컨텐츠 전용

---

## 1. 프로젝트 요약

| 항목 | 내용 |
|------|------|
| 브랜드 | Kinohi (키노히) |
| 제품 | 바디미스트 santal (200ml / 30,000원) |
| 컨텐츠 유형 | AI UGC 제품 리뷰 영상 (인물 토킹 + 제품 사용 장면) |
| 플랫폼 | 인스타그램 릴스 (9:16 세로형) |
| 영상 길이 | 15~30초 |
| 사용 도구 | Kie.ai API (Flux Kontext + InfiniteTalk/Kling Avatar + Veo 3.1) + n8n |

---

## 2. 실제 리뷰 분석 → 셀링 포인트 도출

### 리뷰 원문 요약

| # | 날짜 | 채널 | 제목 | 핵심 키워드 |
|---|------|------|------|-----------|
| R1 | 2025.02 | 29CM | "로션보다 훨씬 사용하기 간편해요!" | 모공각화증, 간편함, 흡수 빠름, 향 좋음 |
| R2 | 2025.05 | 스마트스토어 | "끈적임 없이 바로 스며들어요!" | 수분감, 끈적임 ZERO, 촉촉함 오래 유지, 바디크림 대체 |
| R3 | 2024.05 | 자사몰 | "은은한 향기가 좋아요!" | 분사력 좋음, 은은한 향, 향수 대체, 데일리 사용 |
| R4 | 2024.09 | 스마트스토어 | "보습감이랑 수분력 유지가 좋아요!" | 보습감, 수분력, 심신 안정, 편안함, 재구매 |

### 셀링 포인트 5개

| SP | 셀링 포인트 | 영상 훅(Hook) 문장 |
|----|-----------|-----------------|
| SP1 | 모공각화증에 효과 체감 | "팔뚝 닭살 때문에 반팔 못 입었는데, 이거 쓰고 달라졌어요" |
| SP2 | 바디크림 대체 (간편함) | "바디크림 바르기 귀찮은 사람? 뿌리기만 하면 끝" |
| SP3 | 즉각 보습 + 끈적임 ZERO | "뿌리자마자 스며드는데 끈적임이 없어요" |
| SP4 | 은은한 향 = 향수 대체 | "향수 대신 매일 이거 뿌리는데 계속 뭐 뿌렸냐고 물어봐요" |
| SP5 | 수면 전 루틴 / 심신 안정 | "자기 전에 뿌리면 산속에 온 것 같아요. 잠이 솔솔" |

---

## 3. AI UGC 리뷰 영상 제작 파이프라인

### 3-1. 전체 흐름 (6단계)

```
[STEP 1] AI 리뷰어 캐릭터 이미지 생성 (Flux Kontext Pro via Kie.ai)
   ↓
[STEP 2] 리뷰 나레이션 스크립트 작성 (Claude AI / 수동)
   ↓
[STEP 3] 나레이션 음성 생성 (TTS — ElevenLabs API)
   ↓
[STEP 4] AI 토킹 아바타 영상 생성 (InfiniteTalk API via Kie.ai)
   ↓
[STEP 5] 제품 사용 장면 B-Roll 영상 생성 (Veo 3.1 via Kie.ai)
   ↓
[STEP 6] 최종 편집 — 토킹 영상 + B-Roll + 텍스트 오버레이 + BGM (수동: CapCut/Premiere)
```

### 3-2. 각 STEP 상세 설명

---

### STEP 1: AI 리뷰어 캐릭터 이미지 생성

**사용 API**: Kie.ai — Flux Kontext Pro
**엔드포인트**: `POST https://api.kie.ai/api/v1/flux/kontext/generate`

**목적**: 리뷰 영상에 등장할 AI 인물(한국 20대 여성)의 정면 초상화를 생성. 이 이미지가 STEP 4에서 토킹 아바타의 기반 이미지가 됨.

**요청 JSON:**
```json
{
  "prompt": "Front-facing portrait photograph of a natural-looking young Korean woman aged 24 in a bright clean modern bathroom, she is wearing a simple white cotton t-shirt, her hair is slightly damp and loosely tied back, she has minimal no-makeup makeup look, she is looking directly at the camera with a friendly approachable expression and a slight natural smile, soft diffused natural daylight coming from a window on the left side, the background shows a clean modern bathroom with white tiles slightly out of focus, shot on Sony A7IV with Sony FE 35mm f/1.4 GM lens at f/2.0, ISO 400, color temperature 5200K, medium close-up framing from chest up, hyper-realistic photorealistic photograph indistinguishable from real DSLR photo, ultra-detailed skin texture with visible pores and micro skin imperfections and fine peach fuzz hair and natural skin blemishes and subtle freckles, real human skin subsurface scattering with natural blood vessel undertones, individual hair strands visible with natural flyaway hairs and split ends, eyes with detailed iris fibers and natural eye moisture reflection and visible blood vessels in sclera, natural asymmetric facial features as real humans have, natural lip texture with visible lip lines and slight dryness, shot on real camera sensor with natural lens aberration and chromatic fringing at edges, realistic depth of field with optical bokeh circles, natural film grain matching ISO setting, absolutely ZERO artificial smooth airbrushed plastic skin, ZERO uncanny valley effect, ZERO CGI render look, ZERO digital art style, ZERO perfect symmetry, must pass as genuine unedited photograph taken by professional photographer",
  "model": "flux-kontext-pro",
  "aspectRatio": "9:16",
  "outputFormat": "png",
  "enableTranslation": false,
  "safetyTolerance": 2
}
```

**한국어 프롬프트 해설:**
- `Front-facing portrait`: 정면 초상화 — InfiniteTalk 토킹 아바타 입력에 정면 얼굴이 필수
- `young Korean woman aged 24`: 타겟 페르소나(20대 여성)와 동일한 인물 설정으로 공감대 형성
- `bright clean modern bathroom`: 바디미스트 사용 환경(욕실)을 자연스럽게 연출
- `slightly damp hair loosely tied back`: 샤워 직후 느낌으로 바디미스트 사용 상황 암시
- `minimal no-makeup makeup look`: 과하지 않은 자연스러운 메이크업 — UGC 리얼함의 핵심
- `looking directly at camera`: 카메라 직시 — 리뷰 영상에서 시청자와 눈 맞춤 효과
- `Sony A7IV + Sony FE 35mm f/1.4 GM at f/2.0`: 실제 카메라+렌즈 지정으로 AI에게 사실적인 룩 기준점 제공
- `극사실주의 블록 전체`: visible pores, natural asymmetric features 등으로 AI가 인형 같은 매끈한 피부를 만드는 것을 차단

**이 이미지에서 확인해야 할 것:**
- 정면을 보고 있는가? (측면이면 토킹 아바타 품질 저하)
- 입이 자연스럽게 닫혀 있는가? (립싱크 시작점)
- 조명이 균일한가? (한쪽이 너무 어두우면 영상에서 부자연스러움)

---

### STEP 2: 리뷰 나레이션 스크립트 작성

**목적**: 각 셀링 포인트별로 실제 리뷰어가 말하는 듯한 자연스러운 한국어 나레이션 스크립트 작성

#### 스크립트 A: 모공각화증 리뷰 (SP1 기반 / 약 20초)

```
(밝고 솔직한 톤)
"나 원래 팔뚝 닭살이 심해서 여름에도 반팔 잘 못 입었거든?
근데 이 키노히 바디미스트가 모공각화증에 좋다길래 한번 써봤는데,
로션보다 훨씬 간편하고, 뿌리자마자 바로 스며들어.
끈적임도 아예 없고, 쓴 지 좀 되니까 확실히 피부결이 달라졌어.
향도 은은하게 나무 향 같은 게 진짜 좋거든.
팔뚝 닭살 고민이면 한번 써봐, 진짜 추천."
```

#### 스크립트 B: 바디크림 대체 리뷰 (SP2+SP3 기반 / 약 18초)

```
(편안하고 일상적인 톤)
"나 진짜 바디크림 바르는 거 너무 귀찮아하는 사람인데,
이거 알고 나서 인생이 바뀌었어.
샤워하고 나와서 그냥 뿌리기만 하면 끝이야.
근데 수분감이 진짜 오래 가거든? 끈적임은 아예 없고.
바디크림 귀찮은 사람 이걸로 완전 대체 가능.
키노히 바디미스트 santal, 진심 추천."
```

#### 스크립트 C: 향수 대체 리뷰 (SP4 기반 / 약 18초)

```
(감성적이고 부드러운 톤)
"나 요즘 향수 대신 매일 이거 뿌려.
키노히 바디미스트인데, 뿌리면 은은하게 나무 향이 퍼져.
부담스럽지 않은데 계속 뭐 뿌렸냐고 물어봐 주변에서.
보습도 되면서 향까지 되니까 일석이조지.
카다몬이랑 샌달우드 향인데 산속 공기 같은 느낌이야.
데일리로 쓰기 딱이야, 진짜."
```

#### 스크립트 D: 수면 루틴 리뷰 (SP5 기반 / 약 17초)

```
(차분하고 조용한 톤)
"자기 전에 이거 뿌리면 진짜 잠이 솔솔 와.
키노히 바디미스트인데, 나무껍질 향이랑 좀 청량한 향이 섞여 있어.
뿌리면 마치 산속에 온 것 같은 느낌이 들면서 마음이 편안해져.
보습도 되니까 자고 일어나면 피부도 촉촉하고.
잠 잘 안 오는 사람한테 진짜 강추야."
```

#### 스크립트 E: 올인원 데일리 루틴 (SP2+SP3+SP4 종합 / 약 20초)

```
(밝고 에너지 있는 톤)
"내 데일리 바디케어 루틴 공유할게.
샤워하고 나와서 키노히 바디미스트 온몸에 뿌려.
이것만 하면 보습 끝이야, 바디크림 안 발라도 돼.
끈적임 없이 바로 스며드니까 바로 옷 입어도 되고,
향이 은은하게 하루 종일 가거든.
향수 따로 안 뿌려도 돼서 완전 간편해.
키노히 santal, 데일리 필수템 인정."
```

---

### STEP 3: 나레이션 음성 생성 (TTS)

**사용 API**: ElevenLabs Multilingual V2
**엔드포인트**: `POST https://api.elevenlabs.io/v1/text-to-speech/{voice_id}`

**voice_id 선정 기준:**
- 한국어 여성 음성
- 20대 느낌의 밝고 자연스러운 톤
- ElevenLabs Voice Library에서 한국어 지원 음성 검색 후 선택
- 또는 Voice Cloning으로 커스텀 음성 생성

**요청 JSON:**
```json
{
  "text": "(스크립트 A~E 중 해당 텍스트)",
  "model_id": "eleven_multilingual_v2",
  "voice_settings": {
    "stability": 0.45,
    "similarity_boost": 0.70,
    "style": 0.35,
    "use_speaker_boost": true
  },
  "output_format": "mp3_44100_128"
}
```

**파라미터 해설:**
- `stability: 0.45`: 약간 낮춰서 자연스러운 억양 변화 유도 (너무 높으면 로봇 느낌)
- `similarity_boost: 0.70`: 원본 음성 특성 유지하면서 자연스러움 확보
- `style: 0.35`: 약간의 감정 표현 추가 (리뷰 영상 특성상 감정이 있어야 자연스러움)

**출력**: MP3 파일 (15~20초) → STEP 4의 `audio_url` 입력으로 사용

---

### STEP 4: AI 토킹 아바타 영상 생성

**사용 API**: Kie.ai — InfiniteTalk (MeiGen-AI)
**엔드포인트**: `POST https://api.kie.ai/api/v1/jobs/createTask`

**목적**: STEP 1에서 만든 인물 이미지 + STEP 3에서 만든 음성 파일을 합쳐서 → 립싱크가 맞는 토킹 아바타 영상을 생성

**요청 JSON:**
```json
{
  "model": "infinitalk/from-audio",
  "params": {
    "image_url": "(STEP 1에서 생성된 인물 이미지 URL)",
    "audio_url": "(STEP 3에서 생성된 나레이션 MP3 URL)",
    "prompt": "Natural Korean woman in her 20s speaking casually and expressively to camera in a bathroom setting, subtle head movements, natural blinking, friendly facial expressions matching conversational review tone, slight smile, relaxed shoulders",
    "resolution": "720p",
    "seed": 42
  },
  "callBackUrl": ""
}
```

**파라미터 해설:**
- `image_url`: STEP 1의 정면 초상화 — 이것이 영상의 기반 얼굴이 됨
- `audio_url`: STEP 3의 나레이션 MP3 — 이것에 맞춰 입이 움직임
- `prompt`: 표정/제스처/분위기 지시 — "자연스러운 한국 여성, 카메라에 대고 캐주얼하게 말하는" 느낌
- `resolution: 720p`: 릴스 업로드용으로 충분한 해상도 (480p는 화질 저하 위험)

**가격**: 720p 기준 약 12크레딧/초 → 20초 영상 = 240크레딧 ≈ $1.20

**출력**: 토킹 아바타 MP4 영상 (15~20초) — 인물이 나레이션에 맞춰 입을 움직이며 말하는 영상

**품질 체크 포인트:**
- 립싱크가 한국어 발음과 맞는가?
- 표정이 자연스러운가? (무표정 고정이면 재생성)
- 머리/어깨의 미세한 움직임이 있는가?

---

### STEP 5: 제품 사용 장면 B-Roll 영상 생성

**사용 API**: Kie.ai — Veo 3.1 Fast
**엔드포인트**: `POST https://api.kie.ai/api/v1/veo/generate`

**목적**: 토킹 아바타가 말하는 중간중간에 삽입할 "제품 사용 장면" B-Roll 클립 생성. 실제 UGC 리뷰 영상처럼 "말하면서 중간에 제품 쓰는 장면"이 교차 편집되는 구조.

**B-Roll 클립 목록 (시나리오 공통):**

#### B-Roll 1: 제품 클로즈업 (3초)

**요청 JSON:**
```json
{
  "prompt": "Vertical 9:16 cinematic close-up shot of a Kinohi body mist santal frosted glass bottle being held in a young woman's hand, her fingers gently wrapped around the bottle, the minimalist KINOHI branding clearly visible on the label, soft warm bathroom lighting from above, the bottle has a subtle sage green to cream gradient, clean white bathroom counter background slightly blurred, slow subtle rotation of the bottle, shot on Sony A7RV with 90mm macro lens at f/2.8, ISO 200, color temperature 4800K, hyper-realistic footage indistinguishable from real cinema camera capture, natural motion blur consistent with real camera shutter speed, absolutely ZERO AI-generated look, must pass as genuine footage from professional cinema camera",
  "model": "veo3_fast",
  "aspect_ratio": "9:16",
  "enableTranslation": false
}
```

**프롬프트 해설:**
- `Vertical 9:16`: 인스타 릴스 세로형 지정
- `frosted glass bottle being held in a young woman's hand`: 손에 들고 있는 장면 → 실사용 느낌
- `slow subtle rotation`: 살짝 돌리는 동작으로 제품 라벨 자연스럽게 노출
- `KINOHI branding clearly visible`: 브랜드명이 보여야 광고 효과

#### B-Roll 2: 팔에 미스트 뿌리는 장면 (4초)

**요청 JSON:**
```json
{
  "prompt": "Vertical 9:16 close-up shot of a young Korean woman spraying body mist onto her bare forearm in a bright modern bathroom, fine mist particles catching the soft window light creating a beautiful spray effect, her skin is slightly damp from shower with tiny water droplets visible, the spray creates a delicate mist cloud around her arm, she wears a white bathrobe with sleeve rolled up, camera angle is slightly above looking down at the arm, shot on Canon EOS R5 with RF 50mm f/1.2 at f/2.0, ISO 400, color temperature 5000K, hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin with visible pores and natural skin texture movement during motion, natural motion blur consistent with real camera shutter speed, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, must pass as genuine footage from professional cinema camera",
  "model": "veo3_fast",
  "aspect_ratio": "9:16",
  "enableTranslation": false
}
```

**프롬프트 해설:**
- `spraying body mist onto her bare forearm`: 핵심 사용 장면 — 팔에 뿌리는 모습
- `fine mist particles catching the soft window light`: 미스트 입자가 빛에 반짝이는 연출 → 시각적 만족감
- `skin is slightly damp from shower`: 샤워 직후 설정 → 리뷰 스크립트와 일치

#### B-Roll 3: 피부 흡수 클로즈업 (3초)

**요청 JSON:**
```json
{
  "prompt": "Vertical 9:16 extreme close-up macro shot of Korean woman's forearm skin surface after applying body mist, the skin shows a natural healthy dewy glow with micro water droplets being absorbed into the skin, you can see fine skin texture and natural pores in incredible detail, the moisture creates a subtle sheen that slowly settles into the skin showing absorption, soft warm diffused lighting from above, shot on Canon EOS R5 with RF 100mm f/2.8L Macro at f/4.0, ISO 320, color temperature 5000K, hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin with visible pores and natural texture, absolutely ZERO artificial smooth plastic skin, ZERO AI-generated look, must pass as genuine footage",
  "model": "veo3_fast",
  "aspect_ratio": "9:16",
  "enableTranslation": false
}
```

**프롬프트 해설:**
- `extreme close-up macro`: 피부 결을 극대화하여 "흡수되는 느낌"을 시각적으로 전달
- `micro water droplets being absorbed`: 수분이 흡수되는 과정 — SP3(즉각 보습) 시각화

#### B-Roll 4: 손목에 뿌리고 향 맡는 장면 (3초)

**요청 JSON:**
```json
{
  "prompt": "Vertical 9:16 medium close-up shot of a young Korean woman spraying body mist on her inner wrist and then gently bringing her wrist close to her nose to smell it with eyes slightly closed and a subtle pleased smile, soft natural daylight in a clean bedroom setting, she wears a casual comfortable outfit, her expression shows genuine enjoyment of the fragrance, shot on Sony A7IV with 35mm f/1.4 GM at f/2.0, ISO 400, color temperature 5200K, hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin with visible pores and micro expressions and natural skin texture, realistic hair physics with individual strands, natural subtle body micro-movements like breathing, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, must pass as genuine footage from professional cinema camera",
  "model": "veo3_fast",
  "aspect_ratio": "9:16",
  "enableTranslation": false
}
```

**프롬프트 해설:**
- `spraying on inner wrist then bringing to nose`: 향수처럼 손목에 뿌리고 향 맡는 동작 → SP4(향수 대체) 시각화
- `eyes slightly closed and subtle pleased smile`: 향을 맡고 만족하는 표정 → 감성적 연출

#### B-Roll 5: 엔딩 제품샷 (3초)

**요청 JSON:**
```json
{
  "prompt": "Vertical 9:16 elegant product shot of Kinohi body mist santal bottle standing upright on a light natural wood surface, soft morning sunlight streaming in from the right creating warm golden highlights on the frosted glass bottle, a few dried cardamom pods and small sandalwood chips scattered artistically beside the bottle, minimal sage green and cream color palette, clean bright background with soft shadows, the bottle label KINOHI and santal text clearly readable, shot on Hasselblad X2D 100C with XCD 90mm at f/4.0, ISO 100, color temperature 5000K, hyper-realistic product photography indistinguishable from real studio photograph, ultra-detailed material textures with visible surface micro-texture and natural light interaction, realistic glass refraction and reflection with visible internal light caustics, natural shadow falloff with realistic ambient occlusion, absolutely ZERO CGI render look, ZERO 3D modeling aesthetic, must pass as genuine product photo from commercial studio shoot",
  "model": "veo3_fast",
  "aspect_ratio": "9:16",
  "enableTranslation": false
}
```

**프롬프트 해설:**
- `standing upright on light natural wood surface`: 깔끔한 제품 스탠딩 샷
- `cardamom pods and sandalwood chips`: 향 노트 성분을 시각적 소품으로 배치
- `KINOHI and santal text clearly readable`: 브랜드명+제품명 가독성 확보 — CTA 직전 엔딩샷

---

### STEP 6: 최종 편집 (수동 — CapCut 또는 Premiere Pro)

**편집 타임라인 구조 (스크립트 A 기준 / 약 25초):**

```
[0~3초]  B-Roll 1 (제품 클로즈업) + 나레이션 시작
         텍스트 오버레이: "팔뚝 닭살 때문에 반팔 못 입었는데..."

[3~10초] 토킹 아바타 영상 (인물이 카메라 보고 말하는 장면)
         나레이션: "이 키노히 바디미스트가 모공각화증에 좋다길래..."

[10~14초] B-Roll 2 (팔에 미스트 뿌리는 장면)
          나레이션: "로션보다 훨씬 간편하고, 뿌리자마자 바로 스며들어"

[14~17초] B-Roll 3 (피부 흡수 클로즈업)
          나레이션: "끈적임도 아예 없고, 확실히 피부결이 달라졌어"

[17~21초] 토킹 아바타 영상 (말하는 장면 계속)
          나레이션: "향도 은은하게 나무 향 같은 게 진짜 좋거든"

[21~23초] B-Roll 4 (손목에 뿌리고 향 맡는 장면)

[23~25초] B-Roll 5 (제품 엔딩샷)
          텍스트 오버레이: "키노히 바디미스트 santal | 30,000원"
          CTA: "프로필 링크에서 구매 →"
```

**편집 시 필수 추가 요소:**
- BGM: 잔잔한 어쿠스틱/lo-fi (저작권 프리)
- 자막: 나레이션 전체 한국어 자막 (릴스에서 무음 시청자 대응)
- 텍스트 스타일: 흰색 + 그림자, 하단 배치, 키노히 브랜드 컬러(세이지 그린) 포인트
- 트랜지션: 토킹 ↔ B-Roll 전환 시 컷 또는 부드러운 디졸브

---

## 4. n8n 워크플로우 노드 상세 기획

### 전체 워크플로우 구조

```
[Node 1: Webhook]
  → [Node 2: Set — 시나리오 데이터]
  → [Node 3: Code — 프롬프트 로드]
  → [Node 4: HTTP Request — Flux Kontext (인물 이미지 생성)]
  → [Node 5: Wait 30초]
  → [Node 6: HTTP Request — Task 결과 조회 (이미지 URL 획득)]
  → [Node 7: HTTP Request — ElevenLabs TTS (나레이션 생성)]
  → [Node 8: HTTP Request — 나레이션 MP3를 파일 호스팅에 업로드]
  → [Node 9: HTTP Request — InfiniteTalk (토킹 아바타 생성)]
  → [Node 10: Wait 60초]
  → [Node 11: HTTP Request — Task 결과 조회 (토킹 영상 URL)]
  → [Node 12: Loop — B-Roll 5개 순차 생성]
    → [Node 12a: HTTP Request — Veo 3.1 (B-Roll 영상 생성)]
    → [Node 12b: Wait 90초]
    → [Node 12c: HTTP Request — Task 결과 조회]
  → [Node 13: Set — 전체 결과 집계]
  → [Node 14: Google Sheets — Append Row (에셋 기록)]
  → [Node 15: Slack — 완료 알림]
```

---

### Node 1: Webhook (트리거)

- **노드 타입**: Webhook
- **HTTP Method**: `POST`
- **Path**: `kinohi-review-ugc`
- **Authentication**: Header Auth
- **Response Mode**: `Last Node`

**수신 JSON:**
```json
{
  "script_id": "A",
  "script_text": "나 원래 팔뚝 닭살이 심해서 여름에도 반팔 잘 못 입었거든? 근데 이 키노히 바디미스트가 모공각화증에 좋다길래 한번 써봤는데, 로션보다 훨씬 간편하고, 뿌리자마자 바로 스며들어. 끈적임도 아예 없고, 쓴 지 좀 되니까 확실히 피부결이 달라졌어. 향도 은은하게 나무 향 같은 게 진짜 좋거든. 팔뚝 닭살 고민이면 한번 써봐, 진짜 추천.",
  "selling_point": "SP1",
  "voice_id": "(ElevenLabs 한국어 여성 voice_id)"
}
```

---

### Node 2: Set (시나리오 데이터 매핑)

- **노드 타입**: Set
- **Mode**: Manual Mapping
- **필드:**
  - `script_id` → `{{ $json.script_id }}`
  - `script_text` → `{{ $json.script_text }}`
  - `selling_point` → `{{ $json.selling_point }}`
  - `voice_id` → `{{ $json.voice_id }}`
  - `aspect_ratio` → `9:16`
  - `brand` → `Kinohi`
  - `product` → `바디미스트 santal`

---

### Node 3: Code (프롬프트 로드)

- **노드 타입**: Code
- **Language**: JavaScript

```javascript
const scriptId = $input.first().json.script_id;

// 인물 이미지 프롬프트 (모든 스크립트 공통)
const characterPrompt = "Front-facing portrait photograph of a natural-looking young Korean woman aged 24 in a bright clean modern bathroom, she is wearing a simple white cotton t-shirt, her hair is slightly damp and loosely tied back, she has minimal no-makeup makeup look, she is looking directly at the camera with a friendly approachable expression and a slight natural smile, soft diffused natural daylight coming from a window on the left side, the background shows a clean modern bathroom with white tiles slightly out of focus, shot on Sony A7IV with Sony FE 35mm f/1.4 GM lens at f/2.0, ISO 400, color temperature 5200K, medium close-up framing from chest up, hyper-realistic photorealistic photograph indistinguishable from real DSLR photo, ultra-detailed skin texture with visible pores and micro skin imperfections and fine peach fuzz hair and natural skin blemishes and subtle freckles, real human skin subsurface scattering with natural blood vessel undertones, individual hair strands visible with natural flyaway hairs and split ends, eyes with detailed iris fibers and natural eye moisture reflection and visible blood vessels in sclera, natural asymmetric facial features as real humans have, natural lip texture with visible lip lines and slight dryness, shot on real camera sensor with natural lens aberration and chromatic fringing at edges, realistic depth of field with optical bokeh circles, natural film grain matching ISO setting, absolutely ZERO artificial smooth airbrushed plastic skin, ZERO uncanny valley effect, ZERO CGI render look, ZERO digital art style, ZERO perfect symmetry, must pass as genuine unedited photograph taken by professional photographer";

// B-Roll 프롬프트 배열 (5개)
const brollPrompts = [
  {
    id: "broll_1_product_closeup",
    name: "제품 클로즈업",
    prompt: "Vertical 9:16 cinematic close-up shot of a Kinohi body mist santal frosted glass bottle being held in a young woman's hand, her fingers gently wrapped around the bottle, the minimalist KINOHI branding clearly visible on the label, soft warm bathroom lighting from above, the bottle has a subtle sage green to cream gradient, clean white bathroom counter background slightly blurred, slow subtle rotation of the bottle, shot on Sony A7RV with 90mm macro lens at f/2.8, ISO 200, color temperature 4800K, hyper-realistic footage indistinguishable from real cinema camera capture, natural motion blur consistent with real camera shutter speed, absolutely ZERO AI-generated look, must pass as genuine footage from professional cinema camera"
  },
  {
    id: "broll_2_spray_arm",
    name: "팔에 미스트 뿌리기",
    prompt: "Vertical 9:16 close-up shot of a young Korean woman spraying body mist onto her bare forearm in a bright modern bathroom, fine mist particles catching the soft window light creating a beautiful spray effect, her skin is slightly damp from shower with tiny water droplets visible, the spray creates a delicate mist cloud around her arm, she wears a white bathrobe with sleeve rolled up, camera angle is slightly above looking down at the arm, shot on Canon EOS R5 with RF 50mm f/1.2 at f/2.0, ISO 400, color temperature 5000K, hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin with visible pores and natural skin texture movement during motion, natural motion blur consistent with real camera shutter speed, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, must pass as genuine footage from professional cinema camera"
  },
  {
    id: "broll_3_absorption",
    name: "피부 흡수 클로즈업",
    prompt: "Vertical 9:16 extreme close-up macro shot of Korean woman's forearm skin surface after applying body mist, the skin shows a natural healthy dewy glow with micro water droplets being absorbed into the skin, you can see fine skin texture and natural pores in incredible detail, the moisture creates a subtle sheen that slowly settles into the skin showing absorption, soft warm diffused lighting from above, shot on Canon EOS R5 with RF 100mm f/2.8L Macro at f/4.0, ISO 320, color temperature 5000K, hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin with visible pores and natural texture, absolutely ZERO artificial smooth plastic skin, ZERO AI-generated look, must pass as genuine footage"
  },
  {
    id: "broll_4_wrist_smell",
    name: "손목에 뿌리고 향 맡기",
    prompt: "Vertical 9:16 medium close-up shot of a young Korean woman spraying body mist on her inner wrist and then gently bringing her wrist close to her nose to smell it with eyes slightly closed and a subtle pleased smile, soft natural daylight in a clean bedroom setting, she wears a casual comfortable outfit, her expression shows genuine enjoyment of the fragrance, shot on Sony A7IV with 35mm f/1.4 GM at f/2.0, ISO 400, color temperature 5200K, hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin with visible pores and micro expressions and natural skin texture, realistic hair physics with individual strands, natural subtle body micro-movements like breathing, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, must pass as genuine footage from professional cinema camera"
  },
  {
    id: "broll_5_product_ending",
    name: "엔딩 제품샷",
    prompt: "Vertical 9:16 elegant product shot of Kinohi body mist santal bottle standing upright on a light natural wood surface, soft morning sunlight streaming in from the right creating warm golden highlights on the frosted glass bottle, a few dried cardamom pods and small sandalwood chips scattered artistically beside the bottle, minimal sage green and cream color palette, clean bright background with soft shadows, the bottle label KINOHI and santal text clearly readable, shot on Hasselblad X2D 100C with XCD 90mm at f/4.0, ISO 100, color temperature 5000K, hyper-realistic product photography indistinguishable from real studio photograph, ultra-detailed material textures with visible surface micro-texture and natural light interaction, realistic glass refraction and reflection with visible internal light caustics, natural shadow falloff with realistic ambient occlusion, absolutely ZERO CGI render look, ZERO 3D modeling aesthetic, must pass as genuine product photo from commercial studio shoot"
  }
];

// 토킹 아바타 프롬프트
const talkingPrompt = "Natural Korean woman in her 20s speaking casually and expressively to camera in a bathroom setting, subtle head movements, natural blinking, friendly facial expressions matching conversational review tone, slight smile, relaxed shoulders";

return [{
  json: {
    characterPrompt,
    brollPrompts,
    talkingPrompt,
    scriptText: $input.first().json.script_text,
    voiceId: $input.first().json.voice_id
  }
}];
```

---

### Node 4: HTTP Request — 인물 이미지 생성 (Flux Kontext)

- **노드 타입**: HTTP Request
- **Method**: `POST`
- **URL**: `https://api.kie.ai/api/v1/flux/kontext/generate`
- **Authentication**: Generic Credential Type → Header Auth
  - Name: `Authorization`
  - Value: `Bearer (Kie.ai API Key — Credential에서 관리)`
- **Body Content Type**: JSON
- **JSON Body**:

```json
{
  "prompt": "{{ $json.characterPrompt }}",
  "model": "flux-kontext-pro",
  "aspectRatio": "9:16",
  "outputFormat": "png",
  "enableTranslation": false,
  "safetyTolerance": 2
}
```

---

### Node 5: Wait (이미지 생성 대기)

- **노드 타입**: Wait
- **Resume**: After Time Interval
- **Amount**: `30` 초

---

### Node 6: HTTP Request — 이미지 결과 조회

- **노드 타입**: HTTP Request
- **Method**: `GET`
- **URL**: `https://api.kie.ai/api/v1/task/{{ $('Node 4').item.json.data.taskId }}`
- **Authentication**: 동일 Bearer Token
- **추출**: `data.info.resultUrls[0]` → 인물 이미지 URL

> 이 URL을 STEP 4 (InfiniteTalk)의 `image_url`로 사용

---

### Node 7: HTTP Request — ElevenLabs TTS

- **노드 타입**: HTTP Request
- **Method**: `POST`
- **URL**: `https://api.elevenlabs.io/v1/text-to-speech/{{ $('Set').item.json.voice_id }}`
- **Authentication**: Header Auth
  - Name: `xi-api-key`
  - Value: `(ElevenLabs API Key)`
- **Body Content Type**: JSON
- **JSON Body**:

```json
{
  "text": "{{ $json.scriptText }}",
  "model_id": "eleven_multilingual_v2",
  "voice_settings": {
    "stability": 0.45,
    "similarity_boost": 0.70,
    "style": 0.35,
    "use_speaker_boost": true
  }
}
```

- **Response Format**: Binary (MP3 파일 수신)

---

### Node 8: HTTP Request — MP3 파일 호스팅 업로드

- **노드 타입**: HTTP Request
- **목적**: InfiniteTalk에 `audio_url`을 전달하려면 MP3가 공개 URL로 접근 가능해야 함
- **방법 (택 1)**:
  - A) Google Drive 업로드 → 공유 링크 생성
  - B) AWS S3 / Cloudflare R2에 PUT 업로드
  - C) n8n 서버의 정적 파일 서빙 (데모 한정)

**S3 업로드 예시:**
- **Method**: `PUT`
- **URL**: `https://your-bucket.s3.amazonaws.com/kinohi-ugc/narration_{{ $('Set').item.json.script_id }}.mp3`
- **Authentication**: AWS Credentials
- **Send Binary Data**: Yes
- **Output**: 업로드된 파일의 공개 URL

---

### Node 9: HTTP Request — InfiniteTalk (토킹 아바타)

- **노드 타입**: HTTP Request
- **Method**: `POST`
- **URL**: `https://api.kie.ai/api/v1/jobs/createTask`
- **Authentication**: Bearer Token (Kie.ai)
- **JSON Body**:

```json
{
  "model": "infinitalk/from-audio",
  "params": {
    "image_url": "{{ $('Node 6').item.json.data.info.resultUrls[0] }}",
    "audio_url": "{{ $('Node 8').item.json.uploaded_url }}",
    "prompt": "{{ $json.talkingPrompt }}",
    "resolution": "720p",
    "seed": 42
  }
}
```

---

### Node 10: Wait (토킹 영상 생성 대기)

- **노드 타입**: Wait
- **Amount**: `60` 초

---

### Node 11: HTTP Request — 토킹 영상 결과 조회

- **노드 타입**: HTTP Request
- **Method**: `GET`
- **URL**: `https://api.kie.ai/api/v1/task/{{ $('Node 9').item.json.data.taskId }}`
- **Authentication**: Bearer Token
- **추출**: `data.info.resultUrls[0]` → 토킹 아바타 영상 URL

---

### Node 12: Loop Over Items — B-Roll 5개 순차 생성

#### Node 12-진입: Split In Batches

- **노드 타입**: Split In Batches (Loop Over Items)
- **Batch Size**: `1`
- **입력**: Code 노드에서 생성한 `brollPrompts` 배열 (5개)

#### Node 12a: HTTP Request — Veo 3.1 B-Roll 생성

- **Method**: `POST`
- **URL**: `https://api.kie.ai/api/v1/veo/generate`
- **JSON Body**:

```json
{
  "prompt": "{{ $json.prompt }}",
  "model": "veo3_fast",
  "aspect_ratio": "9:16",
  "enableTranslation": false
}
```

#### Node 12b: Wait

- **Amount**: `90` 초

#### Node 12c: HTTP Request — B-Roll 결과 조회

- **Method**: `GET`
- **URL**: `https://api.kie.ai/api/v1/task/{{ $('Node 12a').item.json.data.taskId }}`
- **추출**: 영상 URL

---

### Node 13: Set — 전체 결과 집계

- **노드 타입**: Set
- **필드:**
  - `script_id`
  - `character_image_url` (Node 6 결과)
  - `talking_video_url` (Node 11 결과)
  - `broll_1_url` ~ `broll_5_url` (Node 12c 결과들)
  - `narration_url` (Node 8 결과)
  - `created_at` (현재 시각)

---

### Node 14: Google Sheets — 에셋 기록

- **노드 타입**: Google Sheets
- **Operation**: Append Row
- **Spreadsheet**: `Kinohi UGC Asset Tracker`
- **Sheet**: `리뷰영상_에셋`
- **컬럼 매핑:**

| 컬럼 | 값 |
|------|-----|
| A: script_id | 스크립트 ID (A~E) |
| B: character_image | 인물 이미지 URL |
| C: talking_video | 토킹 아바타 영상 URL |
| D: broll_1 | B-Roll 1 URL |
| E: broll_2 | B-Roll 2 URL |
| F: broll_3 | B-Roll 3 URL |
| G: broll_4 | B-Roll 4 URL |
| H: broll_5 | B-Roll 5 URL |
| I: narration_mp3 | 나레이션 MP3 URL |
| J: status | "generated" |
| K: created_at | 생성 시각 |

---

### Node 15: Slack — 완료 알림

- **노드 타입**: Slack
- **Operation**: Send Message
- **Channel**: `#kinohi-ugc`
- **Message**:

```
키노히 AI UGC 리뷰 영상 에셋 생성 완료!

스크립트: {{ $json.script_id }}
셀링 포인트: {{ $('Set').item.json.selling_point }}

생성된 에셋:
- 인물 이미지: 1장
- 토킹 아바타 영상: 1개
- B-Roll 클립: 5개
- 나레이션 MP3: 1개

다음 단계: CapCut/Premiere에서 최종 편집
에셋 시트에서 URL 확인하세요.
```

---

## 5. 비용 요약 (리뷰 영상 1개당)

| 항목 | API | 수량 | 단가 | 소계 |
|------|-----|------|------|------|
| 인물 이미지 | Flux Kontext Pro | 1장 | ~$0.10 | $0.10 |
| 나레이션 TTS | ElevenLabs | ~20초 | ~$0.05 | $0.05 |
| 토킹 아바타 | InfiniteTalk 720p | ~20초 | $0.06/초 × 20 | $1.20 |
| B-Roll 영상 | Veo 3.1 Fast | 5개 | ~$1.00 | $5.00 |
| **합계** | | | | **~$6.35 (약 ₩8,600)** |

리뷰 영상 5개(스크립트 A~E) 전체 제작 시: **약 $31.75 (약 ₩43,000)**

---

## 6. 데모 vs 고도화

| | 데모 (현재) | 고도화 (계약 후) |
|--|-----------|---------------|
| 트리거 | Webhook 수동 호출 | 노션/Airtable 연동 자동 |
| 스크립트 | 수동 작성 | Claude AI 자동 생성 (리뷰 데이터 기반) |
| 결과 대기 | Wait 노드 (고정 시간) | Callback URL (즉시 다음 단계) |
| MP3 호스팅 | 수동 업로드 | S3 자동 업로드 |
| 최종 편집 | CapCut/Premiere 수동 | FFmpeg 자동 (자막+BGM+합성) |
| 배포 | 수동 업로드 | Instagram API 자동 포스팅 |
| 수동 비중 | ~35% | ~5% (QC 승인만) |

---

*FlowPilot | office@flowpilots.org*
