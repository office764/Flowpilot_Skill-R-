# Weavy.ai 전체 노드 카탈로그 완전 가이드
## 카테고리별 모델 랭킹 + 용도 분석 | FlowPilot 2026.02.28

---

# A. IMAGE MODELS (이미지 모델)

---

## A-1. Generate from Text (텍스트 → 이미지 생성)

**언제 쓰는 카테고리:** 텍스트 프롬프트만으로 이미지를 처음부터 생성할 때. 제품 컨셉 이미지, 배경, 라이프스타일 장면, 광고 소재 등 "아직 존재하지 않는 이미지"를 만드는 모든 경우.

| 순위 | 모델 | 최적 용도 | 강점 | 약점 |
|------|------|-----------|------|------|
| 1위 | **Flux 2 Pro** | 포토리얼 인물/제품, 광고 소재 | 피부 텍스처, 조명, 반사 등 사진급 최고. 캐릭터 일관성 유지 최강 | 텍스트 렌더링 불안정 |
| 2위 | **GPT Image 1.5** | 종합 퀄리티, 텍스트 포함 이미지 | 텍스트 렌더링 정확도 최고, 멀티모달 이해력 | ChatGPT/API 필요, 속도 느릴 수 있음 |
| 3위 | **Imagen 4** | 고퀄리티 포토리얼 + 텍스트 | Google 모델. 포토리얼리즘 + 텍스트 둘 다 강함 | 스타일 다양성 제한 |
| 4위 | **Ideogram V3** | 배너, 포스터, 텍스트 그래픽 | 텍스트 렌더링 90% 정확도, 타이포 특화 | 포토리얼리즘 약함 |
| 5위 | **Recraft V4** | 로고, 브랜드 에셋, 디자인 | HuggingFace ELO 1위, SVG 내보내기, 브랜드 스타일링 | 사진급 이미지에는 부적합 |
| 6위 | **Flux Pro 1.1 Ultra** | 초고해상도 인쇄물, Raw 포토 | 최고 해상도, Raw 모드 후보정 유연 | 크레딧 비쌈 |
| 7위 | **Flux 2 Dev LoRA** | 커스텀 스타일, LoRA 미세조정 | 자체 LoRA 모델 적용 가능 | 설정 난이도 높음 |
| 8위 | **Reve** | 아트 스타일 이미지 | 독특한 아트 스타일 생성 | 포토리얼 약함 |
| 9위 | **Higgsfield Image** | I2V용 기초 이미지 | 비디오 전환 최적화 이미지 생성 | 단독 사용 시 퀄리티 중간 |
| 10위 | **Minimax Image O1** | 범용 이미지 생성 | 중간 퀄리티, 가성비 양호 | 상위 모델 대비 뚜렷한 강점 없음 |
| 11위 | **Stable Diffusion 3.5** | 빠른 테스트, 프로토타입 | 크레딧 저렴, 스타일 자유도 높음 | 퀄리티 중하위 |
| 12위 | **Flux Fast** | 초고속 프로토타입 | 가장 빠른 생성 | 퀄리티 최하위 |
| 13위 | **Luma Photon** | 범용 | Luma 생태계 연동 | 특출난 강점 없음 |
| 14위 | **Bria** | 상업적 안전 이미지 | 저작권 안전 학습 데이터 | 퀄리티 제한 |
| 15위 | **Nvidia Sana** | 기술 데모 | Nvidia 연구 모델 | 상용 활용 제한적 |

**FlowPilot 추천 조합:**
- 제품 사진 → **Flux 2 Pro**
- 텍스트 배너/포스터 → **Ideogram V3**
- 로고/SVG → **Recraft V4**
- 빠른 테스트 → **Flux Fast** 또는 **Stable Diffusion 3.5**

---

## A-2. Generate Vector Graphics (벡터 그래픽 생성)

