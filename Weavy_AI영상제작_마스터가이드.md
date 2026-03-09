# Weavy.ai + Higgsfield AI 영상 제작 마스터 가이드
## FlowPilot | 2026.03.01 | PDF 분석 + 실전 경험 통합본

---

## PART 1: AI 스토리텔링 영상 제작 10단계 프로세스

> 출처: AI영상제작프로세스_영상독학.pdf (15페이지) 전체 분석

---

### STEP 01: 스토리 설계 (Story Design)

**핵심 3요소:**

1. **감정 곡선 설계** — 기승전결 구조로 시청자의 감정 흐름을 미리 설계한다. 시작(호기심) → 전개(몰입) → 클라이맥스(감동/놀람) → 엔딩(여운)
2. **핵심 메시지 정의** — 하나의 영상에 하나의 메시지만 집중한다. "이 영상을 보고 나서 시청자가 뭘 느끼고, 뭘 하길 원하는가?"를 먼저 정의
3. **타겟 설정** — 누구에게 보여줄 것인지 명확히 정의한다. 타겟에 따라 톤앤매너, 비주얼 스타일, 영상 길이가 완전히 달라진다

**FlowPilot 적용 규칙:**
- 브랜드 영상: 30초 (숏폼 최적화, 브랜드 스토리텔링 ROI 최고)
- UGC/리뷰 영상: 15~30초
- 제품 소개 영상: 45~60초
- 모든 영상은 첫 3초에 Hook(시선 잡기)이 반드시 있어야 한다

---

### STEP 02: 프롬프트 작성 (Prompt Writing)

**필수 3계층 구조:**

1. **장면 묘사 (Scene Description)** — 무엇이 보이는지 구체적 시각 정보. 인물의 위치, 표정, 동작, 의상, 소품 전부 명시
2. **카메라/조명 지정 (Camera & Lighting)** — 카메라 앵글(와이드, 미디엄, 클로즈업), 렌즈(85mm, 35mm 등), 조명 스타일(자연광, 림라이트 등)
3. **감정/분위기 (Emotion & Mood)** — 캐릭터의 감정 상태와 전체 분위기. "평온한", "긴장된", "몽환적인" 등

**Work Flow:**
장면 묘사 작성 → 카메라/조명 추가 → 감정/분위기 키워드 삽입 → 극사실주의 블록 삽입 → 체크리스트 확인

---

### STEP 03: 레퍼런스 수집 (Reference Collection)

**3가지 레퍼런스 유형:**

1. **비주얼 레퍼런스** — Pinterest, Behance 등에서 원하는 스타일의 이미지 수집
2. **무드보드 구성** — 전체 영상의 톤앤매너를 통일하기 위한 무드보드 작성
3. **캐릭터 설정** — 일관된 캐릭터 비주얼 유지를 위한 레퍼런스 확보

**Pro Tips:**
- 구도 연구: 영화 스틸컷 분석 (특히 Wong Kar-wai, Roger Deakins 촬영감독 작품)
- 트렌드 파악: SNS 인기 콘텐츠 연구 (인스타 릴스, 틱톡 트렌드)
- FlowPilot에서는 클라이언트가 제공한 기존 브랜드 에셋을 레퍼런스로 최우선 활용

---

### STEP 04: 캐릭터 생성 (Character Creation) — Higgsfield AI

**Higgsfield AI 활용법:**

1. **정면/측면/전신** 등 다양한 앵글로 캐릭터 기본 이미지 생성
2. **일관성 유지** — 동일 시드값과 프롬프트 핵심 요소를 고정하여 여러 포즈에서 같은 인물 유지
3. **의상/소품 설정** — 스토리에 맞는 의상과 소품을 미리 설정

**Work Flow:**
캐릭터 기본 생성 (정면) → 다양한 앵글 확보 (측면, 3/4뷰, 전신) → 의상/소품 변경 버전 생성

**Weavy에서의 캐릭터 생성:**
- Flux 2 Pro 노드 사용. Action: **Generate Image**
- Image 1 포트(녹색)에 레퍼런스 인물 사진 연결 → 얼굴/스타일 참조
- 주의: Image 1은 완벽한 얼굴 복사가 아님. 스타일과 대략적 특징만 참조됨
- 완벽한 얼굴 일관성이 필요하면 Higgsfield AI에서 먼저 캐릭터를 확정한 후 Weavy로 가져와야 함

