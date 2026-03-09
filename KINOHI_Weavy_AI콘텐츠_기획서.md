# KINOHI x Weavy.ai AI 콘텐츠 제작 기획서
## FlowPilot | 2026.02.28

---

## Part 0. 브랜드 분석

### 브랜드 핵심 정보

| 항목 | 내용 |
|------|------|
| 브랜드명 | KINOHI (키노히) |
| 슬로건 | "for all your breaths" |
| 브랜드 철학 | "호흡은 삶과 직접 연결된다. 키노히는 호흡을 통해 휴식을 제공한다. 진정한 이완은 우리 몸 안에서 시작된다." |
| 핵심 원료 | 제주 편백나무 추출 천연 성분 |
| 포지셔닝 | 프리미엄 미니멀 향 브랜드 (탬버린즈, 딥티크 사이 가격대) |
| 디자인 톤 | 극도의 미니멀리즘, 화이트/크림 베이스, 세리프 타이포, 여백 강조 |
| 가격대 | 28,000~58,000원 |
| 판매 채널 | 자사몰(kinohi.kr), 무신사, 아난티 이터널저니 |
| 타겟 | 20~30대 여성, 라이프스타일/감성 소비, 인테리어에 관심 |

### 제품 라인업

| 제품 | 향 노트 | 가격 | 용량 |
|------|---------|------|------|
| **룸스프레이 (Woody)** | Top: Thyme / Middle: Cypress / Base: Cedar Wood | 28,000원 | 200ml |
| **바디미스트 (Santal)** | Top: Cardamom / Middle: Violet / Base: Sandal Wood | 30,000원 | 200ml |
| **핸드워시 (Kinohi)** | Top: Lime / Middle: Neroli / Base: Sandal Wood | 30,000원 | 200ml |
| **기프트세트** | 룸스프레이 + 바디미스트 세트 | 58,000원 | - |

### 비주얼 분석 (제공된 이미지 기반)

스크린샷에서 확인한 브랜드 비주얼 특징:
- 컬러 팔레트: #FFFFFF(화이트), #F5F0EB(크림), #2C2C2C(다크 그레이), #8B9D77(세이지 그린 — Woody), #D4C5B0(샌들우드 베이지 — Santal)
- 패키징: 원통형 미니멀 보틀, 레이블 최소화, 제품명만 간결하게 표기
- 공간 연출: 자연광, 린넨/대리석/화이트 오크 소재, 식물 소품, 아치형 창문
- 기프트세트: 화이트 박스 + 3종 병렬 배치, "KINOHI" 각인
- 상세페이지: 세리프 타이포(Garamond 계열), 풀스크린 라이프스타일 이미지, 여백 80% 이상

---

## Part 1. AI UGC 콘텐츠 제작 기획 (Weavy.ai)

### 1-1. AI UGC 콘텐츠란? (KINOHI 맥락)

AI 아바타가 실제 소비자처럼 키노히 제품을 **언박싱, 사용, 리뷰**하는 숏폼 영상. 인스타그램 릴스, 틱톡, 유튜브 쇼츠에 배포. 실제 인플루언서 비용(건당 50~200만원) 없이 무한 변형 가능.

### 1-2. AI UGC 콘텐츠 시나리오 5종

**시나리오 1: "출근 전 모닝 루틴"**
- 장면: 아침 햇살이 들어오는 침실 → AI 아바타(20대 여성)가 일어나 바디미스트(Santal)를 뿌림 → 카메라가 미스트 입자 클로즈업 → "하루를 시작하는 향" 자막
- 톤: 따뜻한 골든아워, 린넨 침구, 미니멀 인테리어
- 길이: 15초 (릴스/숏츠)

**시나리오 2: "집에 오면 제일 먼저"**
- 장면: 현관문 열림 → 룸스프레이(Woody) 2번 스프레이 → 편백 숲 오버레이 전환 → 소파에 앉아 눈 감음 → "돌아온 순간, 숲이 됩니다" 자막
- 톤: 쿨톤 자연광, 우디 그린 컬러 그레이딩
- 길이: 15초

**시나리오 3: "기프트 언박싱"**
- 장면: 화이트 기프트박스 탑뷰 → 리본 풀기 → 3종 세트 공개 → 하나씩 집어 향 맡기 → "모든 호흡을 위한 선물" 자막
- 톤: ASMR 감성, 따뜻한 간접조명
- 길이: 20초

