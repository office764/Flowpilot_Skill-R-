# KINOHI 45초 광고 영상 제작 가이드
## 16:9 숏폼 | FlowPilot x Weavy.ai | 2026.02.28

---

## Part 0. 작업 개요

| 항목 | 값 |
|------|-----|
| 영상 길이 | 45초 |
| 해상도 | 16:9 (1920x1080 또는 4K) |
| 타겟 플랫폼 | 유튜브, 인스타그램 릴스 (가로형) |
| 제품 | KINOHI 룸스프레이 (Woody) |
| 톤 | 시네마틱, 프리미엄, 미니멀, 감성적 |
| 비디오 모델 | Kling 3 (가성비 4K/60fps) |
| 이미지 모델 | Flux 2 Pro (포토리얼 키프레임) |
| LLM 프롬프트 생성 | Run Any LLM 노드 (GPT-4o 또는 Claude) |

---

## Part 1. 초단위 스토리보드 (45초 = 9개 장면)

### 장면 1: 오프닝 — 공간 설정 (0~5초)

| 항목 | 내용 |
|------|------|
| 시간 | 00:00 ~ 00:05 |
| 장면 설명 | 새벽 안개가 걷히는 제주 편백 숲. 카메라가 숲 사이를 천천히 관통하며 전진. 볼류메트릭 안개가 나뭇가지 사이로 흐름. |
| 카메라 | Slow dolly forward through forest |
| 분위기 | 신비롭고 고요한 새벽, 자연의 시작 |
| 자막 | 없음 (순수 비주얼) |
| 오디오 | 새소리 + 바람소리 앰비언스 |

키프레임 이미지 프롬프트:
```
Cinematic 16:9 wide shot of a misty Jeju hinoki cypress forest at dawn, volumetric fog rolling between ancient dark green tree trunks, golden sun rays piercing through canopy creating god rays, morning dew on fern leaves in foreground, cool green and warm gold color palette, Arri Alexa Mini LF cinematic look, Cooke S7 anamorphic lens with oval bokeh, atmospheric depth with multiple fog layers, Terrence Malick nature film aesthetic, 8K resolution, serene mystical forest bathing atmosphere, absolutely NO people NO product in frame
```

Kling 3 비디오 프롬프트:
```
Slow cinematic dolly forward through misty cypress forest at dawn, volumetric fog drifting between tree trunks, golden sun rays shifting as camera moves, morning dew droplets glistening on leaves, gentle wind moving branches slightly, particles floating in light beams, smooth camera movement at 0.3x speed, atmospheric and meditative, 5 seconds duration
```

---

### 장면 2: 전환 — 숲에서 실내로 (5~10초)

| 항목 | 내용 |
|------|------|
| 시간 | 00:05 ~ 00:10 |
| 장면 설명 | 안개가 디졸브되며 미니멀 아파트 거실로 전환. 아침 햇살이 아치형 창문을 통해 들어옴. 린넨 커튼이 부드럽게 흔들림. 대리석 테이블 위에 KINOHI 보틀이 서 있음. |
| 카메라 | Wide establishing → 보틀로 천천히 접근 |
| 분위기 | 자연에서 일상으로의 부드러운 전환 |
| 자막 | 없음 |
| 오디오 | 앰비언스 → 피아노 한 음 시작 |

키프레임 이미지 프롬프트:
```
Cinematic 16:9 wide shot of a minimal luxury Korean apartment living room at golden hour morning, warm sunlight streaming through an arched floor-to-ceiling window with sheer cream linen curtains gently billowing, a white Carrara marble side table positioned in the center-right of frame, a single minimalist cylindrical white bottle labeled KINOHI placed on the marble surface, dried eucalyptus arrangement and small unlit cream candle beside it, white oak flooring, cream sofa partially visible in background, warm color temperature 3200K, shallow depth of field, Arri Alexa Mini LF, anamorphic lens with horizontal flare, luxury fragrance commercial aesthetic, 8K resolution
```

