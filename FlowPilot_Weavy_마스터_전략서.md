# FlowPilot x Weavy.ai 마스터 전략서
## AI 콘텐츠 에이전시 실전 가이드 + 비즈니스 모델 + 타겟 페르소나
### FlowPilot | 2026.02.28 | 통합 최종본

---

# CHAPTER 0. 이 사업이 뭔지 — 쉽게 설명

---

## 0-1. "이게 SMMA야? 뭐야?"

결론부터 말하면, **SMMA(Social Media Marketing Agency)가 아니다**. 비슷해 보이지만 본질이 다르다.

SMMA는 "남의 인스타/페북을 대신 운영해주는 대행사"다. 카피 쓰고, 일정 잡고, 댓글 관리하고, 광고 세팅해주는 것이 핵심이다. 진입 장벽이 낮아서 경쟁이 치열하고, 객단가가 월 50~150만원 수준에 머무른다. 누구나 할 수 있으니까 가격 경쟁에 빠진다.

FlowPilot이 하려는 건 **"AI Creative Production Agency"** — 즉, **AI 콘텐츠 제작 에이전시**다.

차이를 비유하면 이렇다:

- SMMA = **배달 대행 (배민 라이더)**. 음식(콘텐츠)을 만드는 게 아니라, 만들어진 걸 배달(포스팅)해주는 것.
- FlowPilot = **고급 레스토랑 (셰프)**. 음식(콘텐츠) 자체를 처음부터 만들어낸다. 배달도 하지만, 핵심 가치는 "요리 실력" 자체에 있다.

좀 더 정확하게 정의하면:

| 구분 | SMMA | FlowPilot (AI Creative Production Agency) |
|------|------|------------------------------------------|
| 핵심 업무 | SNS 운영 대행 (포스팅, 광고 세팅, 커뮤니티 관리) | AI로 광고 이미지/영상/UGC 콘텐츠를 "제작"하는 것 |
| 진입 장벽 | 낮음 (노트북 하나면 시작) | 높음 (Weavy + n8n + Antigravity 기술 스택 필요) |
| 객단가 | 월 50~150만원 | 월 80~500만원 + 초기 구축비 200~2,000만원 |
| 경쟁 구조 | 레드오션 (수만 개 에이전시) | 블루오션 (AI 제작 기술 보유자 극소수) |
| 스케일링 | 사람 수에 비례 (팀원 추가 = 비용 증가) | AI 자동화에 비례 (워크플로우 복제 = 비용 거의 0) |
| 대체 가능성 | 높음 (다른 SMMA로 바로 교체 가능) | 낮음 (기술 장벽 때문에 이탈 비용 높음) |

FlowPilot의 사업을 한 문장으로 요약하면:

> **"AI 기술(Weavy + n8n + Antigravity)로 브랜드의 광고 콘텐츠(이미지, 비디오, UGC)를 제작하고, 자동화하고, 배포까지 해주는 풀스택 AI 콘텐츠 프로덕션 에이전시"**

여기서 사업이 진화하면 4가지 레벨이 존재한다:

```
레벨 1: AI 콘텐츠 에이전시 (외주 제작) — 당장 돈 버는 곳
레벨 2: AI 콘텐츠 교육/컨설팅 (지식 판매) — 수동 소득 파이프
레벨 3: AI UGC 전문 플랫폼 (SaaS) — 스케일의 시작
레벨 4: AI 콘텐츠 자동 생성 플랫폼 (SaaS) — 진짜 기업 가치
```

## 0-2. 왜 이 사업이 지금 되는가?

2024~2025년까지는 AI 이미지/비디오 품질이 "신기한 장난감" 수준이었다. 2026년 지금은 다르다.

- Flux 2 Pro: 실제 촬영 사진과 구분 불가능한 포토리얼리즘
- Kling 3: 네이티브 4K/60fps 비디오를 AI가 생성
- Omnihuman V1.5: 립싱크가 실제 사람과 구분 안 됨
- Weavy: 이 모든 모델을 하나의 캔버스에서 노드로 연결

"AI로 만든 티가 난다"라는 말은 이제 2025년 얘기다. 2026년의 AI 콘텐츠는 **프로 촬영팀이 만든 것과 동급**이다. 이게 핵심이다. 품질이 상업적 기준을 넘어섰기 때문에, 이제 "파는 것"이 가능해졌다.

## 0-3. 수익이 어디서 나오는가? (쉬운 버전)

```
고객: "우리 브랜드 제품 사진이랑 광고 영상 좀 만들어주세요"

FlowPilot:
1. 제품 실물 사진을 받는다 (카톡이든 이메일이든)
2. Weavy에서 AI로 배경 교체 + 라이프스타일 이미지 + 영상 + UGC 만든다
3. 고객에게 전달한다

비용 구조:
- 고객한테 받는 돈: 월 80~500만원
- FlowPilot이 쓰는 돈: Weavy 구독 $36 (약 5만원) + 내 시간
- 마진: 95%+
```

기존에 이 작업을 하려면 카메라맨, 모델, 스튜디오, 조명, 편집자가 필요했고 최소 수백만원이 들었다. FlowPilot은 이걸 Weavy 노드 몇 개로 대체한다. 이 비용 차이가 고객에게는 "96% 비용 절감", FlowPilot에게는 "95% 마진"이 되는 구조다.

---

# CHAPTER 1. Weavy.ai 플랫폼 핵심 정리

---

## 1-1. Weavy란?

Weavy.ai는 **노드 기반(Node-based) AI 크리에이티브 워크플로우 플랫폼**이다. 하나의 캔버스 위에서 이미지 생성, 비디오 생성, 3D 생성, LLM 텍스트 처리, 전문 편집 도구(마스킹, 합성, 색보정, 리라이팅 등)를 **모두 연결하여 재사용 가능한 파이프라인**으로 구축할 수 있다.

핵심 가치: 여러 개의 AI 구독(Midjourney, Runway, DALL-E 등)을 쓰는 대신, **하나의 플랫폼에서 100개 이상의 AI 모델 + 프로 편집 도구**를 통합 사용.

2025년 10월 **Figma가 Weavy를 인수**했으며, "Figma Weave"로 통합 예정이지만 현재 독립 제품으로 운영 중이다.

