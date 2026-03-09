# Google Drive 자동 업로드 — 워크플로우 확장 가이드
## FlowPilot | 2026.03.05

---

## 목표

최종 결과 집계 노드 이후에 **자동으로** 아래 파일들을 Google Drive 지정 폴더에 업로드:

- 토킹 클립 영상 5개 (InfiniteTalk MP4)
- B-Roll 영상 5개 (Veo 3.1 MP4)
- 모델 이미지 1개 (Flux Kontext)
- (나중에 추가) 최종 편집 영상 1개

---

## 사전 준비: Google Drive OAuth2 크리덴셜 설정

Google Drive 노드를 사용하려면 먼저 n8n에 Google OAuth2 크리덴셜을 등록해야 한다.

### Step 1: Google Cloud Console에서 OAuth 클라이언트 생성

1. **Google Cloud Console** 접속: https://console.cloud.google.com
2. 왼쪽 메뉴 → **APIs & Services** → **Credentials**
3. 상단 **+ CREATE CREDENTIALS** → **OAuth client ID** 클릭
4. Application type → **Web application** 선택
5. Name: `n8n FlowPilot` (아무거나)
6. **Authorized redirect URIs** 에 아래 URL 추가:
   ```
   https://n8n.flowpilots.org/rest/oauth2-credential/callback
   ```
7. **Create** 클릭
8. **Client ID**와 **Client Secret** 복사해둔다

### Step 2: Google Drive API 활성화

1. Google Cloud Console → **APIs & Services** → **Library**
2. 검색: **Google Drive API**
3. **ENABLE** 클릭

### Step 3: n8n에서 크리덴셜 등록

1. n8n 열기 → 왼쪽 메뉴 → **Credentials**
2. **+ Add Credential** 클릭
3. 검색: **Google Drive OAuth2 API**
4. 선택하면 설정 화면이 나온다:
   - **Client ID**: Step 1에서 복사한 Client ID 붙여넣기
   - **Client Secret**: Step 1에서 복사한 Client Secret 붙여넣기
5. **Sign in with Google** 버튼 클릭 → Google 계정 로그인 → 권한 허용
6. 연결 완료되면 **Save** 클릭

---

## 추가할 노드: 총 4개

최종 결과 집계 노드 뒤에 아래 4개 노드를 순서대로 추가한다:

```
[최종 결과 집계]
     ↓
[1. 업로드 데이터 준비]  ← Code 노드 (URL + 파일명 리스트 생성)
     ↓
[2. 업로드 순차 루프]    ← SplitInBatches 노드 (batch=1)
     ↓ (loop)
[3. 영상 다운로드]       ← HTTP Request 노드 (URL → 바이너리)
     ↓
[4. Google Drive 업로드] ← Google Drive 노드 (바이너리 → Drive)
     ↓
[2. 업로드 순차 루프]로 되돌아감 (루프 반복)
```

---

## 노드 1: 업로드 데이터 준비 (Code 노드)

### 추가 방법

1. n8n 캔버스에서 **최종 결과 집계** 노드 오른쪽의 빈 공간을 클릭
2. **+** 버튼 또는 **Tab** 키
3. 검색: **Code**
4. **Code** 노드 선택하여 추가
5. 노드 이름을 `업로드 데이터 준비`로 변경

### 연결

**최종 결과 집계** → **업로드 데이터 준비**

### 코드 (전체 — 복사용)

```javascript
const result = $input.first().json;
const brand = result.brand || 'KINOHI';
const product = result.product || 'bodymist';
const timestamp = new Date().toISOString().slice(0, 10).replace(/-/g, '');

const uploadItems = [];

// 토킹 클립 5개
if (result.talking_clips && result.talking_clips.length > 0) {
  result.talking_clips.forEach((clip, i) => {
    if (clip.video_url && clip.video_url.startsWith('http')) {
      uploadItems.push({
        url: clip.video_url,
        fileName: brand + '_' + timestamp + '_talking_' + (i + 1) + '_' + (clip.section || 'clip') + '.mp4',
        type: 'talking_clip',
        section: clip.section || '',
        segment_id: clip.segment_id || (i + 1)
      });
    }
  });
}

// B-Roll 클립 5개
if (result.broll_clips && result.broll_clips.length > 0) {
  result.broll_clips.forEach((clip, i) => {
    if (clip.video_url && clip.video_url.startsWith('http')) {
      uploadItems.push({
        url: clip.video_url,
        fileName: brand + '_' + timestamp + '_broll_' + (i + 1) + '_' + (clip.broll_name || 'clip').replace(/\s+/g, '_') + '.mp4',
        type: 'broll_clip',
        section: clip.section || '',
        broll_name: clip.broll_name || ''
      });
    }
  });
}

// 모델 이미지 (있으면)
const modelImageUrl = result.model_image_url || '';
if (modelImageUrl && modelImageUrl.startsWith('http')) {
  const ext = modelImageUrl.includes('.png') ? '.png' : '.jpg';
  uploadItems.push({
    url: modelImageUrl,
    fileName: brand + '_' + timestamp + '_model_image' + ext,
    type: 'model_image',
    section: 'all'
  });
}

if (uploadItems.length === 0) {
  throw new Error('업로드할 파일이 없습니다. 모든 video_url이 비어있습니다.');
}

return uploadItems.map(item => ({ json: item }));
```