Kling 3 비디오 프롬프트:
```
Cross-dissolve from misty forest to minimal luxury apartment interior, morning golden sunlight streaming through arched window, sheer linen curtains billowing gently in breeze, camera slowly pushes in toward a white marble table where a minimalist KINOHI bottle stands, warm ambient lighting, smooth dolly forward movement, 5 seconds duration, cinematic color grading warm golden tones
```

---

### 장면 3: 제품 히어로 샷 (10~15초)

| 항목 | 내용 |
|------|------|
| 시간 | 00:10 ~ 00:15 |
| 장면 설명 | KINOHI 보틀 클로즈업. 카메라가 45도 각도에서 아이레벨로 천천히 아크 무브. 보틀 표면에 빛이 반사됨. 레이블의 "KINOHI" 텍스트가 선명하게 보임. |
| 카메라 | Slow arc movement 45° → eye level |
| 분위기 | 제품의 프리미엄 존재감 |
| 자막 | 없음 |
| 오디오 | 미니멀 피아노 멜로디 시작 |

키프레임 이미지 프롬프트:
```
Cinematic 16:9 close-up hero shot of KINOHI room spray bottle on white Carrara marble surface, camera at 30-degree angle, warm golden hour side lighting creating dramatic highlights on the cylindrical glass bottle surface, the minimalist white label with KINOHI text clearly visible, a subtle reflection on the polished marble, soft cream bokeh background with blurred arch window, single dried cypress sprig laying beside the bottle, Hasselblad X2D 100C medium format camera sharpness, 100mm f/2.8 lens, warm color temperature, luxury fragrance editorial photography, 8K resolution
```

Kling 3 비디오 프롬프트:
```
Slow cinematic arc camera movement around KINOHI bottle on marble surface from 45-degree angle rotating to eye level, golden light reflections sliding across glass bottle surface as camera moves, subtle light flares, marble surface reflections shifting, background bokeh gently moving, smooth orbiting motion, premium product reveal, 5 seconds duration
```

---

### 장면 4: 손 등장 — 제품 픽업 (15~20초)

| 항목 | 내용 |
|------|------|
| 시간 | 00:15 ~ 00:20 |
| 장면 설명 | 여성의 손이 프레임 왼쪽에서 들어와 KINOHI 보틀을 우아하게 집어 듦. 손톱은 누드톤 매니큐어. 보틀을 들어올리며 빛이 이동. |
| 카메라 | Medium close-up, 정적 카메라 |
| 분위기 | 인간과 제품의 첫 접촉, 우아함 |
| 자막 | 없음 |
| 오디오 | 피아노 + 스트링스 서서히 합류 |

키프레임 이미지 프롬프트:
```
Cinematic 16:9 medium close-up of an elegant Korean woman's hand with natural nude manicure gracefully reaching for and picking up a KINOHI room spray bottle from a white marble surface, the hand is delicate with slender fingers, warm golden side lighting illuminating the skin and bottle, the bottle is being lifted at a slight angle, soft cream linen sleeve of an oversized shirt visible at the wrist, shallow depth of field f/2.0, warm bokeh background with blurred window light, luxury lifestyle photography, Sony A1 with 85mm GM lens, 8K resolution
```

Kling 3 비디오 프롬프트:
```
An elegant woman's hand enters frame from the left side and gracefully picks up a minimalist KINOHI bottle from marble surface, fingers wrap around the bottle and lift it slowly upward, golden light shifts on the bottle surface as it rises, subtle shadow moves on marble, smooth natural hand movement, stationary camera, intimate luxury aesthetic, 5 seconds duration
```

---

### 장면 5: 스프레이 — 미스트 클로즈업 (20~25초)

| 항목 | 내용 |
|------|------|
| 시간 | 00:20 ~ 00:25 |
| 장면 설명 | 보틀 노즐에서 미스트가 분사됨. 슬로우모션으로 미스트 입자가 골든 햇살에 반짝이며 퍼져나감. 미스트 하나하나가 보이는 매크로 수준의 클로즈업. |
| 카메라 | Extreme close-up / Macro, 슬로우모션 |
| 분위기 | 제품의 핵심 순간, 감각적 경험 |
| 자막 | 없음 |
| 오디오 | 스프레이 소리 ASMR + 배경 음악 |