## 1-2. 가격 체계

| 플랜 | 월 요금 | 월 크레딧 | 핵심 특징 |
|------|---------|-----------|-----------|
| **Free** | $0 | 150 | 모든 모델 접근 가능, 워크플로우 5개 제한 |
| **Starter** | $19 | 1,500 | 약 3,750장 이미지 또는 417초 비디오, 상업적 라이선스 |
| **Professional** | $36 | 크레딧 롤오버(최대 3배 누적), 추가 크레딧 $10/1,200 |
| **Team** | $48/유저 | 4,500 | 팀 권한 관리, App Mode 포함 |
| **Enterprise** | 맞춤 | 맞춤 | 전용 지원, 맞춤 통합 |

## 1-3. 핵심 개념: 노드 시스템 + 콘텐츠 타입 호환성

Weavy는 n8n과 유사한 노드 기반 캔버스다. 각 노드는 하나의 기능을 담당하고, 선으로 연결하여 워크플로우를 만든다.

**중요 규칙: 콘텐츠 타입이 같아야 연결된다.**

- File(이미지) 노드와 Prompt(텍스트) 노드는 서로 직접 연결 불가
- File(이미지)과 Prompt(텍스트)는 각각 병렬로 이미지 생성 모델(Flux 등)에 연결
- File → 모델의 Image 입력 포트, Prompt → 모델의 Text 입력 포트

## 1-4. 기본 워크플로우 구축 순서

1단계: 캔버스에 **Prompt Node** 추가 → 원하는 장면/이미지 설명 입력
2단계: **Router Node** 연결 → 하나의 프롬프트를 여러 모델에 동시 전송
3단계: 각 분기에 **Image Gen Node** 또는 **Video Gen Node** 추가 → 모델 선택
4단계: 결과에 **Edit Node** 연결 → 색보정, 마스킹, 업스케일 등 후처리
5단계: **Export Node**로 최종 출력

크레딧 절약 전략: 저렴한 모델(Flux Schnell, SD 3.5 등)로 먼저 프롬프트와 구도를 테스트 → 확정 후 해당 노드만 고급 모델(Flux Pro, Veo 3 등)로 교체하여 최종 렌더링

---

# CHAPTER 2. AI 모델 최종 랭킹 (2026년 2월 기준)

---

## 2-1. 비디오 생성 모델 종합 랭킹

| 순위 | 모델 | ELO 점수 | 핵심 강점 | FlowPilot 용도 |
|------|------|---------|-----------|---------------|
| 1위 | **Runway Gen-4.5** | 1,247 | 모션 퀄리티, 프롬프트 충실도, 시각적 완성도 종합 1위 | 프리미엄 광고, 최종 시안 |
| 2위 | **Veo 3.1 (Google)** | - | 프롬프트 준수율 1위, 4K + 공간 오디오, 방송급 품질 | 시네마틱 브랜드 필름, 4K 콘텐츠 |
| 3위 | **Seedance 2.0 (ByteDance)** | - | 오디오+비디오 동시 생성, 2K, 멀티샷, 90%+ 사용 가능 출력률 | 음악 싱크 영상 |
| 4위 | **Kling 3.0** | - | 네이티브 4K/60fps, 다국어 립싱크, 무료 티어 최강 | **대량 숏폼 생산 (가성비 최강)** |
| 5위 | **Sora 2 (OpenAI)** | - | 25초 네이티브 클립 (경쟁사 대비 2배), 스토리보드 편집 | 긴 형태의 콘텐츠 |
| 6위 | **Higgsfield** | - | 시네마틱 카메라 컨트롤 정밀도 최고, 숏폼 특화 | 카메라 앵글 정밀 제어 |
| 7위 | **Luma Ray 3** | - | 포스트 프로덕션 워크플로우, 3D 씬 이해 | 후처리 |
| 8위 | **Wan 2.5 / 2.2 Animate** | - | 모션 전이(이미지→비디오) 특화, 스타일라이즈 강점 | 이미지에 모션 입히기 |
| 9위 | **LTX 2 Video** | - | 경량/빠른 생성, 프로토타이핑용 | 빠른 테스트 |

### 용도별 Best Pick (비디오)

| 용도 | 1순위 | 이유 |
|------|-------|------|
| 종합 퀄리티 최강 | Runway Gen-4.5 | ELO 전체 1위 |
| 방송급/4K | Veo 3.1 | 4K + 공간 오디오 |
| 가성비/대량 생산 | Kling 3.0 | 네이티브 4K/60fps, 무료 티어 |
| 오디오 싱크 | Seedance 2.0 | 세계 유일 오디오-비디오 Joint 아키텍처 |
| 카메라 앵글 정밀 | Higgsfield | 돌리 줌, 랙 포커스 등 |
| 긴 영상 | Sora 2 | 25초 네이티브 클립 |
| 모션 전이 | Wan 2.2 Animate | 스틸 이미지에 동작 입히기 |

## 2-2. 이미지 생성 모델 종합 랭킹

| 순위 | 모델 | ELO | 핵심 강점 | FlowPilot 용도 |
|------|------|-----|-----------|---------------|
| 1위 | **Recraft V4** | 1,172 | 로고/벡터/SVG 1위. 브랜드 에셋 특화 | 로고, 벡터, 브랜드 에셋 |
| 2위 | **Flux 2 (Black Forest Labs)** | 1,143 | 포토리얼리즘 1위. 피부 텍스처, 조명 최강 | **제품 사진, 인물, 광고 소재** |
| 3위 | **Ideogram 3.0** | 1,102 | 텍스트 렌더링 1위. 배너/포스터 특화 | 텍스트 포함 배너, 포스터 |
| 4위 | **Midjourney V7** | - | 아트/미학적 아름다움 1위 | 아트 컨셉 |
| 5위 | **GPT Image (DALL-E 4)** | - | 텍스트+브랜드 그래픽 강점 | 종합 텍스트 이미지 |

### 용도별 Best Pick (이미지)

| 용도 | 1순위 | 이유 |
|------|-------|------|
| 포토리얼 인물/제품 | Flux 2 Pro | 사진급 품질 최고 |
| 로고/SVG/벡터 | Recraft V4 | 벡터 네이티브 |
| 텍스트 포함 이미지 | Ideogram V3 | 텍스트 정확도 압도적 |
| 빠른 테스트 | Flux Schnell/Fast | 크레딧 최소 소모 |

