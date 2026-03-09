---
name: ai-ugc-content
description: "AI UGC(User Generated Content) 4K 극사실적 리뷰 영상 자동화 제작 전문 스킬. K-뷰티/이커머스 제품의 30초 인스타그램 릴스용 AI UGC 리뷰 영상을 n8n 워크플로우로 자동 생성하는 전체 파이프라인을 설계하고 디버깅한다. 이 스킬은 다음 키워드가 대화에 등장하면 반드시 트리거되어야 한다: ugc컨텐츠, UGC 컨텐츠, AI UGC, 극사실적 영상, 4K 리뷰 영상, AI 리뷰 영상, kie.ai, Kling, Typecast TTS, Suno BGM, AI 아바타 영상, 인스타 릴스 자동화, AI 광고 영상, 바디미스트 영상, K-뷰티 UGC, 영상 자동화 워크플로우, Director AI Agent, FFmpeg 인코딩, hyper-realistic video, AI avatar video generation. 또한 Weavy.ai 관련 영상 제작, 캐릭터 생성, 배경 합성 등을 언급할 때도 이 스킬을 참조해야 한다."
---

# AI UGC 4K 극사실적 리뷰 영상 자동화 제작 스킬

이 스킬은 FlowPilot의 핵심 파이프라인인 **AI UGC 30초 인스타그램 릴스 리뷰 영상**을 n8n으로 완전 자동화하는 데 필요한 모든 기술적 지식을 담고 있다. 수십 번의 실전 디버깅을 거쳐 검증된 노하우이므로, 워크플로우 설계/디버깅/최적화 시 이 문서를 반드시 먼저 참조하라.

---

## 1. 전체 아키텍처 개요

```
[Form 트리거] → [Director AI Agent (GPT-4o)] → [TTS (Typecast)] → [BGM (Suno via kie.ai)]
     → [영상 생성 (Kling via kie.ai)] → [FFmpeg 인코딩 (자막+BGM+4K)] → [Google Drive 업로드]
```

### 1.1 워크플로우 단계별 흐름

1. **Form 입력**: 제품명, 브랜드, 카테고리, 핵심 설명, 타겟, 셀링포인트, 톤/무드, CTA, AI모델 이미지 URL, 제품 이미지 URL
2. **Director AI Agent**: GPT-4o가 30초 타임라인(5~7 세그먼트) 생성 → 각 세그먼트별 TTS 텍스트 + Kling 비디오 프롬프트 + 자막 텍스트 출력
3. **TTS 생성**: Typecast API로 각 세그먼트별 음성 생성 → Cloudinary에 업로드 → 영구 URL 확보
4. **BGM 생성**: Suno AI (kie.ai API 경유)로 30초 배경음악 생성
5. **영상 클립 생성**: Kling AI Avatar Pro (kie.ai API 경유)로 각 세그먼트별 5초 영상 클립 생성
6. **FFmpeg 후처리**: SSH로 원격 서버에서 클립 합치기 + 자막(ASS) + BGM 믹싱 + H.264 인코딩
7. **업로드**: Google Drive에 최종 MP4 업로드

---

## 2. 핵심 API 스펙 및 설정값

### 2.1 kie.ai API

- **Base URL**: `https://kieai.erweimaxinxi.com`
- **인증**: `Authorization: Bearer 78e0997fd3c8399fc94ebfc323fd9994`
- **인증**: `Content-Type: application/json`
- **Rate Limit**: 20 requests / 10초, 최대 100+ 동시 태스크

#### Kling AI Avatar Pro (영상 생성)
- **Endpoint**: POST `/api/v1/jobs/createTask`
- **필수 Body**:
```json
{
  "type": "kling_ai_avatar_pro",
  "input": {
    "avatar_image": "https://cloudinary-url/w_1080,h_1920,c_fill,q_85,f_jpg/...",
    "prompt": "영문 프롬프트 (최대 5000자)",
    "duration": 5,
    "aspect_ratio": "9:16"
  }
}
```
- **제한 사항**: 이미지 최대 10MB, 프롬프트 최대 5000자, 영상 최대 15초
- **폴링**: GET `/api/v1/jobs/{taskId}` → `state: "completed"` 될 때까지 반복
- **결과**: `resultJson` 필드에 `{"resultUrls":["https://...mp4"]}` 형태로 반환

