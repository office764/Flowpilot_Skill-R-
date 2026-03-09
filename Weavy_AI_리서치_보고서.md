# Weavy.ai 플랫폼 완전 분석 보고서
## FlowPilot 전략 활용 가이드 | 2026.02.28

---

## 1. Weavy.ai란?

Weavy.ai는 **노드 기반(Node-based) AI 크리에이티브 워크플로우 플랫폼**이다. 하나의 캔버스 위에서 이미지 생성, 비디오 생성, 3D 생성, LLM 텍스트 처리, 전문 편집 도구(마스킹, 합성, 색보정, 리라이팅 등)를 **모두 연결하여 재사용 가능한 파이프라인**으로 구축할 수 있다.

핵심 가치: 여러 개의 AI 구독(Midjourney, Runway, DALL-E 등)을 쓰는 대신, **하나의 플랫폼에서 100개 이상의 AI 모델 + 프로 편집 도구**를 통합 사용.

2025년 10월 **Figma가 Weavy를 인수**했으며, "Figma Weave"라는 브랜드로 통합 예정이지만 현재 독립 제품으로 운영 중이다.

---

## 2. 플랫폼 사용법 (핵심 정리)

### 2-1. 기본 구조: 노드 시스템

Weavy는 n8n과 유사한 **노드 기반 캔버스**다. 각 노드가 하나의 기능(프롬프트, 이미지 생성, 비디오 변환, 편집 등)을 담당하고, 이들을 **선으로 연결하여 워크플로우**를 만든다.

### 2-2. 핵심 노드 타입

| 노드 타입 | 역할 |
|-----------|------|
| **Import Node** | 외부 이미지/비디오 파일을 캔버스로 가져옴 |
| **Prompt Node** | 텍스트 프롬프트 입력 (LLM 연동 가능) |
| **Image Gen Node** | Flux, Ideogram, Recraft 등 이미지 생성 모델 선택 |
| **Video Gen Node** | Runway, Kling, Veo, Seedance 등 비디오 생성 모델 선택 |
| **Edit Node** | 마스킹, 합성, 색보정, 리라이팅 등 프로 편집 |
| **Router Node** | 하나의 입력을 여러 출력으로 분기 (A/B 비교에 활용) |
| **Preview Node** | 생성 결과를 미리보기 |
| **Export Node** | 최종 결과물을 이미지/비디오 파일로 내보내기 |
| **Prompt Enhancer** | 입력 프롬프트를 AI가 자동으로 개선/확장 |

### 2-3. 기본 워크플로우 구축 순서

1단계: 캔버스에 **Prompt Node** 추가 → 원하는 장면/이미지 설명 입력
2단계: **Router Node** 연결 → 하나의 프롬프트를 여러 모델에 동시 전송
3단계: 각 분기에 **Image Gen Node** 또는 **Video Gen Node** 추가 → 모델 선택
4단계: 결과에 **Edit Node** 연결 → 색보정, 마스킹, 업스케일 등 후처리
5단계: **Export Node**로 최종 출력

### 2-4. 프로 팁: 크레딧 절약 전략

저렴한 모델(Flux Schnell, SD 3.5 등)로 먼저 프롬프트와 구도를 테스트한다. 워크플로우가 확정되면 해당 노드만 고급 모델(Flux Pro, Veo 3 등)로 교체하여 최종 렌더링한다. 이것이 노드 기반의 최대 장점이다.

### 2-5. App Mode (비기술 사용자용)

복잡한 노드 캔버스를 숨기고, **간단한 폼 인터페이스**만 보여주는 모드. 클라이언트에게 워크플로우를 전달할 때, 기술적 복잡성 없이 "프롬프트 입력 → 결과 생성"만 할 수 있게 해준다.

---

## 3. 가격 체계