키프레임 이미지 프롬프트:
```
Cinematic 16:9 extreme macro close-up of fine mist spray dispersing from a premium glass bottle nozzle, individual water droplets suspended in warm golden sunlight creating a luminous halo effect, each droplet visible and glistening like tiny diamonds, shallow depth of field with creamy warm bokeh background, the spray creates an ethereal cloud pattern against soft cream backdrop, golden light rays passing through the mist particles, Zeiss Otus 100mm f/1.4 macro lens, 8K ultra detail, every micro droplet captured in freeze-frame clarity, luxury fragrance commercial key moment, ASMR visual aesthetic
```

Kling 3 비디오 프롬프트:
```
Extreme slow-motion close-up of fine mist spraying from bottle nozzle, individual water droplets dispersing through golden sunlight, each particle glistening and floating in air, mist cloud expanding outward in slow motion, light rays passing through spray creating rainbow micro-prismatic effects, ethereal and dreamlike atmosphere, 0.25x slow motion speed, macro lens perspective, 5 seconds duration
```

---

### 장면 6: 감성 순간 — 눈 감고 호흡 (25~30초)

| 항목 | 내용 |
|------|------|
| 시간 | 00:25 ~ 00:30 |
| 장면 설명 | 여성이 눈을 감고 깊이 들이마시며 미소짓는 클로즈업. 미스트 입자가 아직 공중에 떠 있음. 자연광이 얼굴에 부드럽게 닿음. |
| 카메라 | Close-up portrait, 정적 |
| 분위기 | 이완, 평화, 브랜드 철학 "호흡을 통한 휴식" |
| 자막 | 없음 |
| 오디오 | 피아노 멜로디 피크 |

키프레임 이미지 프롬프트:
```
Cinematic 16:9 close-up portrait of a young Korean woman in her late 20s with natural minimal makeup, eyes gently closed with a serene peaceful smile, face tilted slightly upward as if breathing in deeply, warm golden morning sunlight softly illuminating her left cheek and creating a rim light on her hair, fine mist particles floating in the air around her catching the sunlight, she wears an oversized cream linen shirt, natural skin texture with visible pores preserved no airbrushing, soft cream bokeh background with blurred window, Sony A1 with 85mm f/1.4 GM lens, intimate portrait photography, warm color temperature 3200K, 8K resolution, peaceful meditative expression conveying deep relaxation
```

Kling 3 비디오 프롬프트:
```
Close-up portrait of a young Korean woman with eyes closed and serene smile, she slowly breathes in deeply and her expression softens further into peaceful relaxation, fine mist particles float around her face catching golden sunlight, her hair moves slightly with a gentle breeze from the window, subtle facial muscle movements showing contentment, warm golden lighting on skin, stationary intimate camera, 5 seconds duration
```

---

### 장면 7: 브랜드 철학 자막 (30~35초)

| 항목 | 내용 |
|------|------|
| 시간 | 00:30 ~ 00:35 |
| 장면 설명 | 카메라가 천천히 Pull back하며 전체 공간이 보임. 미니멀 타이포그래피로 브랜드 메시지가 페이드인. |
| 카메라 | Slow pull back / wide |
| 분위기 | 브랜드 메시지 전달 |
| 자막 | "진정한 이완은, 우리 몸 안에서 시작됩니다." (세리프체, 화이트) |
| 오디오 | 음악 유지, 볼륨 살짝 다운 |