#### Suno AI (BGM 생성)
- **Endpoint**: POST `/api/v1/generate`
- **필수 Body**:
```json
{
  "type": "suno",
  "input": {
    "gpt_description_prompt": "calm ambient lo-fi background music for beauty product review, 30 seconds",
    "make_instrumental": true
  }
}
```
- **결과**: `response.sunoData[0].audioUrl` 또는 `.streamAudioUrl` 또는 `.sourceStreamAudioUrl`

### 2.2 Typecast TTS API

- **Endpoint**: POST `https://typecast.ai/api/speak`
- **인증**: `Authorization: Bearer {TYPECAST_API_KEY}`
- **Body**: `{"text": "TTS 텍스트", "lang": "ko", "actor_id": "...", "xapi_hd": true, "xapi_audio_format": "mp3"}`
- **응답**: 바이너리 MP3 (JSON URL이 아님!)
- **주의**: `no_tts: true`인 세그먼트(B-Roll 등)는 반드시 TTS 생성 전에 필터링할 것. 빈 `tts_text`를 Typecast에 보내면 "Invalid request data" 에러 발생.

### 2.3 Cloudinary 설정

- **Cloud Name**: `dqxo0fisu`
- **Upload URL**: `https://api.cloudinary.com/v1_1/dqxo0fisu/auto/upload`
- **Unsigned Preset**: `ml_default` (Cloudinary 대시보드 → Settings → Upload → Upload presets에서 unsigned 활성화 필수)
- **TTS 오디오 업로드**: Body Content Type = Form-Data, file = n8n Binary File (field: data), upload_preset = ml_default, folder = tts_audio
- **응답에서 URL 추출**: `response.secure_url` 사용 (tmpfiles.org 절대 사용 금지 - 1시간 만료됨)

#### Cloudinary 이미지 리사이즈 트릭
원본 URL의 `/upload/` 바로 뒤에 변환 파라미터를 삽입하면 실시간 리사이즈:
```
원본: https://res.cloudinary.com/dqxo0fisu/image/upload/v1772700024/filename.png
리사이즈: https://res.cloudinary.com/dqxo0fisu/image/upload/w_1080,h_1920,c_fill,q_85,f_jpg/v1772700024/filename.png
```
- `w_1080,h_1920`: 1080x1920 해상도 (9:16 세로)
- `c_fill`: 비율 맞춰 크롭
- `q_85`: JPEG 85% 품질
- `f_jpg`: PNG→JPG 변환 (6MB PNG → 200~400KB JPG)

이 변환은 Kling 524 타임아웃 방지에 필수. 10MB 이상 이미지를 그대로 보내면 거의 확실히 타임아웃 발생.

### 2.4 SSH 서버 (FFmpeg 실행)

- **호스트**: `158.247.245.248` (Vultr 24/7)
- **사용자**: `flowpilot`
- **n8n Credential**: `영상편집` (id: ABPEBX8IY94bjKl0)
- **Google Drive Credential**: OAuth2 (id: KZdgxMQHjZI5m4or), 업로드 폴더: `18J0GXmQkmGViJn1l3VS-PcV7qsNl80dS`

---

## 3. n8n 워크플로우 핵심 코드 노드들

### 3.1 BGM URL 저장 (Code - JavaScript)

BGM URL을 반드시 `staticData`에 저장해야 한다. JSON output만 하면 downstream 노드에서 접근 불가.