| 플랜 | 월 요금 | 월 크레딧 | 핵심 특징 |
|------|---------|-----------|-----------|
| **Free** | $0 | 150 | 모든 모델 접근 가능, 워크플로우 5개 제한 |
| **Starter** | $19 | 1,500 | 약 3,750장 이미지 또는 417초 비디오, 상업적 라이선스 |
| **Professional** | $36 | 크레딧 롤오버(최대 3배 누적), 추가 크레딧 $10/1,200 |
| **Team** | $48/유저 | 4,500 | 팀 권한 관리, App Mode 포함 |
| **Enterprise** | 맞춤 | 맞춤 | 전용 지원, 맞춤 통합 |

크레딧 추가 구매: Starter $10/1,000크레딧, Pro/Team $10/1,200크레딧 (12개월 롤오버)

---

## 4. 모델별 분야/용도 분류 (스크린샷 기반 + 리서치)

### 4-1. 이미지 생성 모델

| 모델 | 최적 용도 | 강점 |
|------|-----------|------|
| **Flux (Black Forest Labs)** | 포토리얼리즘, 인물/제품 사진 | 2026년 최고 포토리얼 모델. 피부 텍스처, 조명 탁월 |
| **Flux Schnell** | 빠른 프로토타이핑, 테스트용 | 크레딧 저렴, 빠른 생성. 아이디어 검증에 최적 |
| **Flux Pro / Pro Ultra** | 최종 고품질 렌더링, 인쇄물 | 고해상도 Raw 모드 지원. 최종 결과물에 사용 |
| **Ideogram** | 로고, 텍스트 포함 이미지, 배너 | 텍스트 렌더링 거의 완벽. 로고/배너 제작 2순위 |
| **Recraft V4** | 로고, 벡터, 브랜드 아이덴티티 | HuggingFace 벤치마크 1위. SVG 벡터 내보내기 지원 |
| **Stable Diffusion 3.5** | 범용, 스타일 다양, 테스트 | 크레딧 효율적. 스타일 커스텀 자유도 높음 |
| **Seedream** | 정밀 제어가 필요한 이미지 | Precision 특화 모델 |
| **Nano-Banana** | 정밀 제어 이미지 | Seedream과 유사한 정밀 제어 특화 |
| **Higgsfield Image** | 이미지-비디오 연계 기초 이미지 | 비디오 생성 전 기초 이미지 생성에 활용 |

### 4-2. 비디오 생성 모델

| 모델 | 최적 용도 | 강점 |
|------|-----------|------|
| **Runway Gen-4** | 프로 영상 편집, Motion Brush 활용 | Motion Brush, Camera Controls 등 프로 편집 기능 |
| **Veo 3 (Google)** | 시네마틱 영상, 고품질 장면 | 높은 시각적 퀄리티. 영화급 장면 생성 |
| **Kling** | 빠른 비디오 프로토타이핑 | 속도 대비 품질 균형 우수 |
| **Seedance** | 시네마틱 비디오, 댄스/모션 | 동적 모션 특화 |
| **Luma Ray 3** | 포스트 프로덕션, 3D 씬 이해 | 편집 워크플로우에 강함. 3D 공간 이해 시작 단계 |
| **Luma Reframe** | 영상 리프레이밍, 종횡비 변환 | 기존 영상의 프레임 재구성에 특화 |
| **Wan 2.2 Animate** | 모션 전이 (이미지에 영상 모션 적용) | 스틸 이미지에 비디오의 움직임을 입히는 용도 |
| **Sora (OpenAI)** | 텍스트-투-비디오 | OpenAI의 대표 비디오 모델 |
| **Higgsfield Video** | 숏폼 시네마틱, 카메라 컨트롤 | 영상 내 카메라 앵글/움직임 정밀 제어 |

### 4-3. 편집/유틸리티 도구

| 도구 | 용도 |
|------|------|
| **Upscaler** | 저해상도 → 고해상도 변환 |
| **Masking** | 오브젝트 분리, 배경 제거 |
| **Relighting** | 조명 방향/강도 변경 |
| **Color Grading** | 색감 보정, 브랜드 팔레트 적용 |
| **Compositing** | 여러 레이어 합성 |
| **Prompt Enhancer** | AI 프롬프트 자동 최적화 |