키프레임 이미지 프롬프트:
```
Cinematic 16:9 wide shot pulling back to reveal the full luxury minimal apartment scene, the young Korean woman sits peacefully on a cream sofa holding the KINOHI bottle, warm golden morning light fills the entire room through arched windows, dried eucalyptus and small candle on marble side table, white oak flooring, the space is 80 percent negative space emphasizing minimalism, elegant serif typography text overlay in the lower third reading "진정한 이완은, 우리 몸 안에서 시작됩니다." in thin white Garamond font, cinematic letterbox aspect ratio feel, warm color grading, luxury brand commercial final wide establishing shot, 8K resolution
```

Kling 3 비디오 프롬프트:
```
Camera slowly pulls back from close portrait to wide shot revealing full minimal luxury apartment interior, woman sitting peacefully on cream sofa holding KINOHI bottle, golden morning light flooding through arched windows, curtains swaying gently, the entire room bathed in warm light, smooth slow dolly backward movement, serene atmosphere, 5 seconds duration
```

**자막 처리:** 이 장면의 자막은 Weavy에서 직접 렌더링하지 않음. 후반 편집(CapCut/Premiere)에서 세리프체 자막 오버레이 추가.

---

### 장면 8: 제품 라인업 (35~40초)

| 항목 | 내용 |
|------|------|
| 시간 | 00:35 ~ 00:40 |
| 장면 설명 | 대리석 위에 KINOHI 3종(룸스프레이, 바디미스트, 핸드워시)이 나란히 놓여있음. 각 제품 앞에 원료를 상징하는 소품(편백잎, 카다멈, 라임). 카메라가 왼쪽에서 오른쪽으로 슬라이딩. |
| 카메라 | Slow horizontal slide left to right |
| 분위기 | 전체 라인업 소개, 구매 욕구 자극 |
| 자막 | "Woody · Santal · Kinohi" (각 제품 아래, 세리프체) |
| 오디오 | 음악 볼륨 다시 업 |

키프레임 이미지 프롬프트:
```
Cinematic 16:9 wide product lineup shot of three KINOHI minimalist cylindrical bottles arranged in a row on white Carrara marble surface, left bottle labeled Woody with a small cypress sprig in front, center bottle labeled Santal with cardamom pods, right bottle labeled Kinohi with a thin lime slice, warm golden side lighting from the left creating long elegant shadows, each bottle casting a subtle reflection on polished marble, soft cream background with blurred arched window, editorial product photography composition with intentional spacing between bottles, Hasselblad X2D medium format, 80mm f/2.8 lens, warm color temperature 3200K, luxury fragrance brand catalog aesthetic, 8K resolution
```

Kling 3 비디오 프롬프트:
```
Slow horizontal camera slide from left to right across three KINOHI bottles lined up on marble surface, golden side lighting creating shifting highlights on each bottle as camera passes, subtle shadow movements on marble, each bottle has a small botanical prop in front, smooth lateral tracking shot, premium product showcase, 5 seconds duration
```

---

### 장면 9: 엔딩 — 로고 + CTA (40~45초)

| 항목 | 내용 |
|------|------|
| 시간 | 00:40 ~ 00:45 |
| 장면 설명 | 화면이 크림 화이트로 페이드. KINOHI 로고가 중앙에 페이드인. 슬로건 "for all your breaths"가 아래에 표시. 하단에 kinohi.kr URL. |
| 카메라 | 정적 (그래픽 오버레이) |
| 분위기 | 브랜드 각인, 행동 유도 |
| 자막 | "KINOHI" (로고) + "for all your breaths" (슬로건) + "kinohi.kr" (URL) |
| 오디오 | 피아노 마지막 코드 + 페이드아웃 |

키프레임 이미지 프롬프트:
```
Cinematic 16:9 minimalist brand end card on pure cream white background with subtle warm gradient, the KINOHI logo in elegant thin serif typography centered in the upper third of frame, below it the tagline "for all your breaths" in smaller lightweight serif font, at the bottom "kinohi.kr" in minimal sans-serif, the entire composition is extremely minimal with 90 percent negative space, a very subtle golden warm glow emanating from behind the logo text, clean typography-focused luxury brand end screen, no products no images only typography on cream white, 8K resolution
```