### 이 코드가 하는 일

- 최종 결과 집계의 `talking_clips`와 `broll_clips`에서 URL이 있는 항목만 추출
- 각 파일에 체계적인 이름을 부여: `KINOHI_20260305_talking_1_hook.mp4`
- 모델 이미지도 있으면 포함
- 결과: 최대 11개 item (토킹 5 + B-Roll 5 + 이미지 1)

---

## 노드 2: 업로드 순차 루프 (SplitInBatches 노드)

### 추가 방법

1. **+** 버튼 또는 **Tab** 키
2. 검색: **Split In Batches**
3. **Split In Batches** 노드 선택하여 추가
4. 노드 이름을 `업로드 순차 루프`로 변경

### 설정

1. 노드를 열고 **Batch Size** → **1** 입력

### 연결

**업로드 데이터 준비** → **업로드 순차 루프**

루프 구조:
- **Port 0 (done)**: 아무것도 연결하지 않아도 됨 (모든 업로드 완료 후 종료)
- **Port 1 (loop)**: **영상 다운로드** 노드에 연결

---

## 노드 3: 영상 다운로드 (HTTP Request 노드)

### 추가 방법

1. **+** 버튼 또는 **Tab** 키
2. 검색: **HTTP Request**
3. **HTTP Request** 노드 선택하여 추가
4. 노드 이름을 `영상 다운로드`로 변경

### 설정

1. **Method**: GET
2. **URL**: 아래 expression 붙여넣기
   ```
   ={{ $json.url }}
   ```
3. **Options** 클릭 → **Add Option** → **Response** 추가
4. **Response Format** → **File** 선택
5. **Put Output in Field** → `data` (기본값 유지)

### 연결

**업로드 순차 루프** (Port 1: loop) → **영상 다운로드**

### 이 노드가 하는 일

- 영상 URL (예: `https://tempfile.aiquickdraw.com/v/47ff7cb...mp4`)에 GET 요청
- 응답을 **바이너리 파일(File)**로 받음
- 받은 바이너리 데이터가 `data` 필드에 저장됨
- 이 바이너리 데이터를 다음 Google Drive 노드가 업로드함

---

## 노드 4: Google Drive 업로드 (Google Drive 노드)

### 추가 방법

1. **+** 버튼 또는 **Tab** 키
2. 검색: **Google Drive**
3. **Google Drive** 노드 선택하여 추가
4. 노드 이름을 `Google Drive 업로드`로 변경

### 설정 (순서대로)

1. **Credential to connect with**: 드롭다운에서 사전 준비에서 만든 **Google Drive OAuth2** 크리덴셜 선택

2. **Resource**: **File** 선택 (기본값)

3. **Operation**: **Upload** 선택

4. **Input Data Field Name**: `data`
   - 이것은 이전 HTTP Request 노드에서 바이너리를 저장한 필드명과 동일해야 한다

5. **Parent Drive**: **My Drive** 선택 (또는 공유 드라이브가 있으면 해당 드라이브 선택)

6. **Parent Folder**: 업로드할 대상 폴더 선택
   - **From list** 모드를 선택하면 드롭다운에서 폴더를 직접 선택할 수 있다
   - 또는 **By ID** 모드를 선택하고 Google Drive 폴더 ID를 직접 입력
   - **폴더 ID 찾는 법**: Google Drive에서 해당 폴더를 열면 주소창에 `https://drive.google.com/drive/folders/1aBcDeFgHiJkLmNoPqRsT` 형태의 URL이 보인다. 여기서 `1aBcDeFgHiJkLmNoPqRsT` 부분이 폴더 ID다

7. **Options** → **Add Option** → **File Name** 클릭 → 아래 expression 붙여넣기:
   ```
   ={{ $('업로드 순차 루프').item.json.fileName }}
   ```

### 연결

**영상 다운로드** → **Google Drive 업로드**

**Google Drive 업로드** → **업로드 순차 루프** (루프 반복)

---

## 전체 연결도 (완성 후)

```
[최종 결과 집계]
     ↓
[업로드 데이터 준비]  ← Code: URL + 파일명 리스트 생성 (최대 11개 item)
     ↓
[업로드 순차 루프]  ← SplitInBatches (batch=1)
     ├── Port 0 (done): 종료 (전체 업로드 완료)
     └── Port 1 (loop):
              ↓
         [영상 다운로드]  ← HTTP GET, responseFormat: File
              ↓
         [Google Drive 업로드]  ← Resource: File, Operation: Upload
              ↓
         [업로드 순차 루프]로 되돌아감
```

---

## 연결 상세표