### 4-4. 산업별 추천 모델 조합

**이커머스/제품 사진:**
Flux Pro (제품 이미지) → Relighting (조명 보정) → Upscaler (고해상도) → Export

**소셜 미디어 콘텐츠:**
Ideogram (텍스트 배너) + Flux Schnell (빠른 비주얼) → Seedance/Kling (숏폼 비디오) → Export

**브랜드 아이덴티티:**
Recraft V4 (로고 SVG) + Ideogram (텍스트 그래픽) → Color Grading (브랜드 컬러)

**영상 프로덕션/광고:**
Flux Pro (키프레임 이미지) → Runway Gen-4 (Motion Brush) 또는 Veo 3 (시네마틱) → Luma Ray 3 (후처리)

**부동산 스테이징:**
Import (원본 사진) → Flux Pro (가상 스테이징) → Relighting + Compositing → Export

**패션/뷰티:**
Flux Pro Ultra (모델 사진) → Wan 2.2 Animate (모션 적용) → Color Grading → Export

---

## 5. FlowPilot 세일즈/마케팅 전략

### 5-1. 타겟 고객 세그먼트

| 세그먼트 | Pain Point | 솔루션 프레이밍 |
|----------|------------|-----------------|
| **이커머스 사업자** | 제품 사진 촬영 비용, 시간 소모 | "촬영 없이 AI로 제품 사진 무한 생성, 월 구독으로 연간 수천만 원 절감" |
| **마케팅 에이전시** | 소셜 콘텐츠 대량 생산 병목 | "하나의 워크플로우로 브랜드별 콘텐츠 자동 대량 생산" |
| **부동산 업체** | 가상 스테이징/홈투어 비용 | "빈 집 사진 → AI 가상 인테리어 자동 생성 파이프라인" |
| **영상 프로덕션** | 프리프로덕션 시간/비용 | "컨셉 아트 → 스토리보드 → 프리비즈까지 AI 자동화" |
| **패션/뷰티 브랜드** | 모델/촬영 비용, 계절 콘텐츠 대량 필요 | "AI 모델 + 가상 촬영 파이프라인으로 콘텐츠 비용 80% 절감" |

### 5-2. 세일즈 퍼널 설계

**1단계 - 인지 (Awareness):**
"AI로 제품 사진 5분 만에 만든 결과" 같은 Before/After 숏폼 콘텐츠를 인스타, 유튜브 숏츠, 틱톡에 배포. Weavy 워크플로우 시연 영상이 곧 리드 마그넷이 된다.

**2단계 - 유입 (Attraction):**
"무료 AI 콘텐츠 워크플로우 템플릿 5종 받기" 랜딩페이지. 이메일 수집 후 Weavy 워크플로우 JSON + 사용법 가이드 제공.

**3단계 - 신뢰 구축 (Trust):**
웨비나/라이브에서 실시간 Weavy 워크플로우 시연. "이 워크플로우를 우리가 여러분 비즈니스에 맞춤 구축해드립니다."

**4단계 - 세일즈 콜 (Conversion):**
고객 비즈니스 분석 → 맞춤 워크플로우 데모 → ROI 수치화 제안

**5단계 - 결제 (Close):**
초기 구축비 + 월 유지보수(MRR) 구조

### 5-3. 가격 전략 (FlowPilot 서비스)

| 서비스 | 가격대 | 내용 |
|--------|--------|------|
| **기본 워크플로우 구축** | 150~300만원 (1회) | 고객 맞춤 Weavy 워크플로우 1~3개 구축 + 교육 |
| **프리미엄 파이프라인** | 500~1,000만원 (1회) | n8n + Weavy 통합 자동화 (트리거 → 생성 → 배포 전체) |
| **월 유지보수 (MRR)** | 50~150만원/월 | 워크플로우 업데이트, 새 모델 적용, 기술 지원 |
| **엔터프라이즈** | 2,000만원+ | Weavy + n8n + Antigravity 풀스택 자동화 |