```javascript
const result = $input.first().json;
let bgmUrl = '';

if (result.data && result.data.response && result.data.response.sunoData) {
  const sunoData = result.data.response.sunoData;
  if (Array.isArray(sunoData) && sunoData.length > 0) {
    const track = sunoData[0];
    if (track.audioUrl && String(track.audioUrl).startsWith('http')) {
      bgmUrl = track.audioUrl;
    } else if (track.streamAudioUrl && String(track.streamAudioUrl).startsWith('http')) {
      bgmUrl = track.streamAudioUrl;
    } else if (track.sourceStreamAudioUrl && String(track.sourceStreamAudioUrl).startsWith('http')) {
      bgmUrl = track.sourceStreamAudioUrl;
    } else if (track.sourceAudioUrl && String(track.sourceAudioUrl).startsWith('http')) {
      bgmUrl = track.sourceAudioUrl;
    }
  }
}

if (!bgmUrl && result.response && result.response.sunoData) {
  const sunoData = result.response.sunoData;
  if (Array.isArray(sunoData) && sunoData.length > 0) {
    const track = sunoData[0];
    if (track.audioUrl && String(track.audioUrl).startsWith('http')) {
      bgmUrl = track.audioUrl;
    } else if (track.streamAudioUrl && String(track.streamAudioUrl).startsWith('http')) {
      bgmUrl = track.streamAudioUrl;
    }
  }
}

const staticData = $getWorkflowStaticData('global');
staticData.bgm_url = bgmUrl;

return [{ json: { bgm_url: bgmUrl, status: 'bgm_ready' } }];
```

**핵심**: `staticData.bgm_url = bgmUrl;` 이 한 줄이 빠지면 FFmpeg 단계에서 `has_bgm: false`가 되어 BGM 없이 영상이 만들어짐.

### 3.2 클립 다운로드 명령 생성 (Code - JavaScript)

heredoc 대신 base64 인코딩을 사용하여 ASS 자막과 clips.txt를 안전하게 전달:

```javascript
const staticData = $getWorkflowStaticData('global');
const allResults = staticData.all_results || [];
allResults.sort((a, b) => (a.segment_id || 0) - (b.segment_id || 0));

const commands = [];
for (let i = 0; i < allResults.length; i++) {
  const url = allResults[i].video_url || '';
  if (url) {
    commands.push('curl -L -s -o /tmp/clip_' + (i + 1) + '.mp4 "' + url + '"');
  }
}

const bgmUrl = staticData.bgm_url || '';
if (bgmUrl) {
  commands.push('curl -L -s -o /tmp/bgm.mp3 "' + bgmUrl + '"');
}

const assContent = staticData.ass_content || '';
if (assContent.trim().length > 0) {
  const assBase64 = Buffer.from(assContent, 'utf-8').toString('base64');
  commands.push('echo "' + assBase64 + '" | base64 -d > /tmp/subtitle.ass');
}

let fileList = '';
for (let i = 0; i < allResults.length; i++) {
  if (allResults[i].video_url) {
    fileList += "file '/tmp/clip_" + (i + 1) + ".mp4'\n";
  }
}
if (fileList) {
  const clipsBase64 = Buffer.from(fileList, 'utf-8').toString('base64');
  commands.push('echo "' + clipsBase64 + '" | base64 -d > /tmp/clips.txt');
}

return [{ json: { download_command: commands.join(' && '), clip_count: allResults.length, has_bgm: !!bgmUrl } }];
```

**왜 base64인가**: ASS 자막에 한국어, 특수문자, 줄바꿈이 포함되어 있어서 SSH heredoc(`<<'ASSEOF'`)이 깨짐. base64로 인코딩하면 어떤 내용이든 안전하게 전달 가능.

### 3.3 FFmpeg 명령 생성 (Code - JavaScript)

`-vf`와 `-filter_complex`는 동시에 사용할 수 없다는 것이 핵심:

```javascript
const staticData = $getWorkflowStaticData('global');
const hasBgm = $json.has_bgm || false;
const clipCount = $json.clip_count || 0;

let vfBase = 'scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2';

const assContent = staticData.ass_content || '';
const hasSubtitle = assContent.trim().length > 0;
if (hasSubtitle) {
  vfBase += ',ass=/tmp/subtitle.ass';
}

let cmd = '';
if (hasBgm) {
  cmd = 'ffmpeg -y -f concat -safe 0 -i /tmp/clips.txt -i /tmp/bgm.mp3';
  cmd += ' -filter_complex "';
  cmd += '[0:v]' + vfBase + '[vout];';
  cmd += '[1:a]volume=0.12,afade=t=in:d=1:st=0,afade=t=out:d=2:st=28[bgm]';
  cmd += '" -map "[vout]" -map "[bgm]"';
  cmd += ' -c:v libx264 -preset medium -crf 17 -pix_fmt yuv420p';
  cmd += ' -c:a aac -b:a 192k -shortest';
  cmd += ' -movflags +faststart /tmp/final_output.mp4';
} else {
  cmd = 'ffmpeg -y -f concat -safe 0 -i /tmp/clips.txt';
  cmd += ' -vf "' + vfBase + '"';
  cmd += ' -c:v libx264 -preset medium -crf 17 -pix_fmt yuv420p';
  cmd += ' -an';
  cmd += ' -movflags +faststart /tmp/final_output.mp4';
}

return [{ json: { ffmpeg_command: cmd, has_subtitle: hasSubtitle, has_bgm: hasBgm } }];
```

**핵심 규칙**:
- BGM 있을 때: `-filter_complex`에 비디오 필터(`[0:v]scale...`)와 오디오 필터(`[1:a]volume...`)를 모두 넣음
- BGM 없을 때: `-vf`로 비디오 필터만 적용 + `-an` (no audio) 플래그 필수
- Kling 영상에는 오디오 트랙이 없으므로 `-c:a aac` 단독 사용 시 FFmpeg code 1 에러 발생

### 3.4 TTS URL 추출 (Code - JavaScript)

Cloudinary 업로드 응답에서 URL 추출:

```javascript
const response = $input.first().json;
const loopItem = $('TTS 순차 루프').first().json;

let audioUrl = '';
if (response.secure_url) {
  audioUrl = response.secure_url;
} else if (response.url) {
  audioUrl = response.url.replace('http://', 'https://');
}

return [{
  json: {
    segment_id: loopItem.timeline_id || 0,
    timeline_id: loopItem.timeline_id || 0,
    tts_text: loopItem.tts_text || '',
    audio_url: audioUrl
  }
}];
```

**주의**: tmpfiles.org 형식(`response.data.url`)을 절대 사용하지 마라. Cloudinary는 `response.secure_url`이다.

### 3.5 영상 결과 폴링 (결과 집계 Code)

kie.ai의 `resultJson`은 문자열이므로 JSON.parse가 필요:

```javascript
const resultJson = $json.resultJson || '';
let videoUrl = '';

if (resultJson) {
  try {
    const resultData = JSON.parse(resultJson);
    if (resultData.resultUrls && resultData.resultUrls.length > 0) {
      videoUrl = resultData.resultUrls[0];
    }
  } catch (e) {
    // resultJson 파싱 실패
  }
}
```

---

## 4. 치명적 실수 방지 체크리스트

### 4.1 Kling 524 타임아웃 원인 및 해결

| 원인 | 해결 |
|------|------|
| 이미지 파일 크기 10MB 초과 | Cloudinary URL에 `w_1080,h_1920,c_fill,q_85,f_jpg` 변환 삽입 |
| 이미지 파일명에 한국어 포함 | Cloudinary에 영문 파일명으로 재업로드 |
| TTS 오디오 URL 만료 (tmpfiles.org) | Cloudinary에 TTS 업로드하여 영구 URL 확보 |
| kie.ai 과부하/Rate Limit | 폴링 무한루프 방지: 최대 재시도 10회 제한 |
| Pin된 데이터로 무한 폴링 | Pin 데이터 제거 + Wait 노드 15초 설정 + 재시도 카운터 |

### 4.2 n8n 일반 실수 방지