## 2-3. 립싱크 모델 랭킹

| 순위 | 모델 | 최적 용도 | 강점 |
|------|------|-----------|------|
| 1위 | **Omnihuman V1.5** | AI UGC 리뷰 영상 | 입+표정+고개 움직임까지 자연스러움 |
| 2위 | **Sync 2 Pro** | 프로급 립싱크 | 높은 정확도, 다국어 지원 |
| 3위 | **Kling AI Avatar** | Kling 연계 | Kling 비디오와 최적 연동 |

## 2-4. 편집/유틸리티 핵심 도구

| 카테고리 | 1순위 도구 | 핵심 용도 |
|----------|-----------|-----------|
| 배경 교체 | **Replace Background** | 원클릭 배경 교체, 제품 100% 보존 |
| 제품 추출 | **Segment Anything** | 제품만 깔끔하게 잘라냄 (투명 배경) |
| 합성 | **Composite (Simple Blend)** | 제품(전경) + 배경 합치기 |
| 인페인팅 | **Flux 2 Inpaint** | 특정 영역만 수정 |
| 업스케일 (이미지) | **Topaz Image Upscale** | 원본에 가장 충실한 복원 |
| 업스케일 (비디오) | **Topaz Video Upscaler** | 가장 안정적 비디오 업스케일 |
| 조명 보정 | **Relight 2.0** | 촬영 후 조명 완전 재설정 |
| 색보정 | **Color Grading** | 브랜드 톤 통일 |
| 대량 생산 | **Text Iterator / Image Iterator** | 프롬프트/이미지 리스트 자동 반복 처리 |
| 분기 | **Router** | 하나의 입력 → 여러 출력 (A/B 테스트) |

## 2-5. CTO 인사이트: "조합"이 답이다

2026년 2월 현재, 모든 용도에서 1위인 모델은 **존재하지 않는다**. 이게 Weavy 같은 멀티모델 플랫폼이 뜨는 이유이고, FlowPilot이 팔 수 있는 가치의 핵심이다.

"어떤 모델이 최고냐"가 아니라 "이 특정 샷에 어떤 모델이 최적이냐"가 정답이고, 그걸 설계하고 자동화하는 것이 FlowPilot의 기술 장벽이다.

경쟁사가 "저희는 Runway 씁니다" 하면, FlowPilot은 "저희는 샷마다 최적 모델을 자동 라우팅합니다"로 이긴다.

---

# CHAPTER 3. 실전 워크플로우 — 16:9 브랜드 광고 숏폼 제작

---

## 3-1. 핵심 원칙: "AI가 제품을 절대 건드리게 하지 마라"

이전 작업에서 배운 교훈: Flux Image-to-Image로 제품을 직접 변환하면 뚜껑/노즐/레이블이 변형된다. AI는 제품 디자인을 "해석"해서 자기 맘대로 바꿔버린다.

해결법: **원본 제품은 100% 보존하고, 배경만 AI로 생성한 뒤 합성한다.**

```
원본 제품 → Segment Anything (추출) ──────────────────→ Composite (전경)
                                                            ↓
프롬프트 → Flux 2 Pro (배경만 생성, "NO bottle" 필수) ──→ Composite (배경)
                                                            ↓
                                                       합성 완료 → 비디오 변환
```

## 3-2. 5단계 프로세스

```
[Phase 1]           [Phase 2]          [Phase 3]          [Phase 4]         [Phase 5]
제품 이미지 준비 → 배경 이미지 생성 → 제품+배경 합성 → 비디오 변환 → 최종 출력
(Segment Anything) (Flux 2 Pro)      (Composite)       (Kling 3/Veo)     (Export)
```

## 3-3. Phase 1 — 제품 추출

**Step 1.** + 버튼 → **"Segment Anything"** 검색 → 추가

**Step 2.** File 노드(제품 이미지) 출력 → Segment Anything 입력에 연결

**Step 3.** Run → 투명 배경의 제품 이미지가 나온다. 뚜껑, 레이블, 노즐 전부 원본 그대로 100% 유지.

## 3-4. Phase 2 — 배경 이미지 생성 (3가지 시안)

### 시안 A: "대리석 골든아워" (럭셔리 감성)

+ 버튼 → **Prompt** 노드 추가. 아래 내용 입력:
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

+ 버튼 → **Flux 2 Pro** 추가. Prompt 출력 → Flux의 Prompt 입력에 연결.
- Image Size 파라미터: **Landscape 16 9** 선택

### 시안 B: "편백 숲 새벽안개" (자연/힐링 감성)

+ 버튼 → **Prompt** 노드 추가:
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

+ 버튼 → **Prompt** 노드 추가:
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

## 3-5. Phase 3 — 제품 + 배경 합성

각 Flux 배경 결과물에 원본 제품을 합성한다.

**Step 1.** + 버튼 → **"Composite"** 검색 → **Composite (Simple Blend)** 추가

**Step 2.** 연결:
- Flux 2 Pro 출력 (배경 이미지) → Composite **Background** 입력
- Segment Anything 출력 (잘린 제품) → Composite **Foreground** 입력

**Step 3.** Run → 합성 결과 확인

시안 3개이므로 **Composite 노드 3개 필요** (각 배경마다 1개). Segment Anything 출력은 3개 Composite 모두에 공통 연결.

## 3-6. Phase 4 — 비디오 변환 (16:9 숏폼 광고)

+ 버튼 → **Kling 3** 추가 (가성비 기본)

**연결 3개 필요:**
1. Composite 출력 (합성 이미지) → Kling 3 **Image** 입력
2. 새 Prompt 노드 → Kling 3 **Prompt** 입력
3. 새 Prompt 노드 → Kling 3 **Negative Prompt** 입력

**시안 A 비디오 Prompt (대리석):**
```
Slow elegant camera push-in towards the product bottle on marble surface, golden sunlight
gradually intensifies creating moving light patterns on the marble, sheer curtains billow
gently in soft breeze, fine dust particles drift through the warm light beam, the shadow
of the bottle slowly elongates as light shifts, camera movement: smooth dolly forward at
0.2x speed with subtle 3-degree downward tilt, shallow depth of field rack focus from
background curtains to bottle, warm cinematic color grading, luxury perfume commercial
pacing, serene and sophisticated atmosphere
```