### 5-4. 업셀(Upsell) 전략

**레벨 1 → 레벨 2 업셀:**
"Weavy 워크플로우만 구축" → "n8n 연동으로 트리거 자동화" (예: 구글 시트에 제품 등록 → 자동 이미지 생성 → 쇼핑몰 업로드)

**레벨 2 → 레벨 3 업셀:**
"n8n + Weavy 자동화" → "Antigravity 프론트엔드 대시보드 구축" (고객이 직접 관리하는 AI 콘텐츠 포털)

**크로스셀:**
Weavy 콘텐츠 생성 워크플로우 고객에게 → "생성된 콘텐츠 자동 배포" (소셜 미디어 자동 포스팅, 이메일 마케팅 연동) 추가 제안

---

## 6. FlowPilot만의 차별화 포인트

### 6-1. n8n + Weavy 시너지 (핵심 무기)

Weavy 단독은 "수동으로 버튼 눌러서 콘텐츠 생성"이다. 여기에 **n8n을 붙이면 완전 자동화**가 된다.

예시 파이프라인:
n8n Webhook (신제품 등록 트리거) → n8n HTTP Request (Weavy API 호출) → Weavy 워크플로우 실행 (제품 이미지 5종 자동 생성) → n8n에서 결과 수신 → Shopify/쿠팡에 자동 업로드

이것은 Weavy만 쓰는 경쟁사가 절대 흉내 낼 수 없는 FlowPilot만의 기술 장벽이다.

### 6-2. App Mode 활용 → 고객 셀프서비스 포털

Weavy의 App Mode를 활용하여, 비기술 고객에게 "프롬프트 입력만 하면 결과 나오는" 간편 인터페이스를 제공. 이를 Antigravity 대시보드에 임베딩하면 완전한 "AI 콘텐츠 셀프서비스 포털"이 된다. 이 포털 자체가 월 구독 상품이 되어 MRR을 만든다.

---

## 7. 아이디어 제안

### Cash Cow (즉시 실행 가능)
"이커머스 AI 제품 사진 자동화 서비스" — 쇼핑몰 사업자에게 Weavy 워크플로우 구축 + n8n 자동화. 시장 수요 폭발적이고, 기존 제품 촬영 대비 90% 비용 절감이라는 명확한 ROI 제시 가능.

### Moonshot (고가 혁신형)
"AI 콘텐츠 팩토리 플랫폼" — Antigravity 프론트엔드 + Weavy 엔진 + n8n 오케스트레이션으로, 기업 고객이 자체적으로 수백 종의 마케팅 콘텐츠를 생산하는 풀 플랫폼. 연 계약 5,000만원+ 가능.

---

## Sources

- [Weavy 공식 사이트](https://www.weavy.ai/)
- [Weavy 가격 정책](https://www.weavy.ai/pricing)
- [Weavy 이미지 모델 비교](https://help.weavy.ai/en/articles/12284752-image-models-comparison)
- [Weavy 비디오 모델 비교](https://help.weavy.ai/en/articles/12344226-video-models-comparison)
- [Figma의 Weavy 인수 (TechCrunch)](https://techcrunch.com/2025/10/30/figma-acquires-ai-powered-media-generation-company-weavy/)
- [Weavy 실전 가이드 (Frontmatter)](https://www.frontmatter.io/blog/a-practical-guide-to-weavy-ai-for-creative-pros-who-prefer-tools-over-hype)
- [Weavy x fal 통합](https://blog.fal.ai/powering-creative-workflows-with-weavy-x-fal/)
- [Weavy 제품 리뷰 (Building Creative Machines)](https://buildingcreativemachines.substack.com/p/product-review-weavy-makes-multi)
- [Weavy 가격 분석 (Chase Jarvis)](https://chasejarvis.com/blog/weavy-pricing/)
- [Weavy 구독 플랜 (Knowledge Center)](https://help.weavy.ai/en/articles/12267070-weavy-s-subscription-plans)