**언제 쓰는 카테고리:** 로고, 아이콘, 일러스트를 **확대해도 깨지지 않는 벡터(SVG)** 형식으로 만들 때. 인쇄물, 브랜딩 에셋.

| 순위 | 모델 | 최적 용도 | 강점 |
|------|------|-----------|------|
| 1위 | **Recraft V4** (New) | 로고, 브랜드 아이콘 | 최신 벡터 생성. 디자인 정밀도 최고 |
| 2위 | **Recraft V3 SVG** | SVG 직접 출력 | 검증된 안정성, SVG 네이티브 |
| 3위 | **Recraft Vectorizer** (New) | 래스터 → 벡터 변환 | 기존 PNG/JPG 이미지를 SVG로 변환 |
| 4위 | **Vectorizer** | 기본 벡터 변환 | 심플한 변환 도구 |
| 5위 | **Text To Vector** | 텍스트 → 벡터 직접 | 간단한 타이포 벡터화 |

---

## A-3. Edit Images (이미지 편집)

**언제 쓰는 카테고리:** 이미 존재하는 이미지를 **부분 수정, 오브젝트 제거/추가, 배경 교체, 인페인팅, 아웃페인팅** 할 때. 키노히처럼 제품 원본을 유지하면서 배경만 바꾸는 작업에 핵심.

| 순위 | 모델 | 최적 용도 | 강점 |
|------|------|-----------|------|
| **배경 교체 전용** | | | |
| 1위 | **Replace Background** | 배경만 교체 (제품 유지) | 원클릭 배경 교체, 제품 형태 100% 보존 |
| 2위 | **Bria Replace Background** | 배경 교체 대안 | 상업적 안전 데이터 학습 |
| 3위 | **SD3 Remove Background** | 배경 제거 (투명화) | 깔끔한 배경 제거 |
| 4위 | **Bria Remove Background** | 배경 제거 대안 | 안정적 |
| **인페인팅 (부분 수정)** | | | |
| 1위 | **Flux 2 Inpaint [Klein 9B]** | 특정 영역만 수정 | Flux 퀄리티로 부분 수정, 나머지 유지 |
| 2위 | **Ideogram V3 Inpaint** | 텍스트 영역 수정 | 텍스트 포함 부분 수정에 강함 |
| 3위 | **Flux Fill Pro** | 빈 영역 채우기 | 자연스러운 Content-Aware 채우기 |
| 4위 | **SD3 Inpaint** | 범용 인페인팅 | 안정적, 크레딧 저렴 |
| 5위 | **Bria Inpaint** | 상업적 안전 인페인팅 | 저작권 안전 |
| **아웃페인팅 (이미지 확장)** | | | |
| 1위 | **Flux Pro Outpaint** | 이미지 바깥 영역 생성 | Flux 퀄리티로 이미지 테두리 확장 |
| 2위 | **SD3 Outpaint** | 이미지 확장 대안 | 가성비 양호 |
| **종합 이미지 편집** | | | |
| 1위 | **Nano Banana 2** (New) | 텍스트 지시로 이미지 수정 | 최신 모델, 자연어로 편집 지시 |
| 2위 | **Seedream V5 Edit** (New) | 정밀 편집 | 최신, 높은 편집 정확도 |
| 3위 | **Flux 2 Max** | 고퀄리티 편집 | Flux 최고 품질로 편집 |
| 4위 | **GPT Image 1.5 Edit** | 자연어 편집 | 대화형 편집 지시 |
| 5위 | **Flux Kontext** | 멀티이미지 참조 편집 | 여러 참조 이미지 기반 편집 |
| 6위 | **Runway Gen-4 Image** | Runway 이미지 편집 | 비디오 연계 이미지 편집 |
| **특수 기능** | | | |
| - | **Relight 2.0** | 조명 방향/강도 변경 | 촬영 후 조명 완전 재설정 |
| - | **Kolors Virtual Try On** | 의류 가상 피팅 | 패션 이커머스 필수 |
| - | **SD3 Content-Aware Fill** | 오브젝트 제거 후 채우기 | 포토샵 Content-Aware Fill의 AI 버전 |

