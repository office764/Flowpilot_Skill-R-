# Project Rules

## FlowPilot AI Collaboration System

When working on **n8n workflows, backend automation, or any task involving server `158.247.245.248`**, the FlowPilot protocol is MANDATORY. Use the `/flowpilot-n8n-backend` command or let the `flowpilot-n8n-backend` skill auto-activate.

### Core Rules (Always Active)

1. **Never hardcode credentials.** Always read from the local `.env` file.
2. **Target server**: `flowpilot@158.247.245.248` (Vultr 24/7).
3. **4-Step Process**: Document Init -> Execute -> QA Self-Check -> Report. No exceptions.
4. **Approval Gates**: Stop and ask for user approval after document generation and after each QA report.
5. **One task at a time**: Execute only the first unchecked `todo.md` item per cycle.
6. **n8n JSON**: Every node must have explicit Operation/Action parameters stated.
7. **Error handling**: Every workflow must include an Error Trigger node or error workflow reference.

### Plugin Location

The full FlowPilot skill and command are installed at:
`~/.claude/plugins/flowpilot-n8n-backend/`

Load with: `claude --plugin-dir ~/.claude/plugins/flowpilot-n8n-backend`

---

## FlowPilot 프롬프트 작성 필수 규칙 (Prompt Generation Rule)

### 규칙: 영문 프롬프트 + 한국어 해설 항상 쌍으로 제공

프롬프트 작성을 요청받을 경우, 반드시 다음 2단계를 따른다:

1. **영어로 프롬프트 작성**: 최고 퀄리티와 정확도를 위해 프롬프트는 항상 영문으로 작성한다.
2. **한국어로 작동 원리 설명**: 해당 영문 프롬프트의 작동 원리, 목적, 각 키워드가 왜 포함되었는지를 한국어로 상세하게 설명한다.

**나쁜 예시 (금지):**
```
프롬프트만 덜렁 영어로 던지고 끝
```

**좋은 예시 (필수):**
```
[영문 프롬프트]
Cinematic 16:9 wide shot of misty Jeju hinoki cypress forest at dawn...

[한국어 작동 원리 설명]
- "Cinematic 16:9": 영화적 화면비를 지정하여 극장 느낌의 시네마틱 비율로 출력
- "misty Jeju hinoki cypress": 키노히의 핵심 원료인 제주 편백나무를 직접 시각화
- "at dawn": 새벽 시간대를 지정하여 골든아워 직전의 신비로운 분위기 연출
- "Arri Alexa Mini LF": 실제 영화 촬영에 사용되는 카메라 모델을 지정하여 AI에게 시네마틱 룩의 정확한 기준점 제공
...
```

이 규칙은 이미지 프롬프트, 비디오 프롬프트, LLM 시스템 프롬프트, n8n AI Agent 프롬프트 등 모든 종류의 프롬프트에 적용된다.

---

## Weavy.ai 극사실주의 필수 프롬프트 규칙 (Hyper-Realism Mandatory Rule)

### 핵심 원칙: AI티가 나면 실패다

모든 이미지 및 비디오 프롬프트에는 반드시 아래 극사실주의 키워드 블록을 포함해야 한다. 예외 없음.

### 이미지 프롬프트 필수 삽입 블록 (Flux 2 Pro 등)

모든 인물 이미지 프롬프트의 끝에 반드시 아래 블록을 삽입한다:

```
hyper-realistic photorealistic photograph indistinguishable from real DSLR photo, ultra-detailed skin texture with visible pores and micro skin imperfections and fine peach fuzz hair and natural skin blemishes and subtle freckles, real human skin subsurface scattering with natural blood vessel undertones, individual hair strands visible with natural flyaway hairs and split ends, eyes with detailed iris fibers and natural eye moisture reflection and visible blood vessels in sclera, natural asymmetric facial features as real humans have, visible fingernail texture with natural nail ridges and cuticle detail, toenail with realistic keratin texture and natural imperfections, natural body hair on arms and legs visible under light, realistic ear canal detail and ear lobe texture, natural lip texture with visible lip lines and slight dryness, teeth with natural slight discoloration and realistic enamel reflection, shot on real camera sensor with natural lens aberration and chromatic fringing at edges, realistic depth of field with optical bokeh circles showing cat-eye effect at frame edges, natural film grain matching ISO setting, absolutely ZERO artificial smooth airbrushed plastic skin, ZERO uncanny valley effect, ZERO CGI render look, ZERO digital art style, ZERO perfect symmetry, must pass as genuine unedited photograph taken by professional photographer
```