- `$getWorkflowStaticData('global')`에 값을 저장할 때는 반드시 `staticData.key = value;` 한 줄을 명시적으로 작성. JSON return만으로는 staticData에 저장되지 않음.
- heredoc(`<<'EOF'`)은 특수문자, 한국어, 줄바꿈에 취약. SSH로 파일 전달 시 항상 base64 인코딩 사용.
- `-vf`와 `-filter_complex`는 FFmpeg에서 동시 사용 불가. BGM이 있으면 filter_complex, 없으면 -vf.
- Kling 영상은 오디오 트랙이 없음 (sound: false). BGM 없이 인코딩할 때 반드시 `-an` 플래그 사용.
- n8n이 Docker에서 돌아가면 호스트의 `/tmp/` 파일에 Read/Write File 노드로 접근 불가. SSH 업로드 파이프라인 필수.

### 4.3 TTS 세그먼트 필터링

Director AI가 생성한 타임라인에서 `no_tts: true`인 세그먼트(B-Roll, 제품샷 등)를 TTS 생성 전에 반드시 필터링:

```javascript
// TTS 세그먼트 필터링 Code node
const items = $input.all();
const ttsItems = items.filter(item => {
  const json = item.json;
  return !json.no_tts && json.tts_text && json.tts_text.trim().length > 0;
});
return ttsItems;
```

빈 `tts_text`를 Typecast에 보내면 "Invalid request data" 에러 발생.

---

## 5. 극사실주의 프롬프트 엔지니어링

### 5.1 이미지 프롬프트 필수 블록 (Flux 2 Pro 등)

모든 인물 이미지 프롬프트 끝에 반드시 삽입:

```
hyper-realistic photorealistic photograph indistinguishable from real DSLR photo, ultra-detailed skin texture with visible pores and micro skin imperfections and fine peach fuzz hair and natural skin blemishes and subtle freckles, real human skin subsurface scattering with natural blood vessel undertones, individual hair strands visible with natural flyaway hairs and split ends, eyes with detailed iris fibers and natural eye moisture reflection and visible blood vessels in sclera, natural asymmetric facial features as real humans have, visible fingernail texture with natural nail ridges and cuticle detail, toenail with realistic keratin texture and natural imperfections, natural body hair on arms and legs visible under light, realistic ear canal detail and ear lobe texture, natural lip texture with visible lip lines and slight dryness, teeth with natural slight discoloration and realistic enamel reflection, shot on real camera sensor with natural lens aberration and chromatic fringing at edges, realistic depth of field with optical bokeh circles showing cat-eye effect at frame edges, natural film grain matching ISO setting, absolutely ZERO artificial smooth airbrushed plastic skin, ZERO uncanny valley effect, ZERO CGI render look, ZERO digital art style, ZERO perfect symmetry, must pass as genuine unedited photograph taken by professional photographer
```

### 5.2 비디오 프롬프트 필수 블록 (Kling 3, Veo 3.1 등)

모든 인물 비디오 프롬프트 끝에 반드시 삽입:

```
hyper-realistic footage indistinguishable from real cinema camera capture, ultra-detailed skin with visible pores and micro expressions and natural skin texture movement during motion, realistic hair physics with individual strands responding to movement and gravity and breeze naturally, natural subtle body micro-movements like breathing pulse and muscle tension shifts and weight distribution changes, realistic eye movement with natural blink rate and eye moisture and pupil dilation, fabric physics responding realistically to body movement and air and gravity, visible fingernail and toenail detail during hand and feet close-ups, natural body hair visible on arms catching light, realistic ear and neck skin texture during head turns, natural lip movement with realistic moisture and texture, natural motion blur consistent with real camera shutter speed, absolutely ZERO artificial smooth plastic skin, ZERO uncanny valley, ZERO AI-generated look, ZERO robotic stiff movement, must pass as genuine footage from professional cinema camera
```

### 5.3 제품 전용 프롬프트 (인물 없는 제품샷)

```
hyper-realistic product photography indistinguishable from real studio photograph, ultra-detailed material textures with visible surface micro-texture and natural light interaction, realistic glass or plastic refraction and reflection with visible internal light caustics, natural shadow falloff with realistic ambient occlusion and contact shadows, visible label printing texture and slight label edge lifting, realistic liquid meniscus visible through transparent container, dust particles visible on surface under studio light, shot on medium format camera with natural lens characteristics and shallow depth of field, absolutely ZERO CGI render look, ZERO 3D modeling aesthetic, must pass as genuine product photo from commercial studio shoot
```

