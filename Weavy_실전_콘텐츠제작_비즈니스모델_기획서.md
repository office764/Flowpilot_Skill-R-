# Weavy.ai 실전 콘텐츠 제작 가이드 + 비즈니스 모델 기획
## FlowPilot | 2026.02.28

---

# PART 1. 광고 브랜드 홍보 숏폼 (16:9) — 실전 제작

---

## 1-1. 완성까지의 전체 프로세스 (5단계)

```
[Phase 1]           [Phase 2]          [Phase 3]          [Phase 4]         [Phase 5]
제품 이미지 준비 → 배경 이미지 생성 → 제품+배경 합성 → 비디오 변환 → 최종 편집/출력
(File Node)       (Flux 2 Pro)      (Composite)       (Kling 3/Veo)     (Export)
```

---

## 1-2. Phase 1 — 제품 이미지 준비

대표님 캔버스에 이미 File 노드로 키노히 제품 이미지 6장이 업로드 되어 있는 상태.

지금 해야 할 것:

**Step 1.** + 버튼 → **"Segment Anything"** 검색 → 추가

**Step 2.** File 노드(제품 이미지) 출력 → Segment Anything 입력에 연결

**Step 3.** Segment Anything 노드를 **Run** → 제품만 깔끔하게 잘려나오는지 확인

이렇게 하면 **투명 배경의 제품 이미지**가 나옵니다. 뚜껑, 레이블, 노즐 전부 원본 그대로 100% 유지.

---

## 1-3. Phase 2 — 배경 이미지 생성 (3가지 시안)

### 시안 A: "대리석 골든아워" (럭셔리 감성)

+ 버튼 → **Prompt** 노드 추가, 내용:
```
Cinematic 16:9 wide shot of an elegant white Carrara marble surface with natural grey
veining, positioned in a luxury minimal interior, warm golden hour sunlight streaming
from the left side creating long dramatic shadows across the marble, an arched floor-to-
ceiling window with sheer cream linen curtains softly billowing in background, a single
dried eucalyptus branch and a small unlit cream pillar candle placed on the far right
third of frame, the center-left area of the marble is completely empty and clear for
product placement, shallow depth of field with the background at f/2.0 bokeh, warm color
temperature 3200K, cream ivory and soft gold color palette, anamorphic lens characteristics
with subtle horizontal flare, Arri Alexa Mini LF cinematic look, luxury fragrance
commercial aesthetic, absolutely NO bottle NO product in the scene
```

+ 버튼 → **Flux 2 Pro** 추가. Prompt 출력 → Flux Prompt 입력 연결.
- Image Size: **Landscape 16 9** 선택

### 시안 B: "편백 숲 새벽안개" (자연/힐링 감성)

+ 버튼 → **Prompt** 노드 추가, 내용:
```
Cinematic 16:9 wide establishing shot of a weathered light oak wood table surface
positioned outdoors in a misty hinoki cypress forest at dawn, soft volumetric fog rolling
between ancient dark green tree trunks in the background, delicate morning dew droplets
scattered across the wood grain catching diffused golden light, a small fresh sprig of
cypress needles and a smooth river stone placed on the far right edge, the center-left
of the table is completely empty and clear for product placement, cool green and warm
cream color palette, atmospheric depth with multiple layers of fog, god rays piercing
through canopy above, Cooke S7 anamorphic lens with oval bokeh, Terrence Malick nature
film aesthetic, 8K resolution, serene and mystical forest bathing atmosphere, absolutely
NO bottle NO product in frame
```

+ 버튼 → **Flux 2 Pro** 추가. Image Size: **Landscape 16 9**

### 시안 C: "미니멀 화이트 스튜디오" (클린 광고 감성)

+ 버튼 → **Prompt** 노드 추가, 내용:
```
Cinematic 16:9 professional studio product photography setup, infinite white seamless
background with subtle warm grey gradient at the bottom edges, a clean circular elevated
white acrylic platform pedestal in the center-left of frame, single dramatic key light
from upper left at 45 degrees creating a crisp defined shadow falling to the lower right,
a subtle secondary fill light from the right at 20 percent intensity, fine atmospheric
haze particles floating in the light beam creating depth, the pedestal surface is
completely empty for product placement, Profoto D2 studio flash lighting quality,
Phase One IQ4 150MP camera sharpness, luxury cosmetic brand commercial aesthetic,
clean minimal Aesop Byredo advertising style, 8K resolution, absolutely NO bottle NO
product in frame
```