**시나리오 4: "핸드워시 ASMR"**
- 장면: 대리석 세면대 → 핸드워시 펌핑 → 거품 클로즈업 → 물 흐르는 장면 → 손 위 물방울 매크로 → "손끝에서 시작되는 이완"
- 톤: 차가운 화이트+물소리 ASMR
- 길이: 15초

**시나리오 5: "vs 비교 리뷰"**
- 장면: AI 아바타가 카메라 앞에서 키노히 3종을 하나씩 소개 → 각 향 노트 설명 → "출근 전엔 Santal, 집에선 Woody, 손은 항상 Kinohi" → 추천 마무리
- 톤: 유튜버 스타일, 자연스러운 말투
- 길이: 30초

### 1-3. Weavy 워크플로우: AI UGC 제작

#### 워크플로우 A: "숏폼 UGC 영상 자동 생성"

```
[노드 구성도]

① Import Node ──→ ② Prompt Enhancer ──→ ③ Router Node
   (제품 이미지)      (프롬프트 최적화)         │
                                              ├──→ ④-A Flux 2 Pro (배경 합성 이미지)
                                              ├──→ ④-B Flux 2 Pro (라이프스타일 이미지)
                                              └──→ ④-C Flux 2 Pro (클로즈업 이미지)
                                                        │
                                              ⑤ Router Node (비디오 분기)
                                                        │
                                              ├──→ ⑥-A Kling 3 (숏폼 15초)
                                              ├──→ ⑥-B Seedance (음악 싱크)
                                              └──→ ⑥-C Runway Gen-4 (시네마틱)
                                                        │
                                              ⑦ Export Node (최종 출력)
```

#### 노드별 상세 설정:

**노드 ① Import Node**
- 추가 방법: 캔버스에서 + 버튼 → "Import" 검색 → Import Node 추가
- 설정: 키노히 제품 이미지(제공받은 6장)를 업로드
- 지원 포맷: PNG, JPG

**노드 ② Prompt Enhancer**
- 추가 방법: + 버튼 → "Prompt Enhancer" 검색 → 추가
- 입력: 기본 프롬프트를 넣으면 AI가 더 디테일한 프롬프트로 확장
- 연결: Import Node의 출력 → Prompt Enhancer 입력에 연결

**노드 ③ Router Node**
- 추가 방법: + 버튼 → "Router" 검색 → 추가
- 역할: Prompt Enhancer 출력을 3개 이미지 생성 노드로 분기
- 연결: Prompt Enhancer 출력 → Router 입력

**노드 ④-A Flux 2 Pro (배경 합성)**
- 추가 방법: + 버튼 → "Flux" 검색 → **Flux 2 Pro** 선택
- 모델 선택 이유: 포토리얼리즘 최강. 제품+배경 합성 시 자연스러운 조명/질감 표현

프롬프트 (영문 필수):
```
A premium minimalist room spray bottle with "KINOHI" label, placed on a white marble
countertop beside a small potted cypress plant, soft morning golden hour light streaming
through an arched window with sheer linen curtains, shallow depth of field focusing on the
bottle, cream and sage green color palette, editorial product photography style,
shot on Hasselblad medium format, 8K resolution, natural ambient lighting
```

프롬프트 작동 원리 (한국어 설명):
이 프롬프트는 키노히의 미니멀 브랜드 톤을 정확히 재현한다. "white marble countertop"은 키노히 상세페이지의 대리석 소재를, "arched window with sheer linen curtains"는 브랜드 이미지에서 보이는 아치형 창문을, "cream and sage green"은 Woody 라인의 컬러 팔레트를, "Hasselblad medium format"은 고급 에디토리얼 촬영감을 재현한다. Flux 2 Pro는 이런 포토리얼 질감을 가장 잘 구현하는 모델이다.

**노드 ④-B Flux 2 Pro (라이프스타일)**

프롬프트:
```
A young Korean woman in her late 20s wearing an oversized cream linen shirt, eyes closed
with a gentle smile, holding a minimalist cylindrical body mist bottle labeled "KINOHI"
near her neck, morning sunlight casting soft shadows on her face, bedroom interior with
white oak furniture and dried eucalyptus arrangement, warm golden color grading, intimate
lifestyle photography, shot on Sony A7R V with 85mm f/1.4 lens, bokeh background,
natural skin texture preserved
```