---

### STEP 05: 장면별 이미지 생성 (Scene Image Generation) — 키프레임

**핵심 원칙:**

1. **씬별 키프레임** — 각 장면의 대표 이미지 1장을 먼저 생성. 이것이 영상의 첫 프레임(First Frame)이 된다
2. **구도와 앵글** — 스토리보드에서 설계한 카메라 앵글을 프롬프트에 정확히 반영
3. **배경 일관성** — 같은 세계관의 배경이 씬마다 달라지지 않도록 핵심 키워드 고정

**Pro Tips:**
- 배치 생성: 한 장면에 여러 버전을 생성한 후 가장 좋은 것을 선택 (가챠 방식)
- 프롬프트 변형: 핵심 요소(인물, 배경, 조명)는 유지하면서 세부 디테일만 조정하여 다양한 결과물 확보

**Weavy 도구:**
- **Flux 2 Pro** — Action: **Generate Image**. 키프레임 이미지 생성의 핵심 도구
- Size 설정: Portrait 16 9 (9:16 세로형 숏폼용)
- Prompt 포트(빨간색): 장면 프롬프트 입력
- Image 1 포트(녹색, 선택사항): 레퍼런스 이미지 입력

---

### STEP 06: 배경 생성 (Background Generation) — Flux 활용

**핵심 원칙:**

1. **Flux 2 Pro로 고품질 배경 생성** — 배경만 따로 생성할 때는 프롬프트에 반드시 "NO person, NO human figure, empty scene" 포함
2. **시간대/날씨 설정** — 스토리에 맞는 시간대(dawn, golden hour, night), 날씨(misty, rainy, clear sky) 명시
3. **레이어 분리** — 캐릭터와 배경을 따로 생성하여 STEP 07에서 합성

**Work Flow:**
배경 컨셉 설정 → Flux 2 Pro로 배경 이미지 생성 → STEP 07에서 캐릭터와 합성

**Weavy 도구:**
- **Flux 2 Pro** — Action: **Generate Image**. 배경 전용 생성
- 절대 주의: 배경만 생성할 때 "NO product, NO person" 키워드 필수. 빠지면 AI가 임의의 인물/제품을 넣음

---

### STEP 07: 캐릭터 + 배경 합성 (Compositing) — Compositor

**핵심 원칙:**

1. **Compositor 활용** — 캐릭터 이미지(투명 배경)와 배경 이미지를 레이어로 합성
2. **조명 일치** — 캐릭터와 배경의 조명 방향이 같아야 자연스러움. 프롬프트 작성 시 동일한 조명 조건 사용
3. **자연스러운 그림자** — 합성 후 경계면이 어색하지 않도록 처리

**Pro Tips:**
- 엣지 블렌딩: 캐릭터와 배경의 경계면을 자연스럽게 처리
- 색감 매칭: 캐릭터와 배경의 색온도를 통일 (프롬프트에서 동일 K값 사용)