+ 버튼 → **Flux 2 Pro** 추가. Image Size: **Landscape 16 9**

---

## 1-4. Phase 3 — 제품 + 배경 합성

각 Flux 배경 결과물에 원본 제품을 합성합니다.

**Step 1.** + 버튼 → **"Composite"** 검색 → **Composite (Simple Blend)** 추가

**Step 2.** 연결:
- Flux 2 Pro 출력 (배경 이미지) → Composite **Background** 입력
- Segment Anything 출력 (잘린 제품) → Composite **Foreground** 입력

**Step 3.** Run → 합성 결과 확인

시안 3개이므로 Composite 노드 3개 필요 (각 배경마다 1개).

---

## 1-5. Phase 4 — 비디오 변환 (16:9 숏폼 광고)

합성된 이미지를 비디오로 변환합니다.

### 옵션 A: Kling 3 (가성비, 빠른 생산)

+ 버튼 → **Kling 3** 추가

**연결 3개 필요:**

1. Composite 출력 (합성 이미지) → Kling 3 **Image** 입력
2. 새 Prompt 노드 → Kling 3 **Prompt** 입력
3. 새 Prompt 노드 → Kling 3 **Negative Prompt** 입력

**Kling 3 Prompt (시안 A: 대리석):**
```
Slow elegant camera push-in towards the product bottle on marble surface, golden sunlight
gradually intensifies creating moving light patterns on the marble, sheer curtains billow
gently in soft breeze, fine dust particles drift through the warm light beam, the shadow
of the bottle slowly elongates as light shifts, camera movement: smooth dolly forward at
0.2x speed with subtle 3-degree downward tilt, shallow depth of field rack focus from
background curtains to bottle, warm cinematic color grading, luxury perfume commercial
pacing, serene and sophisticated atmosphere
```

**Kling 3 Prompt (시안 B: 편백숲):**
```
Atmospheric slow camera drift forward towards the product bottle on wood table in misty
forest, fog rolls gently between cypress trees in background creating depth movement,
morning dew droplets on wood surface catch shifting light as sun rises, a single leaf
slowly falls from above passing through frame, subtle breeze moves the cypress sprig,
camera movement: slow creeping dolly at 0.15x speed, volumetric god rays shift angle
slightly, cool to warm color temperature transition, nature documentary meets luxury
commercial aesthetic, ASMR peaceful atmosphere
```

**Kling 3 Prompt (시안 C: 화이트 스튜디오):**
```
Clean product reveal with slow 15-degree arc camera movement around the bottle on white
pedestal, studio key light creates crisp moving shadow as camera orbits, atmospheric haze
particles drift slowly through the light beam, subtle light flicker adds cinematic life,
camera movement: smooth orbital arc from front-left to direct-front at 0.3x speed, the
shadow rotates gracefully on the white surface, ultra clean minimal aesthetic, luxury
cosmetic brand TV commercial style, Aesop Byredo advertising quality
```

**Negative Prompt (3개 시안 공통):**
```
fast motion, shaky camera, blurry, distorted bottle shape, melting product, morphing
bottle design, cap changing shape, nozzle deforming, label text distortion, extra objects
appearing, text overlay, watermark, low quality, oversaturated colors, cartoon style,
anime, illustration, painting style
```

### 옵션 B: Veo 3.1 (프리미엄 4K 시네마틱)

고가 고객용 프리미엄 결과물이 필요할 때. Kling 3 대신 **Veo 3.1 Image to Video** 추가. 동일하게 Composite 출력 → Veo Image 입력, Prompt 노드 → Veo Prompt 입력.

### 옵션 C: Runway Gen-4.5 (최고 퀄리티)

ELO 1위 모델. 가장 높은 비주얼 퀄리티. **Runway Gen-4.5** 추가.

---

## 1-6. Phase 5 — 최종 출력

+ 버튼 → **Export** 노드 추가 → 비디오 모델 출력 연결
- Format: MP4
- Resolution: 1920x1080 (16:9)

---

## 1-7. 최종 캔버스 전체 구성도 (한눈에)