| 출발 노드 | → | 도착 노드 | 포트 |
|----------|---|----------|------|
| 최종 결과 집계 | → | 업로드 데이터 준비 | main[0] → main[0] |
| 업로드 데이터 준비 | → | 업로드 순차 루프 | main[0] → main[0] |
| 업로드 순차 루프 | → (Port 1: loop) | 영상 다운로드 | main[1] → main[0] |
| 영상 다운로드 | → | Google Drive 업로드 | main[0] → main[0] |
| Google Drive 업로드 | → | 업로드 순차 루프 | main[0] → main[0] (루프 반복) |

---

## Google Drive 폴더 구조 제안

Google Drive에 아래 구조로 폴더를 미리 만들어두면 체계적으로 관리할 수 있다:

```
FlowPilot/
  └── AI_UGC_영상/
       └── KINOHI/
            ├── 2026-03-05_batch_001/
            │    ├── KINOHI_20260305_talking_1_hook.mp4
            │    ├── KINOHI_20260305_talking_2_problem.mp4
            │    ├── KINOHI_20260305_talking_3_discovery.mp4
            │    ├── KINOHI_20260305_talking_4_solution.mp4
            │    ├── KINOHI_20260305_talking_5_cta.mp4
            │    ├── KINOHI_20260305_broll_1_제품_클로즈업.mp4
            │    ├── KINOHI_20260305_broll_2_...mp4
            │    ├── KINOHI_20260305_broll_3_...mp4
            │    ├── KINOHI_20260305_broll_4_...mp4
            │    ├── KINOHI_20260305_broll_5_...mp4
            │    └── KINOHI_20260305_model_image.jpg
            └── 2026-03-06_batch_002/
                 └── ...
```

**폴더를 자동으로 생성하고 싶으면** Google Drive 노드를 하나 더 추가할 수 있다:
- Resource: **Folder**, Operation: **Create**
- 업로드 데이터 준비 앞에 배치
- 이건 나중에 필요하면 추가하자

---

## 실행 후 예상 결과

워크플로우 실행이 완료되면:

1. 최종 결과 집계에서 URL이 있는 영상들이 자동으로 식별됨
2. 각 영상이 하나씩 순차적으로 다운로드됨
3. 다운로드된 바이너리가 Google Drive 지정 폴더에 업로드됨
4. 파일명이 `KINOHI_20260305_talking_1_hook.mp4` 형식으로 자동 생성됨

---

## 트러블슈팅

### 에러: "The provided credentials are not valid"

→ Google OAuth2 크리덴셜이 만료됐거나 잘못 설정됐다. n8n Credentials에서 해당 크리덴셜을 열고 **Sign in with Google** 버튼을 다시 클릭한다.

### 에러: "File not found" 또는 HTTP 404 (영상 다운로드 노드)

→ 영상 URL이 만료됐다. kie.ai의 tempfile URL은 보통 24~72시간 유효하다. 최종 결과 집계 실행 후 **즉시** Google Drive 업로드를 해야 한다.

### 에러: "Insufficient permissions" (Google Drive 업로드 노드)

→ OAuth 권한 설정에서 Google Drive 쓰기 권한이 빠졌다. Google Cloud Console → OAuth consent screen에서 `https://www.googleapis.com/auth/drive.file` 스코프가 포함되어 있는지 확인한다.

### 업로드는 되지만 파일명이 이상할 때

→ Google Drive 업로드 노드의 **Options → File Name** expression을 확인:
```
={{ $('업로드 순차 루프').item.json.fileName }}
```
`$('업로드 순차 루프')`의 노드명이 실제 워크플로우와 일치하는지 확인한다.

### 파일 크기가 너무 크면 (타임아웃)

→ 영상 다운로드 노드에서 **Options** → **Timeout** 을 **120000** (120초)로 설정한다. 큰 MP4 파일 다운로드에 시간이 걸릴 수 있다.

---

## 체크리스트

### 사전 준비
- [ ] Google Cloud Console에서 OAuth 클라이언트 ID/Secret 생성
- [ ] Google Drive API 활성화
- [ ] n8n Credentials에 Google Drive OAuth2 등록 및 연결 테스트
- [ ] Google Drive에 업로드 대상 폴더 생성

### 노드 추가
- [ ] **업로드 데이터 준비** (Code 노드) 추가 + 코드 붙여넣기
- [ ] **업로드 순차 루프** (SplitInBatches 노드) 추가, Batch Size: 1
- [ ] **영상 다운로드** (HTTP Request 노드) 추가, GET, URL: `{{ $json.url }}`, Response Format: File
- [ ] **Google Drive 업로드** (Google Drive 노드) 추가, Resource: File, Operation: Upload

### 연결
- [ ] 최종 결과 집계 → 업로드 데이터 준비
- [ ] 업로드 데이터 준비 → 업로드 순차 루프
- [ ] 업로드 순차 루프 (Port 1) → 영상 다운로드
- [ ] 영상 다운로드 → Google Drive 업로드
- [ ] Google Drive 업로드 → 업로드 순차 루프 (루프 반복)

### 테스트
- [ ] 워크플로우 저장
- [ ] 테스트 실행
- [ ] Google Drive 폴더에 파일이 올라왔는지 확인
- [ ] 파일명이 정상인지 확인
- [ ] 영상이 정상 재생되는지 확인