**키노히 프로젝트에 바로 쓸 모델:**
- **Replace Background** → 제품 원본 유지 + 배경 자동 교체 (가장 간단)
- **SD3 Remove Background** + **Composite** → 수동 합성 (더 정밀한 제어)
- **Relight 2.0** → 조명 통일 (여러 시안의 조명 톤 맞추기)

---

## A-4. Generate from Image (이미지 → 이미지 변환)

**언제 쓰는 카테고리:** 기존 이미지를 참조하여 **스타일 변환, 구도 유지하면서 새 이미지 생성, 스케치→완성** 등.

| 순위 | 모델 | 최적 용도 | 강점 |
|------|------|-----------|------|
| 1위 | **Flux Canny Pro** | 외곽선 유지 + 스타일 변환 | 원본 구도/형태를 정확히 유지하면서 스타일만 변경 |
| 2위 | **Flux Depth Pro** | 깊이 맵 기반 변환 | 3D 공간감 유지하면서 스타일 변환 |
| 3위 | **Flux ControlNet & LoRA** | 정밀 제어 변환 | ControlNet으로 포즈/외곽/깊이 제어 |
| 4위 | **Image To Image** | 범용 I2I | 기본 Image-to-Image 변환 |
| 5위 | **Sketch to Image** | 손그림 → 완성 이미지 | 러프 스케치를 완성 이미지로 |
| 6위 | **Flux Dev Redux** | 이미지 재생성 | 참조 이미지 기반 변형 생성 |
| 7위 | **Qwen Edit Multiangle** | 멀티앵글 생성 | 한 이미지에서 여러 앵글 변형 |

---

## A-5. Enhance Images (이미지 업스케일/향상)

**언제 쓰는 카테고리:** AI 생성 이미지를 **고해상도로 업스케일**, 피부 보정, 선명도 향상. 인쇄물 제작, 대형 디스플레이용.

| 순위 | 모델 | 최적 용도 | 강점 | 비용 |
|------|------|-----------|------|------|
| 1위 | **Magnific Precision Upscale V2** | 최고 퀄리티 업스케일 | 디테일 "창조" 능력 최강. AI 아트 업스케일 최적 | 비쌈 |
| 2위 | **Topaz Image Upscale** | 사진 복원, 충실한 업스케일 | 원본에 가장 충실한 복원. 피부/텍스처 왜곡 최소 | 중간 |
| 3위 | **Topaz Sharpen** | 블러 제거, 선명도 | 흐릿한 이미지 복구 최강 | 중간 |
| 4위 | **Magnific Skin Enhancer** | 인물 피부 보정 | AI 생성 인물의 플라스틱 피부 → 자연 피부 | 비쌈 |
| 5위 | **Recraft Crisp Upscale** | 가성비 업스케일 | $0.006/이미지로 가장 저렴. 대량 처리 최적 | 최저 |
| 6위 | **Magnific Upscale** | 크리에이티브 업스케일 | 원본에 없던 디테일 추가 | 비쌈 |
| 7위 | **Enhancor Image Upscale** | AI 이미지 텍스처 복원 | AI 생성 이미지의 플라스틱 느낌 제거 특화 | 중간 |
| 8위 | **Enhancor Realistic Skin** | AI 인물 피부 리얼리즘 | 피부 모공, 질감 추가 | 중간 |

**키노히 프로젝트 추천:**
- 제품 이미지 업스케일 → **Topaz Image Upscale** (원본 충실 복원)
- AI 생성 라이프스타일 이미지 → **Magnific Precision Upscale V2** (디테일 보강)
- 대량 배치 → **Recraft Crisp Upscale** (가성비)

---

# B. VIDEO MODELS (비디오 모델)

---

## B-1. Generate from Text or Image (텍스트/이미지 → 비디오)