### 비디오 프롬프트 필수 삽입 블록 (Kling 3, Veo 3.1 등)

모든 인물 비디오 프롬프트의 끝에 반드시 아래 블록을 삽입한다:

```
hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin with visible pores and micro expressions and natural skin texture movement during motion, realistic hair physics with individual strands responding to movement and gravity and breeze naturally, natural subtle body micro-movements like breathing pulse and muscle tension shifts and weight distribution changes, realistic eye movement with natural blink rate and eye moisture and pupil dilation, fabric physics responding realistically to body movement and air and gravity, visible fingernail and toenail detail during hand and feet close-ups, natural body hair visible on arms catching light, realistic ear and neck skin texture during head turns, natural lip movement with realistic moisture and texture, natural motion blur consistent with real camera shutter speed, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, ZERO robotic stiff movement, must pass as genuine footage from professional cinema camera
```

### 제품 전용 프롬프트 (인물 없는 제품샷)

인물이 없는 순수 제품 사진에는 위 인물 블록 대신 아래 블록을 삽입한다:

```
hyper-realistic product photography indistinguishable from real studio photograph, ultra-detailed material textures with visible surface micro-texture and natural light interaction, realistic glass or plastic refraction and reflection with visible internal light caustics, natural shadow falloff with realistic ambient occlusion and contact shadows, visible label printing texture and slight label edge lifting, realistic liquid meniscus visible through transparent container, dust particles visible on surface under studio light, shot on medium format camera with natural lens characteristics and shallow depth of field, absolutely ZERO CGI render look, ZERO 3D modeling aesthetic, must pass as genuine product photo from commercial studio shoot
```

### 절대 금지 키워드 (이미지 + 비디오 공통)

아래 키워드가 결과물에서 감지되면 즉시 프롬프트를 수정하고 재생성한다:
- smooth skin (매끈한 피부) → 무조건 "textured skin with visible pores" 로 교체
- perfect face (완벽한 얼굴) → 무조건 "naturally asymmetric human face" 로 교체
- beautiful lighting → 무조건 실제 카메라/렌즈 모델명으로 교체
- high quality → 무조건 "photorealistic DSLR photograph" 또는 "cinema camera footage"로 교체
- 4K, 8K 해상도만 단독 사용 금지 → 반드시 카메라 모델 + 렌즈 + ISO + 색온도와 함께 사용

### 프롬프트 체크리스트 (매번 확인)

프롬프트 작성 후 아래 항목이 전부 포함되어 있는지 체크한다:

- [ ] 실제 카메라 모델명 (Sony A7R V, Arri Alexa Mini LF, Canon EOS R5 등)
- [ ] 실제 렌즈 모델명 + 조리개값 (Sony FE 85mm f/1.4 GM, Cooke Anamorphic SF 40mm 등)
- [ ] 색온도 K값 (3200K, 4500K, 5600K 등)
- [ ] ISO/필름그레인 수준 (ISO 800 natural film grain 등)
- [ ] "visible pores" 또는 "skin texture with pores" 포함 여부
- [ ] "hyper-realistic" 또는 "photorealistic" 포함 여부
- [ ] "ZERO AI look" 또는 "indistinguishable from real photo" 포함 여부
- [ ] Negative prompt에 "smooth skin, plastic, CGI, 3D render, cartoon, anime" 포함 여부

---

## Weavy.ai + Higgsfield AI 마스터 가이드 (필독)

**종합 마스터 가이드:** `Weavy_AI영상제작_마스터가이드.md` 참조 (10단계 프로세스 + 도구 맵 + 워크플로우 전체)
**실수 기록:** `Weavy_실수기록_절대금지사항.md` 참조 (실수 8건 상세)

### 핵심 절대 금지 사항 (8개)