**시안 B 비디오 Prompt (편백숲):**
```
Atmospheric slow camera drift forward towards the product bottle on wood table in misty
forest, fog rolls gently between cypress trees in background creating depth movement,
morning dew droplets on wood surface catch shifting light as sun rises, a single leaf
slowly falls from above passing through frame, subtle breeze moves the cypress sprig,
camera movement: slow creeping dolly at 0.15x speed, volumetric god rays shift angle
slightly, cool to warm color temperature transition, nature documentary meets luxury
commercial aesthetic, ASMR peaceful atmosphere
```

**시안 C 비디오 Prompt (스튜디오):**
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

**프리미엄 옵션:** Kling 3 대신 **Veo 3.1** (4K 시네마틱) 또는 **Runway Gen-4.5** (ELO 1위, 최고 퀄리티) 사용 가능. 동일 연결 구조.

## 3-7. Phase 5 — 최종 출력

+ 버튼 → **Export** 노드 추가 → 비디오 모델 출력 연결
- Format: MP4
- Resolution: 1920x1080 (16:9)

## 3-8. 캔버스 전체 구성도

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

# CHAPTER 4. 실전 워크플로우 — AI UGC 콘텐츠 제작

---

## 4-1. AI UGC란?

AI가 생성한 **진짜 사람처럼 보이는 마케팅 콘텐츠**. 실제 인플루언서/모델 없이 AI 아바타가 제품을 리뷰하고, 추천하고, 언박싱하는 영상을 만든다. 2026년 현재 기존 광고 대비 CTR 400% 높고, 전환율 29% 향상이라는 데이터가 나오고 있다.

기존 UGC 비용: 영상 1개당 50~200만원 (인플루언서 섭외, 촬영, 편집)
AI UGC 비용: 영상 1개당 1~5만원 (Weavy 크레딧만)

## 4-2. 5단계 프로세스

```
[Phase 1]           [Phase 2]          [Phase 3]         [Phase 4]        [Phase 5]
AI 인물+제품       비디오 변환       립싱크 추가       음성/BGM 합성    최종 출력
이미지 생성        (Kling 3)         (Omnihuman)       (Merge Audio)    (Export)
(Flux 2 Pro)
```

## 4-3. Phase 1 — AI 인물 이미지 생성

### 방법 1: 텍스트에서 새 AI 인물 생성

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

### 방법 2: 특정 모델 이미지를 활용 (이전에 제공한 모델 사용)

이미 확보한 모델 이미지(세이지 그린 제품을 들고 있는 내추럴 룩 여성)가 있다면 3가지 방법이 있다:

**A. 직접 Image-to-Video (Kling 3):** 모델 이미지를 File 노드로 업로드 → Kling 3의 Image 입력에 바로 연결. 가장 간단하지만 장면 변형이 제한됨.

**B. ID Preservation (Flux):** + 버튼 → Community Models에서 **ID Preservation - Flux** 추가. 모델 이미지를 Reference Face 입력에 연결하고, 새 장면 프롬프트를 Text 입력에 연결. 얼굴/피부톤/헤어는 원본 유지하면서 장면/의상/배경만 변경 가능.

**C. Face Swap (대량 생산):** 먼저 Flux로 다양한 AI 인물 이미지를 대량 생성 → Community Models에서 **Face Swap** 추가 → AI 인물의 얼굴을 모델 얼굴로 교체. 수십 장을 빠르게 만들 때 최적.

## 4-4. Phase 2~5 — 비디오 변환, 립싱크, 오디오 합성, 출력

**Phase 2:** Flux 출력 → **Kling 3** Image 입력 + Motion Prompt 연결

**Kling 3 Prompt (시나리오 1 예시):**
```
The woman gently lifts the KINOHI body mist bottle, sprays once on her left wrist with a
natural flick motion, brings her wrist to her nose and inhales with eyes closing softly,
a subtle satisfied smile spreads across her face, she opens her eyes and looks at camera
nodding slightly as if to say this is good, natural head micro-movements throughout,
camera stays static on tripod, warm morning light, authentic vlogger review moment,
5 second duration
```

**Phase 3:** Kling 출력 → **Omnihuman V1.5** (Lip Sync 카테고리) 연결. 한국어 음성 파일(ElevenLabs TTS 등으로 생성)을 Audio 입력에 연결.

리뷰 스크립트 예시: "요즘 제가 매일 쓰는 바디미스트인데요, 키노히 산탈이에요. 향이 진짜 은은하게 지속되면서 편백 베이스라서 엄청 편안해요. 출근 전에 한 번 뿌리면 점심때까지 은은하게 남아있거든요. 강추합니다."

**Phase 4:** Omnihuman 출력 + BGM 오디오 → **Merge Audio and Video** (Community Models)

**Phase 5:** → **Export** → MP4, 1920x1080

---

# CHAPTER 5. 프롬프트 마스터 가이드

---

## 5-1. 브랜드 톤 키워드 (KINOHI 기준 — 모든 프롬프트 끝에 추가)

```
minimal and serene atmosphere, cream and white color palette with subtle sage green
accents, generous negative space, soft natural lighting, premium editorial quality,
Kinfolk magazine aesthetic
```

## 5-2. 절대 쓰면 안 되는 키워드

- neon, vibrant, bold colors, busy background, cluttered
- HDR, oversaturated, dramatic contrast
- plastic texture, glossy, shiny

## 5-3. 네거티브 프롬프트 (이미지 노드용)

```
text overlay, watermark, logo text, blurry, low quality, oversaturated colors,
neon lighting, cluttered background, plastic looking skin, artificial, stock photo feel,
multiple bottles, distorted proportions, extra fingers, deformed hands
```

## 5-4. 네거티브 프롬프트 (비디오 노드용)

```
fast motion, shaky camera, blurry, distorted bottle shape, melting product, morphing
bottle design, cap changing shape, nozzle deforming, label text distortion, extra objects
appearing, text overlay, watermark, low quality, oversaturated colors, cartoon style,
anime, illustration, painting style
```

## 5-5. UGC 프롬프트 핵심 공식