```
                                    ┌→ Prompt A (대리석) → Flux A ──→ Composite A ──→ Kling 3A → Export A
                                    │                                    ↑
① File (원본) → ② Segment Anything ─├→ Prompt B (편백숲) → Flux B ──→ Composite B ──→ Kling 3B → Export B
                    (제품 추출)      │                                    ↑
                    ────────────────├→ Prompt C (스튜디오) → Flux C → Composite C ──→ Kling 3C → Export C
                    │               │
                    │               └→ (각 Composite의 Foreground에 ② 출력 공통 연결)
                    │
                    └→ (Segment Anything 출력을 3개 Composite Foreground에 모두 연결)

                    각 Kling 3에는 별도 Prompt + Negative Prompt 노드 연결 (총 6개 추가 Prompt 노드)
```

---

# PART 2. AI UGC 콘텐츠 제작 — 실전 워크플로우

---

## 2-1. AI UGC 제작 흐름

```
[Phase 1]           [Phase 2]          [Phase 3]         [Phase 4]        [Phase 5]
AI 인물+제품       비디오 변환       립싱크 추가       음성/BGM 합성    최종 출력
이미지 생성        (Kling 3)         (Omnihuman)       (Merge Audio)    (Export)
(Flux 2 Pro)
```

## 2-2. Phase 1 — AI 인물 + 제품 합성 이미지

+ 버튼 → **Prompt** 노드 추가:

**UGC 시나리오 1: "아침 루틴 리뷰어"**
```
A natural-looking Korean woman in her late 20s with minimal no-makeup makeup look, shoulder
length slightly wavy dark brown hair, wearing an oversized cream knit cardigan over a white
cotton t-shirt, sitting cross-legged on a bed with white linen sheets, holding a KINOHI
santal body mist bottle in her right hand at chest height, looking directly at camera with
a warm authentic smile showing slight teeth, natural morning window light from the left
creating soft shadows on her face, clean minimal bedroom background with white walls and a
small potted plant on nightstand, subtle eye-level camera angle as if filmed on iPhone
propped on stack of books, authentic Korean beauty vlogger YouTube review setup, Canon EOS
R5 with 35mm f/1.4 lens perspective, visible natural skin texture including subtle pores
and tiny moles for authenticity, 16:9 landscape composition, warm and relatable atmosphere,
NOT perfect NOT airbrushed NOT glamorous, real person aesthetic
```

프롬프트 핵심 설명: "NOT perfect NOT airbrushed NOT glamorous, real person aesthetic"가 UGC의 생명이다. AI가 만드는 완벽한 모델 사진은 UGC가 아니다. 의도적으로 "불완전한 자연스러움"을 주문해야 진짜 리뷰어처럼 보인다.

**UGC 시나리오 2: "언박싱 탑뷰"**
```
Top-down birds eye view of a pair of feminine hands with short natural nails unwrapping a
white KINOHI gift box on a clean white desk surface, the box lid is half removed revealing
three minimalist cylindrical bottles nestled in white tissue paper inside, a cup of warm
latte with latte art sits in the upper right corner, an iPhone with cracked screen
protector in upper left corner, warm overhead ring light illumination, authentic unboxing
moment captured mid-action, slight motion blur on the moving hand for realism, 16:9
landscape composition, casual lifestyle flat lay aesthetic, Instagram story screenshot
quality, NOT staged NOT perfect, genuine excitement moment
```

**UGC 시나리오 3: "사용 후기 토킹헤드"**
```
Medium close-up of a Korean woman in her early 30s with glasses and hair in a messy low
bun, wearing a grey crewneck sweatshirt, facing camera directly in a casual home office
setting, bookshelves slightly out of focus behind her, a KINOHI woody room spray bottle
visible on her desk to the right, she is mid-sentence with mouth slightly open and eyebrows
raised expressively as if explaining something enthusiastically, laptop webcam angle
slightly looking up at her face, warm desk lamp lighting mixed with cool monitor glow on
her face creating mixed color temperature, 16:9 landscape, authentic Zoom call or YouTube
review talking head setup, casual Monday morning energy, relatable and trustworthy
```

+ 버튼 → **Flux 2 Pro** 추가. Prompt → Flux Prompt 입력 연결.
- Image Size: **Landscape 16 9**
- 원본 제품 이미지가 필요하면 File 노드도 Flux Image 입력에 연결 (참조용)

## 2-3. Phase 2 — AI 인물 이미지를 비디오로 변환