**언제 쓰는 카테고리:** 프롬프트나 이미지에서 비디오를 생성. 광고, 숏폼, 브랜드 필름 등 모든 비디오 콘텐츠 제작.

| 순위 | 모델 | 최적 용도 | 강점 | 약점 |
|------|------|-----------|------|------|
| 1위 | **Runway Gen-4.5** (New) | 종합 최강, 프리미엄 광고 | ELO 1,247 전체 1위. 스타일 일관성, 모션 퀄리티 최고 | 기본 720p, 비쌈 |
| 2위 | **Veo 3.1 I2V** | 시네마틱 4K, 방송급 | 4K + 공간 오디오, 물리 시뮬레이션 최고 | 속도 느림, 비쌈 |
| 3위 | **Veo 3.1 T2V** | 텍스트→시네마틱 비디오 | 프롬프트 준수율 최고, 방송 파이프라인 투입 가능 | 비쌈 |
| 4위 | **Kling 3** (New) | 가성비 4K/60fps, 대량 생산 | 네이티브 4K/60fps, 무료 티어, 다국어 립싱크 | 텍스처 디테일 간혹 부족 |
| 5위 | **Seedance V1.5 Pro** | 오디오 싱크, 크리에이티브 | 12개 멀티모달 참조 입력, 음악 싱크 | 글로벌 접근성 제한 |
| 6위 | **Sora 2** | 25초 긴 클립, 물리 리얼리즘 | 최장 25초 네이티브 클립, 스토리보드 편집 | 접근 제한, 비쌈 |
| 7위 | **Higgsfield Video** | 카메라 컨트롤, 숏폼 | 돌리 줌, 랙 포커스 등 정밀 카메라 프리셋 | 긴 영상 불가 |
| 8위 | **Runway Gen-4** | 프로 편집, Motion Brush | Motion Brush로 부분 모션 제어 | Gen-4.5 대비 퀄리티 낮음 |
| 9위 | **Grok Imagine Video** | xAI 생태계 | xAI 연동 | 초기 단계 |
| 10위 | **Minimax Hailuo-O2** | 범용 비디오 | 안정적 품질 | 상위 모델 대비 특색 없음 |
| 11위 | **Wan 2.5** | 범용, 카메라 마스터리 | 카메라 움직임 제어 양호 | 전반적 퀄리티 중간 |
| 12위 | **Pixverse V4.5** | 스타일라이즈 비디오 | 독특한 아트 스타일 | 포토리얼 약함 |
| 13위 | **LTX 2 Video** | 빠른 프로토타입 | 속도 최고, 크레딧 저렴 | 퀄리티 하위 |
| 14위 | **Moonvalley** | 범용 | 안정적 | 특출난 강점 없음 |

**FlowPilot 용도별 추천:**
- 프리미엄 광고 → **Runway Gen-4.5**
- 방송급 4K → **Veo 3.1**
- 대량 숏폼 → **Kling 3** (가성비 최강)
- 음악 싱크 → **Seedance V1.5 Pro**
- 카메라 앵글 정밀 제어 → **Higgsfield Video**
- 빠른 테스트 → **LTX 2 Video**

---

## B-2. Generate from Video (비디오 → 비디오 변환)

**언제 쓰는 카테고리:** 기존 비디오에 **스타일 변환, 모션 전이, 리프레이밍, 편집** 적용. 촬영된 영상을 AI로 변형/향상.