UGC의 생명은 "진짜 사람 같은 자연스러움"이다. AI가 만드는 완벽한 모델 사진은 UGC가 아니다. 반드시 다음을 포함:

```
NOT perfect NOT airbrushed NOT glamorous, real person aesthetic,
visible natural skin texture including subtle pores and tiny moles for authenticity
```

## 5-6. 프롬프트 작성 공식

```
[주체 묘사] + [행동/포즈] + [공간/배경 묘사] + [조명 지시] + [카메라/렌즈 지시] +
[색감/무드 지시] + [브랜드 톤 키워드] + [금지 키워드 (NO bottle 등)]
```

---

# CHAPTER 6. 타겟 페르소나 정의 — 누구에게 팔 것인가

---

## 6-1. Primary Persona: "마케팅 팀장 지은" (B2B 핵심 타겟)

| 항목 | 내용 |
|------|------|
| 이름 | 김지은 (가상) |
| 나이 | 32세 |
| 직함 | 중소 뷰티/라이프스타일 브랜드 마케팅 팀장 |
| 회사 규모 | 직원 15~50명, 연 매출 30~100억 |
| 월 콘텐츠 예산 | 300~800만원 |
| Pain Point | 매달 신제품 출시마다 촬영 스튜디오 예약 → 모델 섭외 → 촬영 → 보정 → 영상 편집에 2~3주 소요. 비용은 건당 300만원+. SNS 콘텐츠는 매일 올려야 하는데 생산 속도가 못 따라감. 숏폼 트렌드에 대응하고 싶지만 영상 제작 인력이 없음. |
| 원하는 것 | "제품 사진 보내면 다음 날 광고 소재 10종이 나왔으면 좋겠다" |
| 결정 기준 | ROI 수치, 포트폴리오 퀄리티, 빠른 납기 |
| 어디에 있나 | 인스타그램(경쟁사 벤치마킹), 링크드인, 마케팅 커뮤니티(DMK, 그로스해킹 커뮤니티) |
| 세일즈 훅 | "현재 월 500만원 쓰는 촬영 비용을, AI로 월 80만원에 해결하면서 콘텐츠 양은 3배로 늘릴 수 있습니다" |

## 6-2. Secondary Persona: "1인 셀러 민수" (B2C/교육 타겟)

| 항목 | 내용 |
|------|------|
| 이름 | 박민수 (가상) |
| 나이 | 28세 |
| 직함 | 스마트스토어/쿠팡 1인 셀러 |
| 월 매출 | 500~3,000만원 |
| Pain Point | 제품 상세페이지 이미지가 후져서 전환율이 안 나옴. 촬영 맡기면 최소 50만원인데, 마진이 얇아서 그 돈 쓰기 아까움. 경쟁 셀러는 이미 AI 이미지 쓰는 것 같은데 어떻게 하는지 모름. |
| 원하는 것 | "내가 직접 AI로 제품 사진 만들 수 있게 해줘" |
| 결정 기준 | 가격 (저렴할수록), 난이도 (쉬울수록), 따라하기 쉬운 튜토리얼 |
| 어디에 있나 | 유튜브(셀러 튜토리얼), 네이버 카페(셀러 커뮤니티), 클래스101 |
| 세일즈 훅 | "월 36달러 Weavy 구독 하나로 제품 사진 무한 생성. 이 강의에서 A to Z 알려드립니다" |

## 6-3. Tertiary Persona: "에이전시 대표 승환" (파트너/업셀 타겟)

| 항목 | 내용 |
|------|------|
| 이름 | 이승환 (가상) |
| 나이 | 36세 |
| 직함 | 소형 마케팅 에이전시 대표 (직원 3~8명) |
| Pain Point | 클라이언트는 숏폼 콘텐츠를 요구하는데, 영상 제작 인력이 부족. 외주 편집자 비용이 올라감. 경쟁 에이전시가 AI 도입하면서 가격 경쟁이 시작됨. |
| 원하는 것 | "우리 에이전시에 AI 콘텐츠 제작 역량을 추가하고 싶다" |
| 결정 기준 | 기존 워크플로우에 통합 가능성, 팀원 교육 난이도, 클라이언트에 보여줄 퀄리티 |
| 어디에 있나 | 링크드인, 에이전시 네트워킹 행사, 마케팅 컨퍼런스 |
| 세일즈 훅 | "FlowPilot이 구축해드리는 AI 파이프라인으로, 귀사의 숏폼 제작 단가를 80% 낮추고 납기를 일주일에서 하루로 줄일 수 있습니다" |

## 6-4. Dream Persona: "대기업 브랜드 매니저" (엔터프라이즈 타겟)

| 항목 | 내용 |
|------|------|
| 이름 | 최서연 (가상) |
| 나이 | 38세 |
| 직함 | 대기업(아모레퍼시픽, LG생활건강 등) 브랜드 매니저 |
| Pain Point | 글로벌 론칭 시 10개국 로컬라이즈 콘텐츠 필요. 각 국가별 모델+촬영+편집 비용 엄청남. AI 도입을 상부에 제안하고 싶지만 "품질이 될까?" 확신이 없음. |
| 원하는 것 | "검증된 AI 콘텐츠 파트너가 파일럿 프로젝트를 해줬으면" |
| 세일즈 훅 | "FlowPilot이 귀사 제품으로 AI 파일럿 영상 3편을 무료로 만들어드립니다. 실제 광고 퍼포먼스를 비교해보세요" |
| 예상 객단가 | 초기 2,000만원+ / 월 MRR 500만원 |

---

# CHAPTER 7. 비즈니스 모델 + 수익 구조

---

## 7-1. 4층 피라미드 구조

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

## 7-2. 1층: 포트폴리오 콘텐츠 마케팅 (리드 엔진)

무엇: 실제 브랜드(키노히 등)로 AI 콘텐츠를 만들어 SNS에 공개. "이 영상 AI로 만들었습니다" 후킹으로 바이럴.

구체적 액션:
- 인스타/틱톡에 주 3회 AI 제품 광고 영상 포스팅
- "Before(원본 제품사진) → After(AI 광고)" 비교 릴스
- 유튜브에 "Weavy로 5분 만에 브랜드 광고 만드는 법" 튜토리얼
- FlowPilot 홈페이지에 케이스 스터디 게시

