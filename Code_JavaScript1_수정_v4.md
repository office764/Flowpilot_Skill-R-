# Code in JavaScript1 노드 — 필드 매핑 수정 v4

## 문제 진단: 현재 코드가 존재하지 않는 필드를 읽고 있다

### 현재 Code 노드가 읽으려는 필드 (OLD — 이전 스키마)

| 코드에서 읽는 필드명 | 새 스키마에 존재? | 결과 |
|---------------------|-----------------|------|
| `tl.start_sec` | 없음 | **undefined (빈 값)** |
| `tl.end_sec` | 없음 | **undefined (빈 값)** |
| `tl.scene_description` | 없음 | **undefined (빈 값)** |
| `tl.emotion` | 없음 | **undefined (빈 값)** |
| `tl.camera_angle` | 없음 | **undefined (빈 값)** |
| `tl.lighting_direction` | 없음 | **빈 문자열 (fallback '')** |
| `tl.physical_actions` | 없음 | **undefined (빈 값)** |
| `tl.hand_positions` | 없음 | **undefined (빈 값)** |
| `tl.moisture_state` | 없음 | **undefined (빈 값)** |
| `tl.broll_type` | 없음 | **undefined (빈 값)** |
| `tl.tts_text` | **있음** | 정상 출력 |
| `tl.tts_emotion` | **있음** | 정상 출력 |

### 새 스키마에 있는데 코드가 안 읽는 필드 (치명적 누락)

| 새 스키마 필드명 | 역할 | 코드에서 읽는가? |
|-----------------|------|-----------------|
| `tl.infinitetalk_prompt` | InfiniteTalk 영상 생성 프롬프트 | **안 읽음 — 누락!** |
| `tl.broll_prompt` | Veo 3.1 B-Roll 영상 생성 프롬프트 | **안 읽음 — 누락!** |
| `tl.broll_name` | B-Roll 파일명 | **안 읽음 — 누락!** |
| `tl.time_range` | 시간 범위 (0-6s 등) | **안 읽음 — 누락!** |

**결론**: 가장 중요한 `infinitetalk_prompt`와 `broll_prompt`가 Code 노드를 통과하면서 **완전히 사라지고 있다.**
그래서 출력에서 "빈 느낌"이 나는 것이다. 실제로 비어 있다.

---

## 수정된 전체 코드 (복사하여 Code in JavaScript1 노드에 붙여넣기)

아래 코드를 `Code in JavaScript1` 노드의 JavaScript 필드에 **전체 교체**로 붙여넣는다.

```javascript
const directorOutput = $input.first().json.output || $input.first().json;
const scriptData = $('대본 파싱').first().json;

let plan;
try {
  if (typeof directorOutput === 'string') {
    const jsonMatch = directorOutput.match(/\{[\s\S]*\}/);
    plan = JSON.parse(jsonMatch[0]);
  } else {
    plan = directorOutput;
  }
} catch(e) {
  throw new Error('디렉터 AI 출력 파싱 실패: ' + e.message);
}

const timelines = plan.timelines || [];
const modelImagePrompt = plan.model_image_prompt || '';

return timelines.map(tl => ({
  json: {
    timeline_id: tl.timeline_id,
    section: tl.section,
    time_range: tl.time_range,
    infinitetalk_prompt: tl.infinitetalk_prompt,
    broll_prompt: tl.broll_prompt,
    broll_name: tl.broll_name,
    tts_text: tl.tts_text,
    tts_emotion: tl.tts_emotion,
    model_image_prompt: modelImagePrompt,
    brand: scriptData.brand,
    product: scriptData.product,
    price: scriptData.price || '',
    sensory: scriptData.sensory || '',
    full_script: scriptData.script.full_script || ''
  }
}));
```

---

## 변경 사항 상세 비교

### 삭제한 필드 (새 스키마에 존재하지 않음)

| 삭제된 코드 라인 | 이유 |
|-----------------|------|
| `start_sec: tl.start_sec,` | 새 스키마에 없음. `time_range`로 대체됨 |
| `end_sec: tl.end_sec,` | 새 스키마에 없음. `time_range`로 대체됨 |
| `scene_description: tl.scene_description,` | 새 스키마에 없음. `infinitetalk_prompt`가 장면 묘사를 포함 |
| `emotion: tl.emotion,` | 새 스키마에 없음. `tts_emotion`이 감정을 담당 |
| `camera_angle: tl.camera_angle,` | 새 스키마에 없음. 각 프롬프트 내부에 카메라 정보 포함 |
| `lighting_direction: tl.lighting_direction \|\| '',` | 새 스키마에 없음. 각 프롬프트 내부에 조명 정보 포함 |
| `physical_actions: tl.physical_actions,` | 새 스키마에 없음. `infinitetalk_prompt`에 동작 묘사 포함 |
| `hand_positions: tl.hand_positions,` | 새 스키마에 없음. `infinitetalk_prompt`에 포함 |
| `moisture_state: tl.moisture_state,` | 새 스키마에 없음. `broll_prompt`에 포함 |
| `broll_type: tl.broll_type,` | 새 스키마에 없음. `broll_name`으로 대체됨 |
| `global_visual_rules: globalRules,` | 새 스키마에 없음. 각 프롬프트 내부에 규칙 포함 |
| `camera_setup: cameraSetup,` | 새 스키마에 없음. 각 프롬프트 내부에 카메라 설정 포함 |