Kling 3 비디오 프롬프트:
```
Fade from cream white to reveal KINOHI logo text centered on screen, subtle warm golden glow pulsing gently behind the logo, tagline text fades in below, minimal elegant animation, the golden glow slowly breathes like a gentle inhale and exhale rhythm, pure minimalist brand end card, 5 seconds duration
```

---

## Part 2. Weavy 워크플로우 — 전체 노드 구성도

```
[전체 파이프라인]

① Prompt (제품 정보 입력)
   │
   ▼
② Run Any LLM (9개 장면 프롬프트 자동 생성)
   │
   ▼
③ Text Iterator (9개 프롬프트 순차 처리)
   │
   ├──→ ④ Flux 2 Pro (키프레임 이미지 9장 생성)
   │         │
   │         ▼
   │    ⑤ Kling 3 (각 키프레임 → 5초 비디오 클립 9개 생성)
   │         │
   │         ▼
   │    ⑥ Video Concatenator (9개 클립 → 45초 1개 영상 결합)
   │         │
   │         ▼
   │    ⑦ Merge Audio and Video (BGM + 45초 영상 합성)
   │         │
   │         ▼
   │    ⑧ Topaz Video Upscaler (최종 4K 업스케일)
   │         │
   │         ▼
   │    ⑨ Export (최종 파일 다운로드)
   │
   └──→ [병렬] File (제품 이미지 7장) ──→ ④번 Flux의 Image Reference 포트에 연결
```

---

## Part 3. Weavy 노드별 상세 설정 가이드

### 노드 ① Prompt (제품 정보 입력)

- **추가 방법:** 캔버스 빈 공간 더블클릭 → "Prompt" 검색 → **Prompt** 노드 추가
- **역할:** Run Any LLM에게 전달할 제품 정보 + 영상 기획 지시문
- **입력할 텍스트:**

```
You are a premium brand video ad creative director. Create 9 scene prompts for a 45-second cinematic 16:9 ad video for KINOHI, a Korean premium minimal fragrance brand.

BRAND INFO:
- Brand: KINOHI (키노히)
- Tagline: "for all your breaths"
- Philosophy: "True relaxation begins within our body through breathing"
- Key ingredient: Jeju hinoki cypress natural extract
- Product: Room Spray "Woody" (Top: Thyme / Middle: Cypress / Base: Cedar Wood)
- Design: Ultra minimal cylindrical white bottle, minimal label
- Price: 28,000 KRW (premium positioning between Tamburins and Diptyque)
- Visual tone: Extreme minimalism, white/cream base, serif typography, 80% negative space

VIDEO STRUCTURE (45 seconds = 9 scenes x 5 seconds each):
Scene 1 (0-5s): Opening - Misty Jeju cypress forest at dawn, establishing nature connection
Scene 2 (5-10s): Transition - Forest dissolves into minimal luxury apartment, morning light
Scene 3 (10-15s): Product Hero - KINOHI bottle close-up on marble, arc camera movement
Scene 4 (15-20s): Human Touch - Elegant hand picks up bottle from marble
Scene 5 (20-25s): Key Moment - Mist spray extreme slow-motion macro, golden light
Scene 6 (25-30s): Emotion - Woman eyes closed breathing deeply, serene smile
Scene 7 (30-35s): Brand Message - Pull back wide shot, Korean text overlay space
Scene 8 (35-40s): Product Lineup - Three bottles on marble, horizontal slide
Scene 9 (40-45s): End Card - KINOHI logo fade in on cream white, tagline

FOR EACH SCENE, output TWO prompts:
1. IMAGE PROMPT: For Flux 2 Pro image generation (16:9, cinematic, include camera/lens specs)
2. VIDEO PROMPT: For Kling 3 video generation (camera movement, motion description, 5 seconds)

RULES:
- Every prompt must specify exact camera model and lens (Arri Alexa, Sony A1, Hasselblad, etc.)
- Color temperature must be specified (3200K warm golden)
- Include "8K resolution" in every image prompt
- Product shots must show "KINOHI" label text clearly
- NO stock photo aesthetic. Must look like a real luxury fragrance commercial.
- Scenes 1, 2, 7, 9 should have NO product visible (pure atmosphere/branding)
- Scenes 3, 4, 5, 8 must show the actual KINOHI bottle
- Scene 6 shows the woman, no product in hand
- Output format: Scene X | IMAGE: [prompt] | VIDEO: [prompt]
```