KPI: 월 50건 이상 인바운드 문의 유입

## 7-3. 2층: 교육/컨설팅

### 사업 A: "AI 콘텐츠 마스터클래스"

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

## 7-4. 3층: AI 콘텐츠 에이전시 (핵심 매출)

### 서비스 패키지

| 패키지 | 내용 | 가격 | MRR |
|--------|------|------|-----|
| **Starter** | 제품 이미지 20장 + 숏폼 4편/월 | 초기 200만원 + 월 80만원 | 80만원 |
| **Growth** | 이미지 40장 + 숏폼 8편 + UGC 4편/월 | 초기 400만원 + 월 150만원 | 150만원 |
| **Premium** | 이미지 무제한 + 숏폼 16편 + UGC 8편 + 브랜드 필름 2편/월 | 초기 800만원 + 월 300만원 | 300만원 |
| **Enterprise** | 풀스택 (n8n 자동화 + 자동 배포 + 성과 분석) | 초기 2,000만원+ + 월 500만원 | 500만원 |

### 업셀(Upsell) 전략

**레벨 1 → 2:** "Weavy 워크플로우만" → "n8n 연동으로 트리거 자동화" (예: 구글시트에 제품 등록 → 자동 이미지 생성 → 쇼핑몰 업로드)

**레벨 2 → 3:** "n8n + Weavy 자동화" → "Antigravity 프론트엔드 대시보드 구축" (고객이 직접 관리하는 AI 콘텐츠 포털)

**크로스셀:** 콘텐츠 생성 고객에게 → "생성된 콘텐츠 자동 배포" (소셜 미디어 자동 포스팅, 이메일 마케팅 연동) 추가 제안

### 월 매출 시뮬레이션

| 고객 수 | 패키지 | 월 MRR |
|---------|--------|--------|
| Starter x 5 | 80만원 x 5 | 400만원 |
| Growth x 3 | 150만원 x 3 | 450만원 |
| Premium x 2 | 300만원 x 2 | 600만원 |
| Enterprise x 1 | 500만원 x 1 | 500만원 |
| **합계 11개 고객** | | **월 1,950만원 MRR** |

## 7-5. 4층: SaaS 플랫폼 (Moonshot)

### 사업 C: "BrandForge" — AI 브랜드 콘텐츠 자동 생성 플랫폼

| 항목 | 내용 |
|------|------|
| 컨셉 | 비기술 사업자가 제품 사진만 업로드하면, AI가 자동으로 광고 이미지 10장 + 숏폼 3편 + UGC 2편 생성 |
| 기술 스택 | Antigravity (프론트엔드) + n8n (백엔드 오케스트레이션) + Weavy API (AI 생성 엔진) |
| 가격 | Free (월 5콘텐츠) / Pro 월 9.9만원 (월 50콘텐츠) / Business 월 29.9만원 (무제한) |
| 타겟 | 1인 셀러, 소상공인, 스타트업 마케터 |
| 차별점 | 에이전시 대비 90% 저렴, 즉시 생성, 셀프서비스 |

### 사업 D: "UGC Factory" — 한국형 AI UGC 전문 플랫폼

| 항목 | 내용 |
|------|------|
| 컨셉 | 브랜드가 제품 정보만 입력하면, 한국인 AI 아바타가 리뷰/언박싱/사용 영상을 자동 생성 |
| 차별점 | MakeUGC, Arcads 같은 해외 플랫폼의 한국 로컬라이즈 버전. 한국인 AI 아바타, 한국어 자연스러운 립싱크 |
| 기술 스택 | Flux (인물) + Kling (비디오) + Omnihuman (립싱크) + ElevenLabs (한국어 TTS) + n8n (파이프라인) |
| 가격 | 영상 1편당 5만원 또는 월 구독 39.9만원 (20편/월) |

### 사업 E: "AI 촬영 스튜디오" — 오프라인+AI 하이브리드

| 항목 | 내용 |
|------|------|
| 컨셉 | 오프라인 스튜디오에서 제품 실물 촬영 (기본 컷 3~5장) → AI로 100장+ 변형 생성 |
| 차별점 | 실제 촬영의 정확성 + AI의 무한 확장. "AI만 쓰면 디테일이 안 나온다"는 고객 불안 해소 |
| 가격 | 촬영 패키지 50만원 (실촬영 5컷 + AI 변형 50컷 + 숏폼 5편) |

---

# CHAPTER 8. 세일즈 퍼널 + 리드 발굴 전략

---

## 8-1. 세일즈 퍼널 5단계

**1단계 - 인지 (Awareness):** "AI로 제품 사진 5분 만에 만든 결과" Before/After 숏폼 콘텐츠를 인스타, 유튜브 숏츠, 틱톡에 배포. Weavy 워크플로우 시연 영상이 곧 리드 마그넷.

**2단계 - 유입 (Attraction):** "무료 AI 콘텐츠 워크플로우 템플릿 5종 받기" 랜딩페이지. 이메일 수집 후 Weavy 워크플로우 JSON + 사용법 가이드 제공.

**3단계 - 신뢰 구축 (Trust):** 웨비나/라이브에서 실시간 Weavy 워크플로우 시연. "이 워크플로우를 우리가 여러분 비즈니스에 맞춤 구축해드립니다."

**4단계 - 세일즈 콜 (Conversion):** 고객 비즈니스 분석 → 맞춤 워크플로우 데모 → ROI 수치화 제안

**5단계 - 결제 (Close):** 초기 구축비 + 월 유지보수(MRR) 구조

## 8-2. 타겟 산업별 공략법

| 산업 | Pain Point | 세일즈 훅 | 예상 객단가 |
|------|------------|-----------|-----------|
| **뷰티/화장품** | 신제품마다 촬영 비용 500만원+ | "촬영 없이 AI로 무한 생성, 96% 절감" | 월 150~300만원 |
| **패션/의류** | 시즌마다 모델+스튜디오 | "AI 가상 모델 + 배경 교체" | 월 150~300만원 |
| **F&B/식음료** | 메뉴 사진 업데이트 | "AI 메뉴 비주얼 자동 생성" | 월 80~150만원 |
| **부동산** | 가상 스테이징 비용 | "빈 집 → AI 인테리어 자동" | 월 80~150만원 |
| **이커머스 셀러** | 상세페이지 이미지 대량 필요 | "AI 제품 컷 대량 생산" | 월 80만원 |
| **마케팅 에이전시** | 영상 제작 인력 부족 | "AI 파이프라인으로 납기 1/5 축소" | 월 150~500만원 |