+ 버튼 → **Kling 3** 추가

Flux 출력 (AI 인물 이미지) → Kling 3 Image 입력

**Kling 3 Prompt (시나리오 1):**
```
The woman gently lifts the KINOHI body mist bottle, sprays once on her left wrist with a
natural flick motion, brings her wrist to her nose and inhales with eyes closing softly,
a subtle satisfied smile spreads across her face, she opens her eyes and looks at camera
nodding slightly as if to say this is good, natural head micro-movements throughout,
camera stays static on tripod, warm morning light, authentic vlogger review moment,
5 second duration
```

**Kling 3 Prompt (시나리오 3):**
```
The woman speaks animatedly to camera, gesturing with her right hand while her left hand
occasionally touches the KINOHI room spray bottle on the desk, natural head tilts and
eyebrow raises during speech, she picks up the bottle briefly to show it to camera then
sets it back down, subtle body sway and natural breathing movement, webcam static angle,
mixed warm and cool lighting, authentic YouTube review talking head, 5 second duration
```

## 2-4. Phase 3 — 립싱크 추가

+ 버튼 → **Omnihuman V1.5** 추가 (Lip Sync 카테고리)

Kling 3 비디오 출력 → Omnihuman Image/Video 입력

오디오 파일(한국어 리뷰 음성)을 별도로 준비하여 Omnihuman Audio 입력에 연결. 음성은 ElevenLabs나 외부 TTS로 미리 생성.

리뷰 스크립트 예시:
"요즘 제가 매일 쓰는 바디미스트인데요, 키노히 산탈이에요. 향이 진짜 은은하게 지속되면서 편백 베이스라서 엄청 편안해요. 출근 전에 한 번 뿌리면 점심때까지 은은하게 남아있거든요. 강추합니다."

## 2-5. Phase 4 — 음악/BGM 합성

+ 버튼 → **Merge Audio and Video** 추가 (Community Models)

Omnihuman 비디오 출력 + BGM 오디오 파일 → Merge Audio and Video

## 2-6. Phase 5 — Export

+ 버튼 → **Export** → MP4, 1920x1080

---

# PART 3. 비즈니스 수익구조 + 사업 브레인스토밍

---

## 3-1. 수익 구조 모델 — 4층 피라미드

```
                    ┌─────────────────────┐
                    │   4층: AI 콘텐츠    │  ← 월 300~500만원 MRR
                    │   구독 SaaS 플랫폼  │     (자체 플랫폼 수익)
                    ├─────────────────────┤
                    │   3층: AI 콘텐츠    │  ← 건당 500~2,000만원
                    │   에이전시 (외주)    │     (고단가 프로젝트)
                    ├─────────────────────┤
                    │   2층: 교육/컨설팅   │  ← 월 200~500만원
                    │   (강의, 코칭)       │     (지식 기반 수익)
                    ├─────────────────────┤
                    │   1층: 포트폴리오    │  ← 리드 생성 엔진
                    │   콘텐츠 마케팅      │     (무료, 유입 목적)
                    └─────────────────────┘
```

---

## 3-2. 1층: 포트폴리오 콘텐츠 마케팅 (리드 엔진)

**무엇을 하나:** 키노히 같은 실제 브랜드(또는 가상 브랜드)로 AI 콘텐츠를 만들어 SNS에 공개. "이 영상 AI로 만들었습니다" 라는 후킹으로 바이럴.

**구체적 액션:**
- 인스타그램/틱톡에 주 3회 AI 제품 광고 영상 포스팅
- "Before(원본 제품사진) → After(AI 광고)" 비교 릴스
- 유튜브에 "Weavy로 5분 만에 브랜드 광고 만드는 법" 튜토리얼
- 키노히 포트폴리오를 FlowPilot 홈페이지 케이스스터디로 게시

**KPI:** 월 50건 이상 인바운드 문의 유입

---

## 3-3. 2층: 교육/컨설팅 사업

### 사업 A: "AI 콘텐츠 마스터클래스" 온라인 강의

| 항목 | 내용 |
|------|------|
| 플랫폼 | 클래스101, 탈잉, 자체 Teachable |
| 가격 | 39만원 (기본) / 89만원 (1:1 코칭 포함) |
| 내용 | Weavy 기초 → 제품 이미지 → 비디오 → UGC → 배포 자동화 |
| 타겟 | 1인 이커머스 사업자, 마케터, 프리랜서 디자이너 |
| 예상 매출 | 월 20명 x 39만원 = 월 780만원 |