### 5.4 절대 금지 키워드 (이미지+비디오 공통)

| 금지 키워드 | 교체 키워드 |
|------------|------------|
| smooth skin | textured skin with visible pores |
| perfect face | naturally asymmetric human face |
| beautiful lighting | 실제 카메라/렌즈 모델명 (Arri Alexa Mini LF 등) |
| high quality | photorealistic DSLR photograph 또는 cinema camera footage |
| 4K/8K 단독 사용 | 카메라 모델 + 렌즈 + ISO + 색온도와 함께 사용 |

### 5.5 프롬프트 체크리스트 (매번 확인)

- [ ] 실제 카메라 모델명 (Sony A7R V, Arri Alexa Mini LF, Canon EOS R5 등)
- [ ] 실제 렌즈 모델명 + 조리개값 (Sony FE 85mm f/1.4 GM 등)
- [ ] 색온도 K값 (3200K, 4500K, 5600K 등)
- [ ] ISO/필름그레인 수준 (ISO 800 natural film grain 등)
- [ ] "visible pores" 또는 "skin texture with pores" 포함 여부
- [ ] "hyper-realistic" 또는 "photorealistic" 포함 여부
- [ ] "ZERO AI look" 또는 "indistinguishable from real photo" 포함 여부

---

## 6. Weavy.ai 도구 선택 가이드

### 6.1 상황별 올바른 도구

| 상황 | 올바른 도구 | 잘못된 도구 (사용 금지) |
|------|------------|----------------------|
| 제품 원본 유지 + 배경 교체 | Compositor | Ideogram Replace Background (제품 재생성됨) |
| 배경이 이미 준비된 상태 | Compositor (두 이미지 합성) | Bria Replace Background (배경을 텍스트로 새로 생성) |
| 배경 없이 처음부터 생성 | Replace Background 또는 Bria | - |
| 제품 배경 제거(투명화) | SD3 Remove Background | Flux I2I (제품 왜곡됨) |
| 배경만 별도 생성 | Flux 2 Pro (Action: Generate Image, "NO product" 필수) | - |
| 키프레임 → 영상 변환 | Kling 3 (Action: Image to Video) 또는 Veo 3.1 (Action: Image to Video) | Kling o1 (구버전) |
| 인물+배경 합성 영상 | 합성 키프레임 → Kling 3/Veo 3.1 First Frame | Kling o1 Reference V2V |
| 기존 영상 부분 수정 | Kling o3 Edit Video | Kling o1 Edit (구버전) |
| 카메라 무빙 정밀 제어 | Kling Motion Control (dolly/pan/tilt/zoom/orbit) | - |
| 클립 합치기 | Video Concatenator (모든 씬 완성 후 마지막) | - |

### 6.2 Weavy 포트 색상 규칙

| 포트 색상 | 데이터 타입 | 연결 가능 대상 |
|-----------|------------|---------------|
| 녹색 | 이미지/비디오 | 같은 녹색 포트끼리만 |
| 빨간색 | 텍스트/프롬프트 | 같은 빨간색 포트끼리만 |
| 보라색 | 모델/파라미터 | 같은 보라색 포트끼리만 |

### 6.3 AI 영상 제작 10단계 프로세스

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

---

## 7. n8n 노드 가이드 필수 규칙

n8n 노드를 추가하라고 가이드할 때, 내부에 Operation/Action/Resource 드롭다운이 있는 노드라면, 반드시 **노드명 + 선택해야 할 Operation/Action명**을 함께 명시해야 한다.

**나쁜 예시 (금지)**:
> "Extract from File 노드를 추가하세요."

**좋은 예시 (필수)**:
> "Extract from File 노드를 추가하세요. 캔버스에 올린 후 열어서, **Action** 파라미터에서 **'Extract from text file'**을 선택하세요."