1. **Weavy UI에 존재하지 않는 노드명을 가이드하지 마라** — 추정/상상으로 노드명을 만들어내지 않는다
2. **노드 추천 시 반드시 Operation/Action 선택값까지 명시하라** — "Flux 2 Pro 추가해" (금지) → "Flux 2 Pro 노드 추가, Action: Generate Image 선택" (필수)
3. **제품/캐릭터 원본 보존 필요 시 AI 재생성 도구 사용 금지** — Ideogram Replace Background, Flux I2I 등은 원본을 왜곡한다
4. **포트 색상(데이터 타입)을 확인하지 않고 연결 지시 금지** — 녹색(이미지)↔빨간색(텍스트) 연결 불가
5. **이미 완성된 에셋이 있는데 같은 것을 다시 AI 생성하라고 하지 마라** — Flux 배경이 있는데 Bria로 배경을 또 생성하는 실수
6. **생략 금지** — 코드, 설정값에서 `...` 같은 생략 표시 절대 사용 금지
7. **Weavy에는 현재 공개 API가 없다** — n8n 연동 시 fal.ai API를 사용해야 한다
8. **인물+배경 합성 영상에 Kling o1 Reference V2V 사용 금지** — 합성 키프레임 이미지를 Kling 3/Veo 3.1 First Frame에 넣는 것이 올바른 방법

### AI 영상 제작 10단계 프로세스 (요약)

1. 스토리 설계 (감정 곡선 + 핵심 메시지 + 타겟)
2. 프롬프트 작성 (장면 묘사 + 카메라/조명 + 감정/분위기 + 극사실주의 블록)
3. 레퍼런스 수집 (비주얼 + 무드보드 + 캐릭터 설정)
4. 캐릭터 생성 (Higgsfield AI — 정면/측면/전신)
5. 장면별 키프레임 이미지 생성 (Flux 2 Pro — Action: Generate Image)
6. 배경 생성 (Flux 2 Pro — "NO person/product" 필수)
7. 캐릭터+배경 합성 (SD3 Remove BG → Compositor)
8. 이미지 정교화 (인페인팅 → 리터칭 → 색보정)
9. 애니메이션 적용 (Kling 3/Veo 3.1 Image to Video — 5초 단위 클립)
10. 영상 편집 (Video Concatenator → Premiere Pro/DaVinci Resolve + 오디오)

### Weavy 도구 선택 기준표

| 상황 | 올바른 도구 | 잘못된 도구 (사용 금지) |
|------|------------|----------------------|
| 제품 원본 유지 + 배경 교체 | Compositor (이미지 입력 연결 후 사용) | Ideogram Replace Background (제품 재생성됨) |
| 배경이 이미 준비된 상태 | Compositor (두 이미지 레이어 합성) | Bria Replace Background (배경을 텍스트로 새로 생성) |
| 배경 없이 처음부터 생성 | Replace Background 또는 Bria Replace Background | - |
| 제품 배경 제거(투명화) | SD3 Remove Background | Flux I2I (제품 왜곡됨) |
| 배경만 별도 생성 | Flux 2 Pro (Action: Generate Image, "NO product" 필수) | - |
| 키프레임 → 영상 변환 | Kling 3 (Action: Image to Video) 또는 Veo 3.1 (Action: Image to Video) | Kling o1 (구버전, 품질 낮음) |
| 인물+배경 합성 영상 | 합성 키프레임 이미지 → Kling 3/Veo 3.1 First Frame | Kling o1 Reference V2V (실수 #8) |
| 기존 영상 부분 수정 | Kling o3 Edit Video | Kling o1 Edit (구버전) |
| 카메라 무빙 정밀 제어 | Kling Motion Control (dolly/pan/tilt/zoom/orbit) | - |
| 클립 합치기 | Video Concatenator (모든 씬 완성 후 마지막에 사용) | - |

### Weavy 포트 색상 규칙

| 포트 색상 | 데이터 타입 | 연결 가능 대상 |
|-----------|------------|---------------|
| 녹색 | 이미지/비디오 | 같은 녹색 포트끼리만 |
| 빨간색 | 텍스트/프롬프트 | 같은 빨간색 포트끼리만 |
| 보라색 | 모델/파라미터 | 같은 보라색 포트끼리만 |