프롬프트 작동 원리:
"oversized cream linen shirt"는 키노히의 내추럴 감성 타겟층 비주얼을 정확히 구현한다. "eyes closed with gentle smile"은 "호흡을 통한 이완"이라는 브랜드 철학을 시각화한다. "85mm f/1.4"는 인물 포트레이트의 황금 렌즈 세팅으로 보케 배경을 만든다. "natural skin texture preserved"는 AI 이미지의 플라스틱 피부 문제를 사전 방지하는 핵심 키워드다.

**노드 ④-C Flux 2 Pro (클로즈업)**

프롬프트:
```
Extreme macro close-up of a fine mist spray dispersing from a premium glass bottle nozzle,
individual water droplets suspended in warm golden sunlight, shallow depth of field with
creamy bokeh, the spray creates a luminous halo effect against a soft cream background,
product photography, Zeiss Otus 100mm macro lens, 8K detail, every droplet visible,
ethereal and calming mood
```

프롬프트 작동 원리:
미스트 입자의 매크로 샷은 UGC 영상에서 "제품 사용 순간"의 시각적 임팩트를 극대화한다. "individual water droplets suspended in sunlight"이 ASMR 감성의 핵심 비주얼이다. "Zeiss Otus 100mm macro"는 실제 매크로 촬영의 최고급 렌즈로, AI에게 정확한 광학적 특성을 지시한다.

**노드 ⑤ Router Node (비디오 분기)**
- 추가 방법: + 버튼 → "Router" 검색 → 추가
- 역할: 완성된 이미지를 3개 비디오 모델로 분기 (A/B 비교)

**노드 ⑥-A Kling 3 (숏폼 15초)**
- 추가 방법: + 버튼 → "Kling" 검색 → **Kling 3** 선택
- 모델 선택 이유: 가성비 최강 + 4K/60fps. 대량 숏폼 생산에 최적
- Motion 설정: Camera → Slow Push In (제품으로 천천히 접근)

비디오 프롬프트:
```
Slow cinematic push-in towards the KINOHI room spray bottle, gentle particles of mist
floating in golden sunlight, curtains swaying softly in background, ambient dust particles
catching light, camera movement: smooth dolly forward at 0.5x speed, shallow depth of
field transition, warm color temperature 3200K, peaceful and meditative atmosphere
```

**노드 ⑥-B Seedance V1.5 Pro (음악 싱크)**
- 추가 방법: + 버튼 → "Seedance" 검색 → **Seedance V1.5 Pro** 선택
- 모델 선택 이유: 오디오+비디오 동시 생성. 음악과 싱크된 무드 영상 자동 생성

비디오 프롬프트:
```
A serene lifestyle scene: a woman's hand gracefully picks up a minimalist white bottle from
a marble surface, brings it to her wrist and sprays once, mist particles catch sunlight,
she closes her eyes and breathes deeply, subtle smile appears, camera slowly pulls back to
reveal a sunlit minimal bedroom, ambient piano music rhythm, calming ASMR-style audio,
warm golden hour lighting throughout
```

**노드 ⑥-C Runway Gen-4.5 (시네마틱)**
- 추가 방법: + 버튼 → "Runway" 검색 → **Runway Gen-4** 선택
- 모델 선택 이유: ELO 1위. 가장 높은 비주얼 퀄리티. 프리미엄 광고 소재용
- Motion Brush 활용: 미스트 입자에만 모션 적용 → 배경 정적 유지

비디오 프롬프트:
```
Ultra cinematic product reveal: KINOHI bottle centered on white marble, camera executes
smooth arc movement from 45-degree angle to eye level, volumetric golden light beams
streaming through window illuminate fine mist particles, depth of field shifts from
bottle to background cypress plant, film grain texture, anamorphic lens flare,
Arri Alexa Mini LF look, 24fps cinematic cadence, luxury brand commercial aesthetic
```

**노드 ⑦ Export Node**
- 추가 방법: + 버튼 → "Export" 검색 → 추가
- 설정: Format → MP4, Resolution → 1080x1920 (세로 숏폼) 또는 1920x1080 (가로 광고)
- 3개 비디오 모델 결과를 각각 Export