이 규칙은 Google Sheets, HTTP Request, Kling, Flux, 모든 노드에 동일하게 적용한다.

---

## 8. FFmpeg 후처리 파이프라인 (n8n → SSH → 업로드)

n8n이 Docker에서 실행될 때 호스트의 `/tmp/` 파일에 접근 불가하므로, 다음 파이프라인을 사용:

```
[SSH: FFmpeg 실행] → [SSH: tmpfiles.org 업로드] → [Code: URL 추출] → [HTTP Request: 바이너리 다운로드] → [Google Drive 업로드]
```

1. **SSH 영상 업로드 노드**: `curl -s -F "file=@/tmp/final_output.mp4" https://tmpfiles.org/api/v1/upload`
2. **영상 URL 추출 Code**: tmpfiles.org 응답에서 URL 파싱 후 `/dl/` 경로 삽입
3. **HTTP Request1**: GET 방식, Response Format = File로 바이너리 다운로드
4. **Google Drive 업로드**: 바이너리 데이터를 받아서 지정 폴더에 업로드

---

## 9. Director AI Agent 프롬프트 설계 원칙

Director AI Agent (GPT-4o)에게 전달하는 시스템 프롬프트는 다음 구조를 따라야 한다:

1. **역할 정의**: "You are a professional video director for 30-second Instagram Reels UGC review videos."
2. **출력 형식**: 5~7개 세그먼트의 JSON 타임라인 (각 세그먼트에 timeline_id, start_time, end_time, scene_type, tts_text, video_prompt, subtitle_text 포함)
3. **세그먼트 유형**: TALKING (얼굴+음성), BROLL (제품 클로즈업, no_tts: true), CTA (마무리)
4. **영상 프롬프트 규칙**: 영문 작성, 카메라 앵글/무빙 지정, 극사실주의 키워드 블록 포함
5. **TTS 텍스트 규칙**: 한국어, 자연스러운 구어체, 5초 내 발화 가능한 길이
6. **자막 텍스트 규칙**: TTS와 동일하되 적절히 줄바꿈 포함

---

## 10. 비용 구조 및 최적화

| 항목 | 단가 | 30초 영상 기준 |
|------|------|--------------|
| GPT-4o (Director) | ~$0.03/call | $0.03 |
| Typecast TTS | ~$0.01/segment | $0.05~0.07 |
| Suno BGM (kie.ai) | ~$0.05/track | $0.05 |
| Kling Avatar Pro (kie.ai) | ~$0.10/clip | $0.50~0.70 |
| Cloudinary | Free tier 충분 | $0 |
| SSH 서버 | 월 $6 고정 | - |
| **합계** | | **~$0.65~0.85/영상** |

---

## 11. 트러블슈팅 빠른 참조

| 증상 | 원인 | 해결 |
|------|------|------|
| `failCode: 524, failMsg: generate task timeout` | 이미지 너무 큼 / URL 한글 / rate limit | Cloudinary 리사이즈 + 영문 파일명 + 쿨다운 |
| `has_bgm: false` (BGM 분명히 있는데) | staticData에 bgm_url 미저장 | `staticData.bgm_url = bgmUrl;` 추가 |
| FFmpeg code 1 | 오디오 트랙 없는데 `-c:a` 사용 | `-an` 플래그 사용 |
| FFmpeg `-vf` + `-filter_complex` 충돌 | 동시 사용 불가 | BGM 있으면 filter_complex로 통합 |
| heredoc 깨짐 (`wanted 'ASSEOF'`) | 특수문자/한글 | base64 인코딩으로 교체 |
| TTS "Invalid request data" | 빈 tts_text 전송 | no_tts 세그먼트 필터링 |
| 최종 영상 읽기 No output | Docker/호스트 경로 불일치 | SSH 업로드 파이프라인 사용 |
| Cloudinary "Upload preset must be whitelisted" | ml_default unsigned 미활성화 | 대시보드에서 unsigned 활성화 |
| 774 items 무한 폴링 | Pin 데이터 + 재시도 제한 없음 | Pin 제거 + 최대 10회 재시도 카운터 |