## 8-3. 리드 발굴 구체적 액션 플랜

**온라인:**
- 인스타그램/틱톡: 주 3회 AI 콘텐츠 포스팅 + "#AI콘텐츠" "#AImarketinng" 해시태그
- 유튜브: 주 1회 Weavy 튜토리얼 + 결과물 비교 영상
- 링크드인: 마케팅 팀장 타겟 DM + AI 콘텐츠 사례 공유
- 네이버 카페/블로그: 이커머스 셀러 커뮤니티에 AI 제품 사진 사례 공유
- 클래스101/탈잉: 온라인 강의 등록 → 수강생 중 에이전시 고객 전환

**오프라인:**
- 마케팅 컨퍼런스/밋업 참석 (DMK, 그로스해킹 등)
- 뷰티/패션 브랜드 네트워킹 이벤트
- 무료 세미나 "AI로 광고 콘텐츠 만들기" 개최

**유료 광고:**
- 메타 광고: Before/After 영상 소재, 타겟 = 마케팅 매니저/브랜드 매니저/이커머스 셀러
- 구글 광고: "AI 제품 사진", "AI 광고 영상 제작" 키워드
- A/B 테스트: 소재별(영상 vs 이미지), 카피별, 타겟별 CPL 최적화

---

# CHAPTER 9. 비용 분석 + ROI

---

## 9-1. FlowPilot 운영 비용 (월간)

| 항목 | 비용 |
|------|------|
| Weavy Professional | $36 (약 5만원) |
| n8n Cloud | $20 (약 3만원) |
| 추가 크레딧 (필요 시) | $10~30 (약 1.5~4.5만원) |
| 도메인/호스팅 | 약 2만원 |
| **월 고정 비용** | **약 11~15만원** |

## 9-2. 고객 제공 기준 기존 vs AI 비용 비교

| 항목 | 기존 (전통 촬영) | AI (FlowPilot) | 절감률 |
|------|-----------------|-----------------|--------|
| 제품 촬영 (20장/월) | 150만원 | 약 5만원 | 97% |
| 숏폼 영상 (8편/월) | 400만원 | 약 15만원 | 96% |
| UGC 인플루언서 (4편/월) | 200만원 | 약 8만원 | 96% |
| 시네마틱 광고 (4편/월) | 500만원 | 약 20만원 | 96% |
| **월 합계** | **1,250만원** | **약 48만원** | **96%** |

이 차이가 고객에게 "월 1,200만원 절감"이라는 ROI로 제시되고, FlowPilot에게는 95% 마진이 되는 구조다.

---

# CHAPTER 10. 사업 로드맵

---

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

# CHAPTER 11. n8n + Weavy 자동화 시너지

---

## 11-1. 왜 n8n이 필요한가?

Weavy 단독은 "수동으로 버튼 눌러서 콘텐츠 생성"이다. 여기에 **n8n을 붙이면 완전 자동화**가 된다. 이것은 Weavy만 쓰는 경쟁사가 절대 흉내 낼 수 없는 FlowPilot만의 기술 장벽이다.

## 11-2. 자동화 파이프라인 예시

**콘텐츠 자동 생성 + 배포:**
```
n8n Webhook (Weavy Export 완료 트리거)
→ n8n IF Node (이미지 vs 비디오 분기)
→ [이미지] n8n HTTP Request → 인스타그램 Graph API → 자동 피드 포스팅
→ [비디오] n8n HTTP Request → TikTok Business API → 자동 릴스 포스팅
→ n8n Slack Node → #marketing 채널에 "새 콘텐츠 발행 완료" 알림
```

**월간 콘텐츠 캘린더 자동화:**
```
n8n Schedule Trigger (매주 월/수/금 오전 9시)
→ n8n Google Sheets (콘텐츠 캘린더에서 오늘 날짜 씬 정보 읽기)
→ n8n HTTP Request (Weavy API 호출 → 해당 씬 워크플로우 실행)
→ Weavy 이미지/비디오 생성
→ n8n Export 수신 → 자동 배포
```

**풀 UGC 파이프라인:**
```
n8n 트리거 (신제품 등록)
→ Weavy API: Flux 2로 제품 이미지 5종 자동 생성
→ Weavy API: Kling 3로 숏폼 비디오 변환
→ AI UGC 플랫폼 API: AI 아바타 리뷰 영상 자동 생성
→ 소셜 미디어 API: 자동 예약 포스팅
```

## 11-3. 핵심 기술 스택

| 도구 | 역할 |
|------|------|
| **Weavy** | AI 콘텐츠 생성 엔진 (눈에 보이는 가치) |
| **n8n** | 자동화 오케스트레이션 (보이지 않는 기술 장벽) |
| **Antigravity** | 프론트엔드 대시보드 (고객 접점, SaaS화) |

경쟁사가 "Weavy로 이미지 만들어드립니다"만 하면, FlowPilot은 "제품 등록하면 이미지+영상+UGC+배포까지 전부 자동"으로 이긴다. 이 3단 스택이 경쟁사가 6개월 안에 따라올 수 없는 해자(moat)다.

---

# CHAPTER 12. 브레인스토밍 — 추가 아이디어

---

## 12-1. Cash Cow (즉시 실행, 빠른 캐시)

**아이디어 1: "AI 상세페이지 패키지"**
스마트스토어/쿠팡 셀러 대상. 제품 사진 5장 보내면 → AI로 상세페이지 이미지 20장 + GIF 3개 생성. 건당 30만원, 매일 1건만 해도 월 900만원. 네이버 카페/블로그에서 셀러 직접 타겟팅.

**아이디어 2: "AI 계절 콘텐츠 정기 구독"**
뷰티/F&B 브랜드에게 분기별 계절 테마 콘텐츠 패키지 제공. 봄/여름/가을/겨울 각 테마로 제품 이미지 시리즈 자동 생성. 연간 계약으로 월 80만원 MRR 확보.