---

## Part 2. AI 브랜드 광고 콘텐츠 제작 기획 (Weavy.ai)

### 2-1. 광고 콘텐츠 시나리오 5종

**시나리오 A: "숲에서 온 편지" (브랜드 필름 30초)**
- 컨셉: 편백 숲 → 이슬 → 추출 → 보틀에 담기는 여정
- 장면 구성: 안개 낀 편백 숲 / 이슬 방울 매크로 / 물 흐르는 장면 / 보틀 클로즈업 / 브랜드 로고
- 배포: 인스타 릴스 광고, 유튜브 프리롤

**시나리오 B: "All Your Breaths" (감성 슬로모션 15초)**
- 컨셉: 하루의 호흡 순간들을 슬로모션으로
- 장면: 아침 기지개 → 샤워 후 미스트 → 업무 중 룸스프레이 → 밤 취침 전 → 로고
- 배포: 메타 광고, 인스타 스토리

**시나리오 C: "3 Breaths, 3 Moments" (제품 3종 소개 20초)**
- 컨셉: 각 제품 = 하루의 한 순간
- 장면: Santal(아침) / Kinohi Hand(낮) / Woody(저녁) 시간대별 매칭
- 배포: 틱톡 광고, 카카오 비즈보드

**시나리오 D: "기프트 시즌" (시즈널 광고 15초)**
- 컨셉: 소중한 사람에게 보내는 향
- 장면: 기프트박스 포장 → 리본 묶기 → 전달 → 상대방 미소 → 로고
- 배포: 메타 광고 (발렌타인/크리스마스/화이트데이 시즌)

**시나리오 E: "Product Flat Lay" (정물 광고 이미지 시리즈)**
- 컨셉: 계절별 배경 변형 → 같은 제품, 다른 무드
- Spring: 벚꽃잎 + 린넨 / Summer: 얼음+레몬 / Autumn: 마른잎+우드 / Winter: 눈+캔들
- 배포: 인스타 피드, 네이버 쇼핑 썸네일

### 2-2. Weavy 워크플로우: AI 브랜드 광고 제작

#### 워크플로우 B: "브랜드 필름 자동 생성"

```
[노드 구성도]

① Prompt Node ──→ ② Prompt Enhancer ──→ ③ Flux 2 Pro Ultra
   (씬 설명)          (프롬프트 확장)         (키프레임 이미지 5장)
                                                    │
                                           ④ Relighting Node
                                              (조명 통일)
                                                    │
                                           ⑤ Color Grading Node
                                              (키노히 브랜드 톤)
                                                    │
                                           ⑥ Veo 3.1 Image-to-Video
                                              (시네마틱 4K 비디오)
                                                    │
                                           ⑦ Export Node
                                              (최종 MP4 출력)
```

#### 노드별 상세 설정:

**노드 ① Prompt Node**
- 추가 방법: + 버튼 → "Prompt" 검색 → Prompt Node 추가
- 역할: 각 씬의 기본 설명을 텍스트로 입력

씬 1 프롬프트:
```
Aerial view of a misty Japanese hinoki cypress forest at dawn, rays of golden sunlight
piercing through fog between ancient tree trunks, morning dew on dark green needles,
cinematic drone shot descending slowly into the forest canopy, volumetric god rays,
film grain, Arri Alexa 65 look, 4K resolution, serene and mystical atmosphere
```

씬 2 프롬프트:
```
Extreme macro shot of a single dewdrop on a hinoki cypress leaf, the droplet contains a
perfect reflection of the forest canopy, golden morning light refracting through the water,
chromatic aberration at edges, Laowa 24mm probe macro lens aesthetic, crystal clear focus on
the droplet with dreamy bokeh background, 8K detail, nature documentary quality
```

씬 3 프롬프트:
```
Clear spring water flowing over smooth river stones in a cypress forest, slow motion at
120fps, individual water molecules catching sunlight creating rainbow prism effects,
the water transitions into a glass laboratory flask through a seamless morph, clean
pharmaceutical aesthetic meets nature, split screen nature-to-product transformation
```