---

### 노드 ② Run Any LLM (프롬프트 자동 생성)

- **추가 방법:** 캔버스 더블클릭 → "Run Any LLM" 검색 → **Run Any LLM** 노드 추가
- **역할:** 입력된 제품 정보를 기반으로 9개 장면의 이미지/비디오 프롬프트를 자동 생성
- **연결:** Prompt 노드의 출력(빨간색) → Run Any LLM의 텍스트 입력(빨간색)에 연결
- **내부 설정:**
  - Model: GPT-4o 또는 Claude 3.5 Sonnet (사용 가능한 모델 중 선택)
  - Temperature: 0.7 (창의성과 일관성 밸런스)

**출력 결과:** 9개 장면 각각에 대한 IMAGE 프롬프트 + VIDEO 프롬프트 텍스트가 생성됨

---

### 노드 ③ Text Iterator (9개 프롬프트 순차 처리)

- **추가 방법:** 캔버스 더블클릭 → "Text Iterator" 검색 → **Text Iterator** 노드 추가
- **역할:** Run Any LLM이 생성한 9개 프롬프트를 하나씩 순서대로 Flux 2 Pro에 전달
- **연결:** Run Any LLM 출력(빨간색) → Text Iterator 입력(빨간색)
- **내부 설정:** Separator를 "Scene"으로 설정하여 장면별로 분리

**대안 방식:** Text Iterator가 복잡하면, 수동으로 Prompt 노드 9개를 각각 만들어 각 장면 프롬프트를 직접 입력하는 방법도 있음. (시간은 더 걸리지만 제어가 정확함)

---

### 노드 ④ Flux 2 Pro (키프레임 이미지 생성)

- **추가 방법:** 캔버스 더블클릭 → "Flux" 검색 → **Flux 2 Pro** 노드 추가
- **역할:** 각 장면의 대표 이미지(키프레임) 1장씩 생성
- **연결:**
  - Text Iterator의 출력(빨간색) → Flux 2 Pro의 프롬프트 입력(빨간색)
  - File 노드(제품 이미지) → Flux 2 Pro의 Image Reference 입력(녹색) [장면 3,4,5,8에만]
- **내부 설정:**
  - Aspect Ratio: 16:9
  - Quality: High
  - Guidance Scale: 7~8 (프롬프트 준수율 높게)
- **출력:** 9장의 키프레임 이미지

---

### 노드 ⑤ Kling 3 (키프레임 → 비디오 클립)

- **추가 방법:** 캔버스 더블클릭 → "Kling" 검색 → **Kling 3** 노드 추가
- **역할:** 각 키프레임 이미지를 5초 비디오 클립으로 변환 (Image-to-Video)
- **연결:**
  - Flux 2 Pro 출력(녹색/이미지) → Kling 3의 이미지 입력(녹색)
  - 별도 Prompt 노드(비디오 프롬프트) → Kling 3의 텍스트 입력(빨간색)
- **내부 설정:**
  - Duration: 5 seconds
  - Resolution: 4K (또는 1080p)
  - FPS: 60 (부드러운 슬로모션 가능)
  - Mode: Image-to-Video (I2V)
- **출력:** 5초 비디오 클립 9개

---

### 노드 ⑥ Video Concatenator (비디오 결합)

- **추가 방법:** 캔버스 더블클릭 → "Video Concatenator" 검색 → **Video Concatenator** 노드 추가
- **역할:** 9개의 5초 클립을 순서대로 이어붙여 45초 1개 영상으로 결합
- **연결:** 9개의 Kling 3 출력(녹색) → Video Concatenator 입력(녹색)에 순서대로 연결
- **내부 설정:**
  - Transition: Cross Dissolve 또는 None (장면 간 전환 방식)
  - Order: 장면 1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 순서