### 사업 B: "브랜드 AI 전환 컨설팅"

| 항목 | 내용 |
|------|------|
| 대상 | 중소 뷰티/패션/F&B 브랜드 |
| 서비스 | AI 콘텐츠 전략 수립 + Weavy 워크플로우 세팅 + 팀 교육 |
| 가격 | 300~500만원 (3개월 컨설팅) |
| 업셀 | 컨설팅 후 → 3층 에이전시 서비스로 전환 |

---

## 3-4. 3층: AI 콘텐츠 에이전시 (핵심 매출)

### 서비스 패키지

| 패키지 | 내용 | 가격 | MRR |
|--------|------|------|-----|
| **Starter** | 제품 이미지 20장 + 숏폼 4편/월 | 초기 200만원 + 월 80만원 | 80만원 |
| **Growth** | 이미지 40장 + 숏폼 8편 + UGC 4편/월 | 초기 400만원 + 월 150만원 | 150만원 |
| **Premium** | 이미지 무제한 + 숏폼 16편 + UGC 8편 + 브랜드 필름 2편/월 | 초기 800만원 + 월 300만원 | 300만원 |
| **Enterprise** | 풀스택 (n8n 자동화 + 자동 배포 + 성과 분석) | 초기 2,000만원+ + 월 500만원 | 500만원 |

### 타겟 산업

| 산업 | Pain Point | FlowPilot 솔루션 | 객단가 |
|------|------------|-----------------|--------|
| **뷰티/화장품** | 신제품마다 촬영 비용 500만원+ | AI 이미지+영상 무한 생성 | 높음 |
| **패션/의류** | 시즌마다 모델+스튜디오 촬영 | AI 가상 모델 + 배경 교체 | 높음 |
| **F&B/식음료** | 메뉴 사진 업데이트 비용 | AI 메뉴 비주얼 자동 생성 | 중간 |
| **부동산** | 가상 스테이징 비용 | AI 인테리어 자동 생성 | 중간 |
| **이커머스 셀러** | 상세페이지 이미지 대량 필요 | AI 제품 컷 대량 생산 | 낮음 (볼륨) |
| **스타트업** | 투자 IR 덱 비주얼 | AI 프로덕트 비주얼라이제이션 | 중간 |

### 월 매출 시뮬레이션

| 고객 수 | 패키지 | 월 MRR |
|---------|--------|--------|
| Starter x 5 | 80만원 x 5 | 400만원 |
| Growth x 3 | 150만원 x 3 | 450만원 |
| Premium x 2 | 300만원 x 2 | 600만원 |
| Enterprise x 1 | 500만원 x 1 | 500만원 |
| **합계 11개 고객** | | **월 1,950만원 MRR** |

---

## 3-5. 4층: AI 콘텐츠 SaaS 플랫폼 (Moonshot)

### 사업 C: "BrandForge" — AI 브랜드 콘텐츠 자동 생성 플랫폼

| 항목 | 내용 |
|------|------|
| 컨셉 | 비기술 사업자가 제품 사진만 업로드하면, AI가 자동으로 광고 이미지 10장 + 숏폼 3편 + UGC 2편 생성 |
| 기술 스택 | Antigravity (프론트엔드) + n8n (백엔드 오케스트레이션) + Weavy API (AI 생성 엔진) |
| 가격 | Free (월 5콘텐츠) / Pro 월 9.9만원 (월 50콘텐츠) / Business 월 29.9만원 (무제한) |
| 타겟 | 1인 셀러, 소상공인, 스타트업 마케터 |
| 차별점 | 에이전시 대비 90% 저렴, 즉시 생성, 셀프서비스 |
| 기술 장벽 | Weavy API + n8n 자동화 + Antigravity 대시보드 = 경쟁사 진입 장벽 |

수익 시뮬레이션:
- Pro 유저 200명 x 9.9만원 = 월 1,980만원
- Business 유저 50명 x 29.9만원 = 월 1,495만원
- 합계: 월 3,475만원 (마진율 80%+, AI API 비용만 원가)

### 사업 D: "UGC Factory" — AI UGC 전문 플랫폼