| 순위 | 모델 | 최적 용도 | 강점 |
|------|------|-----------|------|
| 1위 | **Kling o3 Edit Video** (New) | 비디오 부분 편집 | 최신 모델, 비디오 인페인팅/편집 |
| 2위 | **Runway Aleph** | 스타일 전환 | 비디오 전체 스타일 변환 |
| 3위 | **Runway Act-Two** | 캐릭터 모션 전이 | 한 비디오의 동작을 다른 캐릭터에 적용 |
| 4위 | **Luma Reframe** | 종횡비 변환, 리프레이밍 | 가로→세로, 세로→가로 자동 리프레이밍 |
| 5위 | **Luma Modify** | 비디오 수정 | 비디오 내 요소 수정 |
| 6위 | **Wan 2.2 Animate - Move** | 이미지에 모션 입히기 | 스틸 이미지에 비디오 움직임 전이 |
| 7위 | **Wan 2.2 Animate - Replace** | 비디오 내 요소 교체 | 비디오 속 오브젝트를 다른 것으로 대체 |
| 8위 | **Grok Imagine - Video Edit** | 비디오 편집 | xAI 기반 비디오 수정 |
| 9위 | **Kling Motion Control** | 모션 궤적 제어 | 오브젝트 움직임 경로 지정 |
| 10위 | **Wan Vace Depth/Pose/Reframe** | 깊이/포즈/리프레임 | 정밀 제어 도구 세트 |

**키노히 프로젝트 추천:**
- 가로 촬영 → 세로 숏폼 변환 → **Luma Reframe**
- 제품 이미지에 모션 입히기 → **Wan 2.2 Animate - Move**
- 기존 영상 스타일 변환 → **Runway Aleph**

---

## B-3. Lip Sync (립싱크)

**언제 쓰는 카테고리:** AI 아바타/인물에 **말하는 입 움직임을 입힐 때**. AI UGC 리뷰 영상, 제품 소개 영상의 핵심.

| 순위 | 모델 | 최적 용도 | 강점 |
|------|------|-----------|------|
| 1위 | **Omnihuman V1.5** | 가장 자연스러운 립싱크 | 입 + 표정 + 고개 움직임까지 자연스러움 |
| 2위 | **Sync 2 Pro** | 프로급 립싱크 | 높은 정확도, 다국어 지원 |
| 3위 | **Kling AI Avatar** | Kling 생태계 립싱크 | Kling 비디오와 연계 최적 |
| 4위 | **Pixverse Lipsync** | 범용 립싱크 | 기본적인 립싱크 |

**AI UGC 리뷰 영상 제작 시:**
Flux로 인물 이미지 생성 → Kling 3로 비디오 변환 → **Omnihuman V1.5**로 립싱크 추가

---

## B-4. Enhance Videos (비디오 업스케일/향상)

**언제 쓰는 카테고리:** AI 생성 비디오의 **해상도 업스케일, 프레임 보간, 화질 향상**. 최종 배포 전 마무리 단계.

| 순위 | 모델 | 최적 용도 | 강점 |
|------|------|-----------|------|
| 1위 | **Topaz Video Upscaler** | 비디오 해상도 업스케일 | 가장 안정적, 원본 충실 복원 |
| 2위 | **Video Smoother** | 프레임 보간, 부드러운 재생 | AI 영상 특유의 깜빡임/떨림 제거 |
| 3위 | **Bria Upscale** | 가성비 비디오 업스케일 | 저렴한 비디오 업스케일 |
| 4위 | **Real-ESRGAN Video Upscaler** | 오픈소스 업스케일 | 무료/저렴 대안 |

---

# C. TEXT TOOLS (텍스트 도구)

**언제 쓰는 카테고리:** 프롬프트 작성, AI 텍스트 처리, 이미지/비디오 설명 생성.

| 도구 | 역할 | 언제 쓰나 |
|------|------|-----------|
| **Prompt** | 텍스트 프롬프트 입력 | 모든 워크플로우의 시작점 |
| **Prompt Enhancer** | AI가 프롬프트 자동 확장/개선 | 짧은 프롬프트 → 디테일한 프롬프트로 변환 |
| **Prompt Concatenator** | 여러 프롬프트 합치기 | 부분별 프롬프트를 하나로 결합 |
| **Run Any LLM** | GPT/Claude 등 LLM 실행 | 프롬프트 자동 생성, 텍스트 분석 |
| **Image Describer** | 이미지 → 텍스트 설명 생성 | 이미지를 보고 프롬프트 역추출 |
| **Video Describer** | 비디오 → 텍스트 설명 생성 | 비디오 장면 설명 자동 생성 |