씬 4 프롬프트:
```
A minimalist KINOHI cylindrical white bottle with sage green label standing alone on a
polished white marble pedestal, dramatic single-source side lighting creating long elegant
shadows, the bottle slowly rotates 15 degrees, fine mist emanates from the nozzle tip,
particles suspended in volumetric light beam, luxury perfume commercial aesthetic,
black background transitioning to cream, Profoto studio lighting setup
```

씬 5 프롬프트:
```
Clean white frame with generous negative space, "KINOHI" appears in elegant thin serif
typography center-screen, subtle particle animation of golden dust motes drifting through
frame, tagline "for all your breaths" fades in below in smaller type, minimal motion
graphics, premium brand identity reveal, cream to white gradient background
```

**노드 ② Prompt Enhancer**
- 추가 방법: + 버튼 → "Prompt Enhancer" 검색 → 추가
- 연결: Prompt Node 출력 → Prompt Enhancer 입력

**노드 ③ Flux 2 Pro Ultra (키프레임 이미지)**
- 추가 방법: + 버튼 → "Flux" 검색 → **Flux 2 Pro Ultra** 선택
- 모델 선택 이유: 최고 해상도 포토리얼 모델. Raw 모드로 후보정 유연성 최대
- 해상도: 2048x2048 이상 (Pro Ultra 전용)
- 각 씬 프롬프트당 1장씩, 총 5장 키프레임 생성

**노드 ④ Relighting Node**
- 추가 방법: + 버튼 → "Relight" 검색 → **Relighting** 추가
- 역할: 5장의 키프레임 이미지 조명 방향/온도를 통일
- 설정: Light Direction → Left 45°, Warm Temperature → 3500K (골든아워 톤 통일)
- 키노히 브랜드 이미지가 모두 자연광 좌측 45도 세팅이므로 이를 재현