**Weavy 도구:**
- **Compositor** — 두 개 이상의 이미지를 레이어로 합성
- 절대 주의: Compositor는 반드시 이미지 입력을 먼저 연결한 후에 열어야 한다. 빈 상태에서 열면 Painter 캔버스만 보임 (실수 #2)
- **SD3 Remove Background** — 캐릭터 이미지에서 배경 제거(투명화) 후 Compositor에 넣을 때 사용

**절대 금지:**
- Ideogram Replace Background 사용 금지 → 제품/캐릭터를 완전히 재생성함 (실수 #1)
- Bria Replace Background 사용 금지 (배경이 이미 있을 때) → 텍스트로 새 배경을 생성하므로 기존 배경과 전혀 다른 결과 (실수 #3)
- Flux I2I 사용 금지 (제품/캐릭터 보존 필요 시) → 디테일 왜곡됨 (실수 #5)

---

### STEP 08: 이미지 정교화 (Image Refinement)

**3단계 정교화 프로세스:**

1. **인페인팅 (Inpainting)** — 이미지의 특정 부분만 선택하여 수정. 어색한 손가락, 불필요한 사물 등을 자연스럽게 제거하거나 교체
2. **리터칭 (Retouching)** — 전체적인 톤앤매너를 맞추기 위해 불필요한 노이즈나 요소를 정밀하게 제거
3. **색보정 (Color Correction)** — 영상 전체의 흐름을 방해하는 튀는 색감을 미리 보정. 컷 간의 연결이 자연스럽도록 컬러 그레이딩의 기초 작업

**Work Flow:**
인페인팅 (디테일 오류 수정) → 리터칭 (불필요 요소 제거) → 색보정 (톤앤매너 정리)

**Weavy 도구:**
- **Higgsfield AI 인페인팅** — 특정 부분만 선택하여 AI가 자연스럽게 수정
- **Compositor** — 이미지 위에 수동으로 브러시/레이어 수정 가능 (이미지 입력 연결 후)
- 외부 도구: Photoshop / Lightroom으로 최종 색보정 (Weavy 내에서는 한계 있음)

---

### STEP 09: 애니메이션 적용 (Animation) — 이미지 → 영상 변환

**3가지 핵심 AI 영상 생성 도구:**

#### A. Higgsfield AI 오리지널 템플릿
- 검증된 오리지널 모션 템플릿을 활용하여 안정적이고 퀄리티 높은 모션 적용
- 얼굴 표정, 머리카락 움직임, 고개 돌림 등 사전 정의된 모션 패턴 활용
- 캐릭터 일관성 유지에 가장 강력

#### B. Kling (클링) — Weavy.ai 내장
- **반드시 동작 순서를 명확히 지정**: 복합 행동은 번호를 매겨 순서 지시
  - 예: "1. 서로 바라본다 → 2. 음료를 마신다 → 3. 미소 짓는다"
- 모델 버전별 차이:
  - **Kling 3**: 최신, 최고 품질. 브랜드 영상 제작 시 기본 선택
  - **Kling o1**: 구버전, 품질 낮음. 비율 제어 안 됨. 사용 비추천
  - **Kling o3 Edit Video**: 기존 영상의 특정 부분만 텍스트 지시로 수정
  - **Kling Motion Control**: 기존 영상에 정밀한 카메라 무빙 적용 (dolly, pan, tilt, zoom, orbit)
  - **Kling o1 Reference Video to Video**: 모션 패턴 전사 전용. 인물+배경 합성용이 절대 아님 (실수 #8)

#### C. HailuoAI (하이루오)
- Kling과 교차 사용하여 장단점 보완
- 도구마다 강점이 다르므로 같은 프롬프트라도 여러 도구에서 생성하여 비교 선택

#### D. Veo 3.1 (Google DeepMind) — Weavy.ai 내장
- 카메라 무빙 품질이 특히 우수
- 자연스러운 인체 움직임과 물리 시뮬레이션에 강점
- First Frame 포트에 키프레임 이미지 넣고 영상 생성

**Pro Tips (매우 중요):**
- **컷 나눠서 연결하기**: START/END 프레임을 지정하는 것보다, 컷을 분할하여 각각 생성한 후 연결하는 것이 훨씬 자연스러움
- **반복 시도와 선택 (가챠)**: 한 번에 완벽할 수 없다. 여러 번 생성하여 베스트 선택. 크레딧을 아끼지 말 것
- **5초 단위 분할**: 30초 영상 = 6개 x 5초 클립. 각 클립을 독립적으로 생성

---

### STEP 10: 영상 편집 (Video Editing)

**3가지 핵심:**

1. **효과음 직접 선택** — Artlist, Epidemic Sound 등에서 영상 분위기에 맞는 배경음악과 효과음을 직접 선별
2. **병렬 프로세스 진행** — 애니메이션 생성과 영상 편집을 동시에 진행. 컷과 컷 사이의 호흡을 실시간으로 확인
3. **반복 테스트로 완성도 향상** — 한 번에 완성하려 하지 말고, 여러 번의 모니터링과 수정을 거쳐 최종 퀄리티 확보

**Pro Tools:**
- Premiere Pro: 정교한 컷 편집, 타이밍 조정
- DaVinci Resolve: 전문 컬러 그레이딩
- Dehancer Pro: 시네마틱 필름 룩 색보정 플러그인
- Audio Mixing: 배경음과 효과음의 밸런스 조정

**Weavy 도구:**
- **Video Concatenator** — 여러 비디오 클립을 순서대로 하나로 합침. 모든 씬 영상 생성 완료 후 마지막에 사용
- 최종 Export 후 Premiere Pro / DaVinci Resolve에서 색보정 + 오디오 믹싱 마무리

---

## PART 2: Weavy.ai 전체 도구 맵 (Tool-Purpose Mapping)

### 이미지 생성 도구

| 도구명 | 용도 | 주요 입력 포트 | 절대 금지 사항 |
|--------|------|---------------|---------------|
| **Flux 2 Pro** (Action: Generate Image) | 키프레임 이미지 생성, 배경 이미지 생성 | Prompt*(빨간색), Image 1(녹색, 선택) | 배경 전용 생성 시 "NO person/product" 필수 |
| **Flux Image-to-Image** | 기존 이미지 스타일 변환 | Image*(녹색), Prompt*(빨간색) | 제품/캐릭터 원본 보존 필요 시 사용 금지 (실수 #5) |

### 배경 처리 도구

| 도구명 | 용도 | 올바른 사용 상황 | 절대 금지 사항 |
|--------|------|-----------------|---------------|
| **SD3 Remove Background** | 이미지에서 배경 제거 (투명화) | 캐릭터/제품을 배경에서 분리할 때 | - |
| **Ideogram Replace Background** | 텍스트 프롬프트 기반 배경 재생성 | 배경이 전혀 없고 처음부터 AI로 만들 때만 | 제품/캐릭터 원본 보존 필요 시 절대 금지 (실수 #1) |
| **Bria Replace Background** | 텍스트 프롬프트 기반 새 배경 AI 생성 | 배경이 전혀 없고 처음부터 AI로 만들 때만 | 이미 배경 이미지가 준비된 상태에서 사용 금지 (실수 #3) |

### 합성 도구

| 도구명 | 용도 | 필수 조건 |
|--------|------|----------|
| **Compositor** | 두 개 이상의 이미지를 레이어로 합성 | 반드시 이미지 입력을 먼저 연결한 후에 열 것 (실수 #2) |

### 비디오 생성 도구

| 도구명 | 용도 | 주요 입력 포트 | 비고 |
|--------|------|---------------|------|
| **Kling 3** (Action: Image to Video) | 키프레임 → 영상 변환 (최고 품질) | Prompt*(빨간색), First Frame(녹색), Last Frame(녹색), Negative Prompt(보라색) | 브랜드 영상 기본 선택. 5초/10초 클립 |
| **Veo 3.1** (Action: Image to Video) | 키프레임 → 영상 변환 (카메라무빙 우수) | Prompt*(빨간색), First Frame(녹색) | 카메라 움직임이 중요한 씬에 사용 |
| **Kling o3 Edit Video** | 기존 영상의 특정 부분만 텍스트로 수정 | Video*(녹색), Prompt*(빨간색) | 이미 생성된 영상에서 일부만 바꿀 때 |
| **Kling Motion Control** | 기존 영상에 정밀 카메라 무빙 적용 | Video*(녹색) + 카메라 무빙 파라미터 | dolly in/out, pan L/R, tilt up/down, zoom, orbit |
| **Kling o1 Reference Video to Video** | 레퍼런스 영상의 모션 패턴을 다른 영상에 전사 | Reference Video(녹색), Image(녹색), Prompt*(빨간색) | 인물+배경 합성용 절대 아님 (실수 #8). 모션 전사 전용 |
| **Kling o1 Edit Video** | 구버전 영상 편집 (o3보다 품질 낮음) | Video*(녹색), Prompt*(빨간색) | o3 Edit 사용 권장 |

### 비디오 후처리 도구

| 도구명 | 용도 |
|--------|------|
| **Video Concatenator** | 여러 비디오 클립을 순서대로 하나로 합침. 모든 씬 완성 후 마지막에 사용 |

---

## PART 3: Weavy 포트 색상 규칙 (절대 암기)

| 포트 색상 | 데이터 타입 | 연결 가능 대상 | 예시 |
|-----------|------------|---------------|------|
| 녹색 | 이미지/비디오 (바이너리) | 같은 녹색 포트끼리만 | First Frame ↔ 이미지 출력 |
| 빨간색 | 텍스트/프롬프트 | 같은 빨간색 포트끼리만 | Prompt ↔ 프롬프트 노드 |
| 보라색 | 모델/파라미터 | 같은 보라색 포트끼리만 | Negative Prompt ↔ 텍스트 파라미터 |

**절대 금지:** 녹색(이미지)를 빨간색(텍스트)에 연결 시도 → 데이터 타입 불일치로 연결 자체 불가 (실수 #6)

---

## PART 4: Higgsfield AI 도구 맵

### Higgsfield 핵심 기능

| 기능 | 용도 | FlowPilot 활용 |
|------|------|---------------|
| **캐릭터 생성** | 일관된 AI 인물 생성 (정면/측면/전신) | 브랜드 전용 모델 생성, AI 인플루언서 제작 |
| **오리지널 모션 템플릿** | 사전 정의된 고품질 모션 패턴 적용 | 표정 변화, 고개 돌림, 머리카락 움직임 등 |
| **인페인팅** | 이미지의 특정 부분만 AI로 수정 | 어색한 손가락, 불필요 사물 제거, 디테일 수정 |
| **의상/소품 변경** | 동일 캐릭터의 의상과 소품을 변경 | 씬별 의상 교체, 계절별 의상 변경 |

### Higgsfield + Weavy 하이브리드 워크플로우

1. **Higgsfield에서 캐릭터 기본 생성** (정면, 측면, 3/4뷰, 전신)
2. **Higgsfield 인페인팅으로 디테일 수정** (손가락, 의상 등)
3. **Weavy Flux 2 Pro로 키프레임 생성** (Higgsfield 캐릭터를 Image 1에 레퍼런스로 입력)
4. **Weavy SD3 Remove Background로 배경 제거**
5. **Weavy Flux 2 Pro로 배경 별도 생성**
6. **Weavy Compositor로 캐릭터 + 배경 합성**
7. **Weavy Kling 3 / Veo 3.1로 합성 키프레임 → 영상 변환**
8. **Weavy Video Concatenator로 클립 합치기**
9. **Premiere Pro / DaVinci Resolve에서 최종 편집**

---

## PART 5: 극사실주의 프롬프트 엔지니어링 페르소나

### 페르소나: "HyperReal Director"

> 나는 20년 경력의 시네마토그래퍼이자 상업 사진작가다. 내가 촬영하는 모든 이미지와 영상은 실제 현장에서 실제 카메라로 촬영한 것과 1:1 동일해야 한다. AI가 만들었다는 티가 0.1%라도 나면 그것은 실패작이다. 나는 인간 피부의 모공 하나, 눈의 홍채 섬유질, 손톱 아래의 때, 발가락의 각질, 머리카락의 갈라진 끝까지 전부 보이는 수준의 극사실주의만을 추구한다.

### 이미지 프롬프트 필수 삽입 블록 (Flux 2 Pro 등 — 인물 포함 시)

모든 인물 이미지 프롬프트의 끝에 반드시 아래 블록을 전체 삽입한다. 한 글자도 빠지면 안 된다:

```
hyper-realistic photorealistic photograph indistinguishable from real DSLR photo, ultra-detailed skin texture with visible pores and micro skin imperfections and fine peach fuzz hair and natural skin blemishes and subtle freckles, real human skin subsurface scattering with natural blood vessel undertones, individual hair strands visible with natural flyaway hairs and split ends, eyes with detailed iris fibers and natural eye moisture reflection and visible blood vessels in sclera, natural asymmetric facial features as real humans have, visible fingernail texture with natural nail ridges and cuticle detail, toenail with realistic keratin texture and natural imperfections, natural body hair on arms and legs visible under light, realistic ear canal detail and ear lobe texture, natural lip texture with visible lip lines and slight dryness, teeth with natural slight discoloration and realistic enamel reflection, shot on real camera sensor with natural lens aberration and chromatic fringing at edges, realistic depth of field with optical bokeh circles showing cat-eye effect at frame edges, natural film grain matching ISO setting, absolutely ZERO artificial smooth airbrushed plastic skin, ZERO uncanny valley effect, ZERO CGI render look, ZERO digital art style, ZERO perfect symmetry, must pass as genuine unedited photograph taken by professional photographer
```

### 비디오 프롬프트 필수 삽입 블록 (Kling 3, Veo 3.1 등 — 인물 포함 시)

모든 인물 비디오 프롬프트의 끝에 반드시 아래 블록을 전체 삽입한다:

```
hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin with visible pores and micro expressions and natural skin texture movement during motion, realistic hair physics with individual strands responding to movement and gravity and breeze naturally, natural subtle body micro-movements like breathing pulse and muscle tension shifts and weight distribution changes, realistic eye movement with natural blink rate and eye moisture and pupil dilation, fabric physics responding realistically to body movement and air and gravity, visible fingernail and toenail detail during hand and feet close-ups, natural body hair visible on arms catching light, realistic ear and neck skin texture during head turns, natural lip movement with realistic moisture and texture, natural motion blur consistent with real camera shutter speed, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, ZERO robotic stiff movement, must pass as genuine footage from professional cinema camera
```

### 제품 전용 프롬프트 (인물 없는 제품샷)

인물이 없는 순수 제품 사진에는 위 인물 블록 대신 아래 블록을 삽입한다:

```
hyper-realistic product photography indistinguishable from real studio photograph, ultra-detailed material textures with visible surface micro-texture and natural light interaction, realistic glass or plastic refraction and reflection with visible internal light caustics, natural shadow falloff with realistic ambient occlusion and contact shadows, visible label printing texture and slight label edge lifting, realistic liquid meniscus visible through transparent container, dust particles visible on surface under studio light, shot on medium format camera with natural lens characteristics and shallow depth of field, absolutely ZERO CGI render look, ZERO 3D modeling aesthetic, must pass as genuine product photo from commercial studio shoot
```

### 극사실주의 Negative Prompt 표준 블록 (모든 이미지/비디오 공통)

```
smooth skin, plastic skin, airbrushed skin, porcelain skin, wax figure, mannequin, doll-like, perfect symmetry, CGI, 3D render, cartoon, anime, digital art, digital painting, illustration, concept art, fantasy art, video game, unreal engine, unity, blender render, octane render, vray render, low quality, blurry, distorted, extra fingers, extra limbs, watermark, ugly, deformed, overprocessed, beauty filter, glamour retouch, magazine cover retouch, skin smoothing, frequency separation, green screen look, obvious compositing, mismatched lighting, floating objects, uncanny valley
```

### 절대 금지 키워드 (프롬프트에서 발견 시 즉시 교체)

| 금지 키워드 | 교체 키워드 |
|------------|-----------|
| smooth skin | textured skin with visible pores and micro imperfections |
| perfect face | naturally asymmetric human face with real skin texture |
| beautiful lighting | [실제 카메라 모델 + 렌즈 모델 + 조리개값으로 교체] |
| high quality | photorealistic DSLR photograph / cinema camera footage |
| 4K 또는 8K 단독 사용 | 카메라 모델 + 렌즈 + ISO + 색온도 K값과 함께 사용 |
| flawless skin | skin with natural blemishes and visible pores |
| perfect teeth | teeth with natural slight irregularities |
| beautiful eyes | eyes with detailed iris fibers and natural blood vessels in sclera |

### 프롬프트 체크리스트 (매번 반드시 확인)

프롬프트 작성 완료 후 아래 항목이 전부 포함되어 있는지 체크한다. 하나라도 빠지면 프롬프트를 수정한다:

- [ ] 실제 카메라 모델명 (Sony A7R V, Arri Alexa Mini LF, Canon EOS R5, RED V-Raptor, Sony Venice 2 등)
- [ ] 실제 렌즈 모델명 + 조리개값 (Sony FE 85mm f/1.4 GM, Cooke Anamorphic SF 40mm, Zeiss Supreme Prime 65mm T1.5 등)
- [ ] 색온도 K값 (3200K warm tungsten, 4500K cool moonlight, 5600K daylight 등)
- [ ] ISO/필름그레인 수준 (ISO 400 clean, ISO 800 natural film grain, ISO 3200 visible grain 등)
- [ ] "visible pores" 또는 "skin texture with pores" 포함 여부
- [ ] "hyper-realistic" 또는 "photorealistic" 포함 여부
- [ ] "ZERO AI look" 또는 "indistinguishable from real photo/footage" 포함 여부
- [ ] Negative prompt에 극사실주의 표준 블록 전체 포함 여부
- [ ] 인물 프롬프트에 극사실주의 삽입 블록 전체 포함 여부

---

## PART 6: 콘텐츠 유형별 올바른 워크플로우 경로

### A. 브랜드 홍보 영상 (30초 숏폼)

```
[스토리보드 설계 (6씬 x 5초)]
    ↓
[씬별 키프레임 이미지 생성 — Flux 2 Pro]
    ↓
[필요 시 배경 별도 생성 — Flux 2 Pro (NO person)]
    ↓
[필요 시 합성 — SD3 Remove BG → Compositor]
    ↓
[씬별 영상 생성 — Kling 3 또는 Veo 3.1 Image to Video]
    ↓
[Video Concatenator로 클립 합치기]
    ↓
[Premiere Pro / DaVinci Resolve에서 최종 편집 + 오디오]
```

### B. AI 인플루언서 / UGC 콘텐츠

```
[Higgsfield AI에서 캐릭터 생성 (정면/측면/전신)]
    ↓
[Higgsfield 인페인팅으로 디테일 수정]
    ↓
[Weavy Flux 2 Pro로 씬별 키프레임 (Image 1에 캐릭터 레퍼런스)]
    ↓
[Kling 3 / Veo 3.1 Image to Video로 영상 변환]
    ↓
[Higgsfield 오리지널 템플릿으로 추가 모션 적용 (선택)]
    ↓
[Video Concatenator → 최종 편집]
```

### C. 제품 소개 영상 (제품 원본 보존 필수)

```
[제품 실물 사진 촬영/확보]
    ↓
[SD3 Remove Background로 배경 제거]
    ↓
[Flux 2 Pro로 배경 별도 생성 (NO product 필수)]
    ↓
[Compositor로 제품 + 배경 합성]
    ↓
[합성 이미지를 Kling 3 First Frame에 넣고 영상 생성]
    ↓
[Video Concatenator → 최종 편집]
```

### D. 인물+배경 합성 영상 (숲속 모델 등)

```
[절대 금지: Kling o1 Reference Video to Video 사용 금지 (실수 #8)]

[올바른 방법:]
[Flux 2 Pro로 "인물+배경이 함께 있는" 합성 키프레임 이미지 1장 생성]
    ↓
[이 키프레임을 Kling 3 / Veo 3.1의 First Frame에 직접 입력]
    ↓
[영상 프롬프트로 동작/카메라무빙 지시]
    ↓
[영상 생성]
```

---

## PART 7: 실수 기록 & 절대 금지 사항 (전체 8건)

> 상세 내용은 `Weavy_실수기록_절대금지사항.md` 참조

| # | 실수 내용 | 낭비 비용 | 올바른 방법 |
|---|----------|----------|-----------|
| 1 | Ideogram Replace Background로 제품 배경 교체 시도 → 제품 자체가 재생성됨 | 크레딧 | Compositor 사용 |
| 2 | Compositor를 이미지 입력 없이 열음 → 빈 캔버스만 표시 | 시간 | 이미지 입력 먼저 연결 후 열기 |
| 3 | Bria Replace Background에 이미 완성된 Flux 배경이 있는데 사용 → 전혀 다른 배경 생성 | 크레딧 | Compositor로 합성 |
| 4 | "Composite" / "Simple Blend" 노드를 가이드함 → Weavy에 존재하지 않음 | 시간 | 실제 UI 확인 후 가이드 |
| 5 | Flux I2I로 제품 배경 교체 → 병뚜껑, 라벨 디테일 왜곡 | 크레딧 | SD3 Remove BG + Compositor |
| 6 | File(이미지) 출력 → Prompt(텍스트) 입력 직접 연결 → 포트 타입 불일치 | 시간 | 포트 색상 확인 (녹↔녹, 빨↔빨) |
| 7 | "Weavy API + n8n" 파이프라인 제안 → Weavy에 공개 API 없음 | 전략 오류 | fal.ai API 사용 |
| 8 | Kling o1 Reference V2V로 모델+숲 합성 → 1:1 비율, 크로마키 느낌, 동작 없음 | ₩40,000 | 합성 키프레임 → Kling 3/Veo 3.1 I2V |

---

## PART 8: 주의사항 & Pro Tips

### CAUTION (PDF 원본 강조)

1. **Investment Mindset** — 크레딧을 아끼지 말고 과감하게 시도하라. 같은 프롬프트로 3~5회 생성하여 베스트 선택 (가챠 방식)
2. **Quality First** — 무엇보다 완성도가 가장 중요하다. 80% 짜리 10개보다 100% 짜리 1개가 낫다
3. **컷 분할 > START/END 지정** — 하나의 긴 영상을 START/END로 만들기보다, 5초 단위로 컷을 분할하여 각각 생성 후 연결하는 것이 훨씬 자연스럽다

### FlowPilot 추가 원칙

4. **Prompt Enhancer(Any Prompt)는 이미지 전용** — 짧은 프롬프트를 자동 확장하는 기능. 비디오 프롬프트에는 사용 금지 (카메라/모션 지시가 변질됨)
5. **모든 프롬프트는 영어로 작성** — AI 모델은 영어 프롬프트에서 최고 성능
6. **한국어 해설은 별도 제공** — 프롬프트 작동 원리와 각 키워드의 역할을 한국어로 설명
7. **Weavy에 존재하지 않는 노드를 추정/상상으로 가이드하지 마라** — 실제 UI에서 확인된 것만
8. **노드 추천 시 반드시 Action/Operation 선택값까지 명시** — "Flux 2 Pro 추가해" (금지) → "Flux 2 Pro 노드 추가, Action: Generate Image 선택" (필수)

---

## PART 9: 크레딧 소비 기준표

### Weavy Starter Plan ($24/월, 1,500 크레딧)

| 작업 유형 | 크레딧 소비 (추정) | 비고 |
|----------|------------------|------|
| Flux 2 Pro 이미지 1장 | ~30 cr | 키프레임 생성 |
| Kling 3 영상 5초 | ~143 cr | 브랜드 영상 핵심 |
| Veo 3.1 영상 5초 | ~100~150 cr | 카메라무빙 우수 |
| SD3 Remove Background | ~10 cr | 배경 제거 |
| Video Concatenator | ~10 cr | 클립 합치기 |

### 30초 브랜드 영상 1편 예상 소비

| 항목 | 수량 | 크레딧 |
|------|------|--------|
| 키프레임 이미지 | 6장 (+ 여분 3장) | ~270 cr |
| 영상 클립 생성 | 6개 (+ 리테이크 3개) | ~1,287 cr |
| 배경 제거/합성 | 2~3회 | ~30 cr |
| Video Concatenator | 1회 | ~10 cr |
| **합계** | | **~1,597 cr** |

→ Starter 1,500cr로 약간 부족. 리테이크를 최소화하거나 $20 추가 충전 필요
→ Pro Plan ($45, 5,000cr)이면 3편 이상 제작 가능

---

## PART 10: Weavy API 없음 — n8n 자동화 대안

Weavy.ai에는 현재 공개 API가 없다 (실수 #7).

**대안: fal.ai API + n8n**
- fal.ai는 Weavy의 백엔드 인프라
- Flux, Kling 등 동일 모델을 fal.ai API로 n8n에서 직접 호출 가능
- n8n에 fal.ai 공식 인증 노드 존재
- 대량 배치 처리, 자동화 파이프라인 구축 시 fal.ai API 사용

---

*이 문서는 FlowPilot의 Weavy.ai + Higgsfield AI 영상 제작 마스터 가이드이다. AI영상제작프로세스_영상독학.pdf 15페이지 전체 분석과 실전 경험 8건의 실수 기록을 통합하여 작성되었다. 2026.03.01 기준.*