### 추가한 필드 (새 스키마의 핵심 필드)

| 추가된 코드 라인 | 역할 |
|-----------------|------|
| `time_range: tl.time_range,` | "0-6s", "6-12s" 등 시간 범위 |
| `infinitetalk_prompt: tl.infinitetalk_prompt,` | InfiniteTalk API에 전달할 영상 프롬프트 (가장 중요!) |
| `broll_prompt: tl.broll_prompt,` | Veo 3.1 API에 전달할 B-Roll 영상 프롬프트 (가장 중요!) |
| `broll_name: tl.broll_name,` | B-Roll 클립의 파일명 식별자 |

### 유지한 필드

| 유지된 코드 라인 | 이유 |
|-----------------|------|
| `timeline_id: tl.timeline_id,` | 타임라인 순서 식별 |
| `section: tl.section,` | 섹션명 (hook, problem 등) |
| `tts_text: tl.tts_text,` | Typecast TTS 텍스트 |
| `tts_emotion: tl.tts_emotion,` | Typecast TTS 감정 태그 |
| `model_image_prompt: modelImagePrompt,` | Flux Kontext 모델 이미지 프롬프트 |
| `brand: scriptData.brand,` | 브랜드명 |
| `product: scriptData.product,` | 제품명 |
| `price: scriptData.price \|\| '',` | 가격 |
| `sensory: scriptData.sensory \|\| '',` | 감각 묘사 텍스트 |
| `full_script: scriptData.script.full_script \|\| ''` | 전체 대본 |

---

## 수정 후 예상 OUTPUT (5 items)

Code 노드 실행 후 각 item은 다음 필드를 모두 갖고 있어야 한다:

```
Item 1 (hook):
  timeline_id: 1
  section: "hook"
  time_range: "0-6s"
  infinitetalk_prompt: "Camera: iPhone 15 Pro, Lens: 26mm f/1.6 with natural depth of field, ISO: 200, Color Temperature: 5200K, Format: 4K UHD. Young Korean woman, 24, standing in bright modern bathroom..."  ← 이 필드가 채워져야 정상
  broll_prompt: "Camera: RED Komodo 6K, Lens: Canon RF 35mm f/1.8 STM MACRO IS, Aperture: f/2.8, ISO: 250, Color Temperature: 5300K..."  ← 이 필드가 채워져야 정상
  broll_name: "제품 클로즈업"  ← 이 필드가 채워져야 정상
  tts_text: "야 나 살냄새 좋다는 애기 진짐 많이 듣거든?"
  tts_emotion: "excited"
  model_image_prompt: "UGC selfie of a natural Korean woman in her late 20s..."
  brand: "KINOHI"
  product: "키노히 바디미스트(santal)"
  price: "30,000원"
  sensory: "처음 뿌리면 톡 쏘듯 상쾌하고..."
  full_script: "야 나 살냄새 좋다는 애기 진짜 많이 듣거든?..."

Item 2 (problem): 같은 구조, section: "problem", time_range: "6-12s"
Item 3 (discovery): 같은 구조, section: "discovery", time_range: "12-18s"
Item 4 (solution): 같은 구조, section: "solution", time_range: "18-24s"
Item 5 (cta): 같은 구조, section: "cta", time_range: "24-30s"
```

---

## 적용 방법

1. `Code in JavaScript1` 노드를 더블클릭하여 연다
2. JavaScript 필드의 기존 코드를 **전체 선택 (Ctrl+A)** 한다
3. 위 "수정된 전체 코드" 섹션의 코드를 **붙여넣기 (Ctrl+V)** 한다
4. 우측 상단 **Execute step** 버튼을 클릭한다
5. OUTPUT에서 5개 item 모두 `infinitetalk_prompt`와 `broll_prompt`가 채워져 있는지 확인한다

---

## 주의: 이 Code 노드 이후 연결된 노드들도 확인 필요

이 Code 노드의 출력 필드명이 바뀌었기 때문에, 이후에 연결된 노드들(예: InfiniteTalk HTTP Request, Veo 3.1 HTTP Request, Typecast HTTP Request 등)에서 참조하는 필드명도 맞춰야 한다.

예를 들어:
- InfiniteTalk HTTP Request 노드가 `{{ $json.scene_description }}`을 참조하고 있다면 → `{{ $json.infinitetalk_prompt }}`로 변경해야 함
- Veo 3.1 HTTP Request 노드가 `{{ $json.broll_type }}`을 참조하고 있다면 → `{{ $json.broll_prompt }}`로 변경해야 함

이 부분은 워크플로우의 나머지 노드를 확인한 후 별도로 안내할 수 있다.