---

# D. TOOLBOX - EDITING (편집 도구)

**언제 쓰는 카테고리:** 생성된 이미지/비디오의 **밝기, 색감, 크기, 흐림, 자르기** 등 기본 편집.

| 도구 | 역할 | 언제 쓰나 |
|------|------|-----------|
| **Levels** | 밝기/대비/감마 조정 | 이미지 밝기 전체 보정 |
| **Compositor** | 레이어 합성 | 여러 이미지를 겹쳐 합성 |
| **Painter** | 캔버스 위 직접 그리기/마스킹 | 인페인팅 마스크 수동 지정 |
| **Crop** | 이미지 자르기 | 불필요한 영역 제거, 구도 조정 |
| **Resize** | 이미지 크기 변경 | 해상도/비율 변경 (SNS 사이즈 맞추기) |
| **Blur** | 흐림 효과 | 배경 블러, 보케 효과 |
| **Invert** | 색상 반전 | 네거티브 이미지 |
| **Channels** | RGB 채널 분리 | 색상 채널별 편집 |
| **Extract Video Frame** | 비디오에서 프레임 추출 | 비디오 특정 장면을 이미지로 |
| **Video Concatenator** | 비디오 이어붙이기 | 여러 클립을 하나로 합침 |

---

# E. MATTE (마스킹/매트)

**언제 쓰는 카테고리:** 이미지/비디오에서 **특정 오브젝트를 분리, 배경 제거, 마스크 생성**. 합성 작업의 핵심.

| 도구 | 역할 | 언제 쓰나 |
|------|------|-----------|
| **Mask Extractor** | 자동 마스크 추출 | 제품/인물 자동 분리 |
| **Mask by Text** | 텍스트로 마스크 지정 | "bottle"이라고 입력하면 보틀만 마스킹 |
| **Matte Grow/Shrink** | 마스크 영역 확대/축소 | 마스크 경계 미세 조정 |
| **Merge Alpha** | 알파 채널 병합 | 두 마스크 합치기 |
| **Video Matte** | 비디오 배경 제거 | 영상에서 인물/제품만 추출 |
| **Video Mask by Text** | 텍스트로 비디오 마스크 | 영상 속 특정 오브젝트만 분리 |

---

# F. ITERATORS (반복 처리)

**언제 쓰는 카테고리:** **대량 배치 작업**. 여러 프롬프트/이미지/비디오를 자동 반복 처리. 거미줄 워크플로우의 대량 생산 핵심.

| 도구 | 역할 | 언제 쓰나 |
|------|------|-----------|
| **Text Iterator** | 프롬프트 리스트 자동 반복 | 프롬프트 10개를 넣으면 이미지 10장 자동 생성 |
| **Image Iterator** | 이미지 리스트 자동 반복 | 제품 이미지 5장 → 각각 배경 교체 자동 반복 |
| **Video Iterator** | 비디오 리스트 자동 반복 | 생성된 비디오 여러 개 일괄 후처리 |

**거미줄 워크플로우의 비밀이 바로 이 Iterator다.** Text Iterator에 프롬프트 20개를 넣고, Flux에 연결하면 이미지 20장이 자동 생성된다.

---

# G. HELPERS (헬퍼/유틸리티)

| 도구 | 역할 |
|------|------|
| **Import** | 외부 파일 가져오기 |
| **Export** | 결과 파일 내보내기 |
| **Preview** | 결과 미리보기 |
| **Router** | 하나의 입력을 여러 출력으로 분기 |
| **Output** | 워크플로우 최종 출력 지점 |
| **Import Model** | 외부 AI 모델 가져오기 |
| **Import LoRA** | LoRA 모델 가져오기 |
| **Import Multiple LoRAs** | 여러 LoRA 동시 적용 |
| **Sticky Note** | 캔버스 메모 |