**아이디어 3: "AI 모델 대체 서비스"**
패션/뷰티 브랜드의 모델 촬영을 AI로 대체. ID Preservation + Face Swap으로 한 명의 모델 이미지에서 수십 가지 장면/의상/배경 변형. 모델+스튜디오 비용 대비 90% 절감.

## 12-2. Moonshot (고단가 혁신)

**아이디어 4: "AI 글로벌 로컬라이즈 서비스"**
하나의 광고 소재를 10개국 버전으로 자동 변형. AI 모델의 인종, 배경 문화, 텍스트 언어를 각 국가에 맞게 변환. Kling 3의 다국어 립싱크 + Flux의 인종별 모델 생성. 글로벌 브랜드 대상 건당 2,000만원+.

**아이디어 5: "AI 콘텐츠 A/B 테스트 자동화 플랫폼"**
하나의 제품 사진에서 배경/모델/색감/앵글 변형을 100가지 자동 생성 → 메타/구글 광고에 자동 배포 → 성과 데이터 수집 → 위닝 소재 자동 발견. Text Iterator + Router + n8n + Meta API. 퍼포먼스 마케팅 에이전시 대상.

**아이디어 6: "AI 브랜드 가상 앰배서더"**
브랜드 전용 AI 캐릭터를 만들어 SNS 인플루언서로 운영. ID Preservation으로 일관된 캐릭터 유지, Omnihuman으로 말하는 영상 생성, ElevenLabs로 일관된 목소리. 실제 인플루언서 없이 브랜드가 자체 AI 인플루언서를 보유하는 시대.

---

# CHAPTER 13. 세일즈 포인트 — 고객에게 이렇게 말해라

---

## 13-1. 에이전시 고객 대상 (세일즈 콜)

"현재 월 500만원 쓰는 촬영 비용을, AI로 월 80만원에 해결하면서 콘텐츠 양은 3배로 늘릴 수 있습니다. 기존에는 촬영 스케줄 잡고, 모델 섭외하고, 촬영하고, 보정하는 데 최소 2주 걸렸잖아요? 저희는 제품 사진 받으면 다음 날 이미지 20장 + 숏폼 4편 납품합니다."

## 13-2. UGC 서비스 대상

"현재 UGC 영상 하나 만드는 데 인플루언서 섭외부터 촬영, 편집까지 평균 50~200만원 쓰고 계시죠? 저희가 구축하는 AI UGC 파이프라인은 한 번 세팅하면 영상 하나당 1~3만원으로 무한 생산됩니다. 월 20개 영상 기준으로 연간 약 2,000만원 이상 절감되는 구조입니다."

## 13-3. SaaS 고객 대상 (BrandForge)

"제품 사진 올리면 5분 안에 광고 이미지 10장이 나옵니다. 디자이너도, 촬영 팀도 필요 없어요. 월 9.9만원이면 무제한 생성. 촬영 한 번 맡기는 것보다 싸죠."

---

# APPENDIX. Weavy 전체 노드 카탈로그 (요약)

---

## A. 이미지 모델

| 카테고리 | 1순위 | 2순위 | 3순위 |
|----------|-------|-------|-------|
| 텍스트→이미지 | Flux 2 Pro | GPT Image 1.5 | Imagen 4 |
| 벡터 그래픽 | Recraft V4 | Recraft V3 SVG | Recraft Vectorizer |
| 이미지 편집 (배경) | Replace Background | Bria Replace BG | SD3 Remove BG |
| 인페인팅 | Flux 2 Inpaint | Ideogram V3 Inpaint | Flux Fill Pro |
| 이미지→이미지 | Flux Canny Pro | Flux Depth Pro | Flux ControlNet |
| 업스케일 | Magnific Precision V2 | Topaz Image Upscale | Topaz Sharpen |

## B. 비디오 모델

| 카테고리 | 1순위 | 2순위 | 3순위 |
|----------|-------|-------|-------|
| 텍스트/이미지→비디오 | Runway Gen-4.5 | Veo 3.1 | Kling 3 |
| 비디오→비디오 | Kling o3 Edit | Runway Aleph | Runway Act-Two |
| 립싱크 | Omnihuman V1.5 | Sync 2 Pro | Kling AI Avatar |
| 비디오 업스케일 | Topaz Video Upscaler | Video Smoother | Bria Upscale |

## C. 유틸리티

| 도구 | 핵심 용도 |
|------|-----------|
| Segment Anything | 제품/인물 자동 추출 (투명 배경) |
| Composite | 전경+배경 합성 |
| Router | 1입력 → 다중 출력 (A/B 테스트) |
| Text/Image/Video Iterator | 대량 배치 처리 (거미줄 워크플로우 핵심) |
| Prompt Enhancer | AI 프롬프트 자동 확장 |
| Relight 2.0 | 조명 방향/온도 변경 |
| Color Grading | 브랜드 톤 통일 |
| Face Swap | 얼굴 교체 (UGC 대량 생산) |
| ID Preservation - Flux | 동일 인물 유지 (다중 장면) |
| Merge Audio and Video | 나레이션/BGM + 비디오 합성 |

---

## Sources

- [Weavy 공식 사이트](https://www.weavy.ai/)
- [Weavy 가격 정책](https://www.weavy.ai/pricing)
- [Weavy 이미지 모델 비교](https://help.weavy.ai/en/articles/12284752-image-models-comparison)
- [Weavy 비디오 모델 비교](https://help.weavy.ai/en/articles/12344226-video-models-comparison)
- [Figma의 Weavy 인수](https://techcrunch.com/2025/10/30/figma-acquires-ai-powered-media-generation-company-weavy/)
- [Runway Gen-4.5 ELO 1위](https://www.pixazo.ai/blog/ai-video-generation-models-comparison)
- [Veo 3.1 vs 경쟁 모델](https://pxz.ai/blog/veo-31-vs-top-ai-video-generators-2026)
- [AI 이미지 모델 비교 2026](https://www.teamday.ai/blog/best-ai-image-models-2026)
- [AI UGC 플랫폼 비교 2026](https://videoai.me/blog/best-ai-ugc-generators-2026)
- [키노히 공식몰](https://kinohi.kr/)
- [Arcads AI UGC](https://www.arcads.ai/)
- [MakeUGC](https://www.makeugc.ai/)