| 항목 | 내용 |
|------|------|
| 컨셉 | 브랜드가 제품 정보만 입력하면, AI 아바타가 리뷰/언박싱/사용 영상을 자동 생성 |
| 차별점 | MakeUGC, Arcads 같은 해외 플랫폼의 한국 로컬라이즈 버전. 한국인 AI 아바타, 한국어 자연스러운 립싱크 |
| 기술 스택 | Flux (인물 생성) + Kling (비디오) + Omnihuman (립싱크) + ElevenLabs (한국어 TTS) + n8n (파이프라인) |
| 가격 | 영상 1편당 5만원 또는 월 구독 39.9만원 (20편/월) |
| 시장 | 한국 DTC 브랜드, 이커머스 셀러, 마케팅 에이전시 |

### 사업 E: "AI 촬영 스튜디오" — 오프라인 + AI 하이브리드

| 항목 | 내용 |
|------|------|
| 컨셉 | 오프라인 스튜디오에서 제품 실물 촬영 (기본 컷 3~5장) → AI로 100장+ 변형 생성 |
| 차별점 | 실제 촬영의 정확성 + AI의 무한 확장. "AI만 쓰면 디테일이 안 나온다"는 고객 불안 해소 |
| 가격 | 촬영 패키지 50만원 (실촬영 5컷 + AI 변형 50컷 + 숏폼 5편) |
| 시장 | 뷰티, 패션, 식품 브랜드 |
| 투자 | 소형 스튜디오 월세 100~200만원 + 촬영 장비 500만원 |

---

## 3-6. 사업 로드맵 타임라인

```
[Month 1-2]                    [Month 3-4]                [Month 5-6]
포트폴리오 구축               에이전시 런칭               스케일업
────────────────────         ──────────────────         ──────────────────
- 키노히 케이스 완성           - 첫 유료 고객 3개         - 고객 10개+ 확보
- SNS 포트폴리오 30개+         - Starter/Growth 중심      - Premium 고객 유치
- 클래스101 강의 촬영           - 강의 런칭                - 월 MRR 1,000만원+
- FlowPilot 홈페이지 리뉴얼     - 월 MRR 300만원 돌파      - 팀원 1명 채용 (에디터)

[Month 7-9]                    [Month 10-12]
SaaS MVP                      풀스택 런칭
──────────────────            ──────────────────
- BrandForge MVP 개발           - BrandForge 정식 런칭
  (Antigravity + n8n)           - 유료 유저 100명+
- 클로즈드 베타 테스트           - 에이전시 MRR 2,000만원+
- 월 MRR 1,500만원+             - SaaS MRR 1,000만원+
                                - 총 월 매출 3,000만원+
```

---

## 3-7. 핵심 인사이트

이 모든 사업의 공통 엔진은 **"Weavy + n8n + Antigravity"** 하이브리드 스택이다.

- Weavy = AI 콘텐츠 생성 엔진 (눈에 보이는 가치)
- n8n = 자동화 오케스트레이션 (보이지 않는 기술 장벽)
- Antigravity = 프론트엔드 대시보드 (고객 접점, SaaS화)

경쟁사가 "Weavy로 이미지 만들어드립니다"만 하면, FlowPilot은 "제품 등록하면 이미지+영상+UGC+배포까지 전부 자동"으로 이긴다. 이 3단 스택이 경쟁사가 6개월 안에 따라올 수 없는 해자(moat)다.

가장 빠르게 현금을 만드는 길: 키노히 포트폴리오 완성 → SNS 바이럴 → 인바운드 문의 → Starter 패키지 클로징. 이걸 Month 1에 끝내야 한다.

---

## Sources

- [Weavy 공식](https://www.weavy.ai/)
- [AI UGC 플랫폼 비교 2026](https://videoai.me/blog/best-ai-ugc-generators-2026)
- [Runway Gen-4.5 ELO 1위](https://www.pixazo.ai/blog/ai-video-generation-models-comparison)
- [AI 비디오 모델 비교](https://www.aifreeapi.com/en/posts/seedance-2-vs-kling-3-vs-sora-2-vs-veo-3)
- [AI 이미지 모델 비교](https://www.teamday.ai/blog/best-ai-image-models-2026)
- [MakeUGC](https://www.makeugc.ai/)
- [Arcads](https://www.arcads.ai/)