---

### 노드 ⑦ Merge Audio and Video (오디오 합성)

- **추가 방법:** 캔버스 더블클릭 → "Merge Audio" 검색 → **Merge Audio and Video** 노드 추가
- **역할:** 결합된 45초 영상에 BGM을 입힘
- **연결:**
  - Video Concatenator 출력(녹색/비디오) → Merge Audio and Video의 비디오 입력
  - File 노드(BGM 오디오 파일) → Merge Audio and Video의 오디오 입력
- **BGM 준비:** 45초 분량의 미니멀 피아노 앰비언트 트랙 (Artlist, Epidemic Sound 등에서 라이센스 구매 또는 Weavy의 Video to Audio 노드로 자동 생성)

---

### 노드 ⑧ Topaz Video Upscaler (최종 업스케일) [선택사항]

- **추가 방법:** 캔버스 더블클릭 → "Topaz" 검색 → **Topaz Video Upscaler** 노드 추가
- **역할:** 최종 영상을 4K로 업스케일하여 화질 극대화
- **연결:** Merge Audio and Video 출력 → Topaz Video Upscaler 입력

---

### 노드 ⑨ Export (최종 출력)

- **추가 방법:** 캔버스 더블클릭 → "Export" 검색 → **Export** 노드 추가
- **역할:** 최종 완성 영상을 다운로드
- **연결:** 마지막 노드의 출력 → Export 입력

---

## Part 4. LLM 프롬프트 생성 — 상세 설명

### Run Any LLM이란?

Weavy 캔버스 안에서 GPT-4o, Claude 등 LLM을 직접 실행하는 노드. 제품 정보와 영상 기획 지시를 입력하면, LLM이 각 장면에 맞는 이미지/비디오 프롬프트를 자동으로 생성해준다.

### 작동 흐름:

```
[제품 정보 + 영상 기획 지시]
         │
         ▼
   [Run Any LLM]
         │
         ▼
   [9개 장면별 프롬프트 출력]

   Scene 1 | IMAGE: Cinematic 16:9 wide shot of misty... | VIDEO: Slow dolly forward...
   Scene 2 | IMAGE: Cinematic 16:9 wide shot of minimal... | VIDEO: Cross-dissolve...
   Scene 3 | IMAGE: Cinematic 16:9 close-up hero shot... | VIDEO: Slow arc movement...
   ... (9개까지)
```

### 수동 방식 (Text Iterator 없이):

LLM이 생성한 9개 프롬프트를 직접 복사하여:
- Prompt 노드 9개 생성 → 각각에 IMAGE 프롬프트 붙여넣기
- 별도 Prompt 노드 9개 생성 → 각각에 VIDEO 프롬프트 붙여넣기
- 각 IMAGE Prompt → 해당 Flux 2 Pro에 연결
- 각 VIDEO Prompt → 해당 Kling 3에 연결

---

## Part 5. 후반 편집 (Weavy 외부)

Weavy에서 45초 영상이 완성된 후, 다음 항목은 외부 편집 도구(CapCut, Premiere Pro)에서 처리:

| 항목 | 도구 | 내용 |
|------|------|------|
| 자막 오버레이 | CapCut / Premiere | 장면 7의 브랜드 메시지 자막 (세리프체) |
| 로고 삽입 | CapCut / Premiere | 장면 9의 KINOHI 로고 + 슬로건 |
| 컬러 그레이딩 통일 | DaVinci Resolve / Premiere | 9개 클립의 색감 일관성 맞추기 |
| 사운드 디자인 | Premiere / Audacity | 스프레이 ASMR 소리, 앰비언스 추가 |
| 최종 컷 타이밍 조정 | CapCut / Premiere | 장면 전환 타이밍 미세 조정 |