**노드 ⑤ Color Grading Node**
- 추가 방법: + 버튼 → "Color" 검색 → **Color Grading** 추가
- 역할: 키노히 브랜드 컬러 톤 적용
- 설정:
  - Shadows → Warm (크림 #F5F0EB 방향)
  - Highlights → Neutral White
  - Saturation → -15% (키노히의 저채도 미니멀 톤)
  - Contrast → Soft (부드러운 대비)

**노드 ⑥ Veo 3.1 Image-to-Video**
- 추가 방법: + 버튼 → "Veo" 검색 → **Veo 3.1** 선택
- 모델 선택 이유: 4K 시네마틱 퀄리티 + 공간 오디오. 브랜드 필름급 품질에 유일한 선택
- 설정: Duration → 5초/씬, Resolution → 4K, Camera Motion → Slow (0.3x)

비디오 프롬프트 (씬 1 예시):
```
Cinematic slow drone descent through misty hinoki cypress forest, camera gently tilts
downward revealing forest floor, volumetric fog rolls between tree trunks, golden sun rays
shift subtly as camera moves, ambient forest sounds with gentle wind, film grain overlay,
24fps cinematic cadence, Terrence Malick visual style, peaceful and transcendent mood
```

**노드 ⑦ Export Node**
- 추가 방법: + 버튼 → "Export" 검색 → 추가
- 설정: Format → MP4 H.265, Resolution → 3840x2160 (4K) 또는 1080x1920 (세로 숏폼)

---

#### 워크플로우 C: "계절별 제품 이미지 시리즈 자동 생성"

```
[노드 구성도]

① Import Node ──→ ② Router Node ──→ ③-A Flux 2 Pro (Spring)
   (제품 이미지)                     ├──→ ③-B Flux 2 Pro (Summer)
                                    ├──→ ③-C Flux 2 Pro (Autumn)
                                    └──→ ③-D Flux 2 Pro (Winter)
                                              │
                                    ④ Color Grading (각 계절 톤)
                                              │
                                    ⑤ Upscaler (고해상도 변환)
                                              │
                                    ⑥ Export Node (PNG 출력)
```

**노드 ③-A Flux 2 Pro (Spring)**

프롬프트:
```
A minimalist KINOHI white cylindrical bottle placed on a pale pink marble surface,
scattered soft pink cherry blossom petals around the base, a single delicate branch of
sakura flowers leaning against the bottle, soft diffused spring morning light from the left,
pastel pink and cream color palette, clean negative space on right side for text overlay,
editorial product photography, overhead angle at 30 degrees, 8K resolution,
Kinfolk magazine aesthetic
```

**노드 ③-B Flux 2 Pro (Summer)**

프롬프트:
```
A minimalist KINOHI white cylindrical bottle sitting on a block of crystal clear melting
ice, thin slices of yuzu citrus and fresh mint leaves arranged artfully around the base,
condensation droplets on the bottle surface catching bright summer sunlight, cool blue
and white color palette with subtle yellow accents, harsh directional noon sun creating
crisp shadows, fresh and invigorating mood, editorial flat lay with generous white space,
8K resolution, bright and airy summer product photography
```

**노드 ③-C Flux 2 Pro (Autumn)**

프롬프트:
```
A minimalist KINOHI white cylindrical bottle resting on a weathered dark walnut wood
surface, surrounded by dried autumn leaves in amber and burgundy, a small bundle of
dried lavender tied with natural twine beside the bottle, warm late afternoon golden
light streaming from a low angle, rich warm earth tones — amber, burnt sienna, deep
green, cozy and intimate atmosphere, editorial still life photography, shallow depth
of field, 8K resolution, hygge aesthetic
```

**노드 ③-D Flux 2 Pro (Winter)**

프롬프트:
```
A minimalist KINOHI white cylindrical bottle placed on fresh untouched snow surface,
a small lit beeswax candle in a ceramic holder beside it casting warm glow, delicate
frost crystals forming on the edges of the frame, cool blue shadows on snow contrasting
with warm candle light creating dual color temperature, minimalist winter landscape
blurred in background, serene and peaceful atmosphere, editorial product photography
with cinematic lighting, 8K resolution, Scandinavian winter aesthetic
```

---

#### 워크플로우 D: "AI UGC 리뷰어 영상" (AI 아바타 + 제품)

```
[노드 구성도]

① Import Node ──→ ② Flux 2 Pro ──→ ③ Higgsfield Video
   (제품 이미지)    (AI 인물+제품     (시네마틱 카메라
                    합성 이미지)       제품 시연 영상)
                                          │
                                  ④ Export Node
                                     (MP4 출력)
                                          │
                            ⑤ [외부] Arcads 또는 MakeUGC API
                               (AI 아바타 리뷰 음성 합성)
```

**노드 ② Flux 2 Pro (AI 인물 + 제품 합성)**

프롬프트:
```
A natural-looking Korean woman in her late 20s with minimal makeup, shoulder-length
black hair slightly tousled, wearing a cream cashmere turtleneck, sitting at a clean
white desk holding a KINOHI body mist bottle in her right hand at chest height, looking
directly at camera with a warm genuine smile, soft ring light reflection in her eyes,
clean white wall background with a small potted eucalyptus plant, YouTube beauty reviewer
setup, Canon EOS R5 with 50mm f/1.2 lens, natural skin texture with subtle pores visible,
authentic and relatable Korean beauty influencer aesthetic
```

프롬프트 작동 원리:
이 프롬프트는 UGC의 핵심인 "진짜 사람 같은 자연스러움"을 구현한다. "minimal makeup", "slightly tousled hair", "genuine smile", "natural skin texture with subtle pores visible"가 AI 이미지의 부자연스러운 완벽함을 의도적으로 깨뜨려 실제 리뷰어처럼 보이게 만든다. "YouTube beauty reviewer setup"은 AI에게 정확한 컨텍스트(조명, 앵글, 배경)를 제공한다.

**노드 ③ Higgsfield Video**
- 추가 방법: + 버튼 → "Higgsfield" 검색 → **Higgsfield** 선택
- 모델 선택 이유: 카메라 앵글 정밀 제어 + 시네마틱 무드. 제품 시연 장면에 최적

비디오 프롬프트:
```
The woman picks up the KINOHI bottle, brings it close to her neck, sprays once with a
natural wrist motion, closes her eyes briefly and smiles contentedly, then looks back at
camera, camera: medium close-up with slow subtle zoom-in during spray moment, soft
focus shift from bottle to face, warm natural lighting, authentic UGC feel
```

---

## Part 3. 프롬프트 마스터 가이드

### 3-1. KINOHI 전용 프롬프트 공식

모든 키노히 이미지/비디오 프롬프트에 반드시 포함할 키워드 세트:

**브랜드 톤 키워드 (매 프롬프트 끝에 추가):**
```
minimal and serene atmosphere, cream and white color palette with subtle sage green
accents, generous negative space, soft natural lighting, premium editorial quality,
Kinfolk magazine aesthetic
```

**절대 쓰면 안 되는 키워드 (키노히 톤 파괴):**
- neon, vibrant, bold colors, busy background, cluttered
- HDR, oversaturated, dramatic contrast
- plastic texture, glossy, shiny

### 3-2. 네거티브 프롬프트 (모든 이미지 노드에 적용)
```
text overlay, watermark, logo text, blurry, low quality, oversaturated colors,
neon lighting, cluttered background, plastic looking skin, artificial, stock photo feel,
multiple bottles, distorted proportions, extra fingers, deformed hands
```

---

## Part 4. 콘텐츠 배포 + n8n 자동화 연동

### 4-1. Weavy → n8n 자동화 파이프라인

Weavy에서 콘텐츠가 생성되면, n8n이 나머지를 자동으로 처리:

n8n Webhook (Weavy Export 완료 트리거)
→ n8n IF Node (이미지 vs 비디오 분기)
→ [이미지] n8n HTTP Request → 인스타그램 Graph API → 자동 피드 포스팅
→ [비디오] n8n HTTP Request → TikTok Business API → 자동 릴스 포스팅
→ n8n Slack Node → #marketing 채널에 "새 콘텐츠 발행 완료" 알림

### 4-2. 월간 콘텐츠 캘린더 자동화

n8n Schedule Trigger (매주 월/수/금 오전 9시)
→ n8n Google Sheets (콘텐츠 캘린더에서 오늘 날짜 씬 정보 읽기)
→ n8n HTTP Request (Weavy API 호출 → 해당 씬 워크플로우 실행)
→ Weavy 이미지/비디오 생성
→ n8n Export 수신 → 자동 배포

---

## Part 5. 예상 비용 및 ROI

### 5-1. Weavy 크레딧 소모 예상 (월간)

| 콘텐츠 유형 | 수량/월 | 모델 | 크레딧/건 | 월 크레딧 |
|-------------|---------|------|----------|----------|
| 제품 이미지 (Flux 2 Pro) | 20장 | Flux 2 Pro | 7 | 140 |
| 라이프스타일 이미지 | 10장 | Flux 2 Pro Ultra | 10 | 100 |
| 숏폼 비디오 (Kling 3) | 8편 | Kling 3 | 50 | 400 |
| 시네마틱 비디오 (Veo 3.1) | 4편 | Veo 3.1 | 100 | 400 |
| UGC 영상 (Higgsfield) | 4편 | Higgsfield | 60 | 240 |
| 편집(Relight+Color) | 30건 | - | 3 | 90 |
| **합계** | | | | **약 1,370** |

Weavy Professional 플랜($36/월) 크레딧으로 약 커버 가능. 초과 시 $10/1,200 크레딧 추가.

### 5-2. 기존 방식 vs AI 비용 비교

| 항목 | 기존 (전통 촬영) | AI (Weavy 파이프라인) | 절감률 |
|------|-----------------|---------------------|--------|
| 제품 촬영 (20장/월) | 150만원 | 약 5만원 (크레딧) | 97% |
| 숏폼 영상 (8편/월) | 400만원 | 약 15만원 | 96% |
| UGC 인플루언서 (4편/월) | 200만원 | 약 8만원 | 96% |
| 시네마틱 광고 (4편/월) | 500만원 | 약 20만원 | 96% |
| **월 합계** | **1,250만원** | **약 48만원 + Weavy $36** | **96%** |

---

## Sources

- [Weavy 공식](https://www.weavy.ai/)
- [Weavy 노드 가이드](https://help.weavy.ai/en/articles/12292386-understanding-nodes)
- [키노히 공식몰](https://kinohi.kr/)
- [키노히 무신사](https://www.musinsa.com/brand/kinohi)
- [키노히 신제품 출시 기사](https://www.gokorea.kr/news/articleView.html?idxno=753106)
- [키노히 글로우픽 리뷰](https://glowpick.co.kr/product/177550)
- [AI UGC 플랫폼 비교 2026](https://videoai.me/blog/best-ai-ugc-generators-2026)
- [Arcads AI UGC](https://www.arcads.ai/)
- [MakeUGC](https://www.makeugc.ai/)