---

# H. DATATYPES (데이터 타입)

| 도구 | 역할 | 언제 쓰나 |
|------|------|-----------|
| **Number** | 숫자 값 입력 | Strength, CFG 등 파라미터 제어 |
| **Text** | 텍스트 값 입력 | 고정 텍스트 전달 |
| **Toggle** | On/Off 스위치 | 조건부 분기 |
| **List Selector** | 리스트에서 선택 | 여러 옵션 중 하나 선택 |
| **Seed** | 시드 값 지정 | 동일 결과 재현 (같은 시드 = 같은 결과) |
| **Array** | 배열 데이터 | Iterator와 함께 배치 데이터 전달 |

---

# I. 3D MODELS (3D 모델 생성)

**언제 쓰는 카테고리:** 텍스트/이미지에서 **3D 오브젝트 생성**. 제품 3D 뷰, AR/VR 에셋, 3D 애니메이션.

| 순위 | 모델 | 최적 용도 | 강점 |
|------|------|-----------|------|
| 1위 | **Meshy V6** | 종합 3D 생성 | 텍스트/이미지→3D 변환 품질 최고 |
| 2위 | **Rodin V2** | 정밀 3D | 하이디테일 3D 메시 |
| 3위 | **Hunyuan 3D V3** | 최신 3D | 텐센트 최신 모델 |
| 4위 | **Trellis 3D V2** (New) | 최신 3D | 빠른 생성 |
| 5위 | **SAM 3D - Objects** | 오브젝트 추출 | 이미지에서 3D 오브젝트 분리 |

---

# J. COMMUNITY MODELS (커뮤니티 모델) — 핵심만

| 모델 | 용도 | 언제 쓰나 |
|------|------|-----------|
| **Face Swap** | 얼굴 교체 | AI UGC에서 아바타 얼굴 변경 |
| **Expression Editor** | 표정 편집 | 인물 표정 수정 |
| **ID Preservation - Flux** | 동일 인물 유지 | 여러 장면에서 같은 캐릭터 |
| **Add Logo** | 로고 삽입 | 생성 이미지에 브랜드 로고 자동 삽입 |
| **ElevenLabs - Voice Changer** | 음성 변환 | AI UGC 나레이션용 |
| **Video to Audio** | 비디오→오디오 생성 | 영상에 맞는 배경음 자동 생성 |
| **Merge Audio and Video** | 오디오+비디오 합치기 | 나레이션/음악을 비디오에 합성 |
| **Increase Frame-rate** | 프레임레이트 증가 | 24fps→60fps 보간 |
| **Recraft Creative Upscale** | 크리에이티브 업스케일 | Recraft 스타일 업스케일 |
| **SDXL Consistent Character** | 일관된 캐릭터 생성 | 여러 장면에서 같은 캐릭터 유지 |

---

## Sources

- [Flux 2 vs Recraft V4 vs GPT Image 비교 2026](https://www.teamday.ai/blog/best-ai-image-models-2026)
- [AI 이미지 모델 ELO 리더보드](https://artificialanalysis.ai/image/models)
- [Seedance 2.0 vs Kling 3.0 vs Sora 2 vs Veo 3.1 비교](https://www.aifreeapi.com/en/posts/seedance-2-vs-kling-3-vs-sora-2-vs-veo-3)
- [Runway Gen-4.5 ELO 1위](https://www.pixazo.ai/blog/ai-video-generation-models-comparison)
- [Topaz vs Magnific 업스케일 비교](https://chasejarvis.com/blog/topaz-vs-magnific-best-ai-image-scaler/)
- [AI 업스케일러 가격 비교](https://medium.com/code-canvas/testing-different-upscalers-paid-vs-free-options-1260ab82d403)
- [Weavy 공식](https://www.weavy.ai/)
- [Weavy 노드 가이드](https://help.weavy.ai/en/articles/12292386-understanding-nodes)
