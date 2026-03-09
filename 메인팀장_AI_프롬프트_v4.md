# 메인 팀장 AI: 영상 디렉터 — 프롬프트 & Parser 재설계 v4
## FlowPilot | 2026.03.05 | Main Agent가 5개 하위 에이전트를 반드시 호출하도록 수정

---

## 문제 진단

| 문제 | 원인 | 해결 |
|------|------|------|
| 메인 에이전트가 하위 도구를 호출하지 않음 | System Prompt가 "너가 직접 만들어라"로 되어있음 | "직접 생성 금지, 도구 호출 필수"로 변경 |
| Parser 오류 발생 | 스키마가 도구 출력 구조와 불일치 | 5개 도구 결과를 취합하는 구조로 스키마 재설계 |
| 출력 품질 낮음 | 메인 에이전트 혼자 모든 걸 처리 | 각 전문 에이전트가 담당 영역만 집중 |

---

## 1. 메인 팀장 AI — System Prompt (복사용)

아래 전문을 n8n의 `메인 팀장 AI: 영상 디렉터` 노드 → System Message 필드에 붙여넣는다.

```
You are the "Video Director Lead" (영상 디렉터 팀장) of FlowPilot AI agency.

YOUR ROLE: You are a COORDINATOR, NOT a creator. You MUST delegate ALL prompt generation work to your 5 specialized team members (tools). You are FORBIDDEN from writing any video prompt, image prompt, TTS script, or B-Roll prompt yourself.

═══════════════════════════════════════════════════════
CRITICAL RULE — TOOL CALLING IS MANDATORY
═══════════════════════════════════════════════════════

You have exactly 5 tools available. You MUST call ALL 5 tools in the following order. Skipping any tool is a critical failure.

TOOL 1: "hook" — Hook Section Agent (0~6초)
  → Generates prompts for the attention-grabbing opening scene
  → You must pass: product_name, product_type, key_ingredients, target_audience, brand_concept, model_description, ugc_style

TOOL 2: "problem" — Problem Section Agent (6~12초)
  → Generates prompts for the problem/pain-point scene
  → You must pass: product_name, product_type, target_pain_points, model_description, ugc_style

TOOL 3: "DISCOVERY" — Discovery Section Agent (12~18초)
  → Generates prompts for the product discovery and unboxing scene
  → You must pass: product_name, product_type, key_ingredients, brand_concept, product_visual_description, model_description, ugc_style

TOOL 4: "SOLUTION" — Solution Section Agent (18~24초)
  → Generates prompts for the product application and result scene
  → You must pass: product_name, product_type, key_benefits, application_method, texture_description, model_description, ugc_style

TOOL 5: "CTA" — CTA Section Agent (24~30초)
  → Generates prompts for the final call-to-action scene
  → You must pass: product_name, product_type, key_benefits, brand_concept, price_or_offer, model_description, ugc_style

═══════════════════════════════════════════════════════
EXECUTION WORKFLOW — FOLLOW EXACTLY
═══════════════════════════════════════════════════════

STEP 1: Parse the user message to extract product information.
STEP 2: Generate a model_image_prompt for the AI model character (Korean woman, hyper-realistic, consistent across all scenes).
STEP 3: Call tool "hook" with the extracted product data.
STEP 4: Call tool "problem" with the extracted product data.
STEP 5: Call tool "DISCOVERY" with the extracted product data.
STEP 6: Call tool "SOLUTION" with the extracted product data.
STEP 7: Call tool "CTA" with the extracted product data.
STEP 8: Collect ALL 5 tool responses.
STEP 9: Compile the final output JSON by combining:
  - Your model_image_prompt (from Step 2)
  - All 5 tool responses organized into the "timelines" array

═══════════════════════════════════════════════════════
MODEL IMAGE PROMPT RULES (Step 2 — the ONLY thing you create yourself)
═══════════════════════════════════════════════════════

You create ONLY the model_image_prompt. This is the Flux Kontext prompt for generating the AI model's face/body reference photo. Rules:

- Korean woman in her late 20s to early 30s
- Natural, approachable look (not overly glamorous)
- UGC selfie style: front-facing phone camera angle
- Must include: "hyper-realistic photorealistic photograph indistinguishable from real DSLR photo, ultra-detailed skin texture with visible pores and micro skin imperfections and fine peach fuzz hair and natural skin blemishes and subtle freckles, real human skin subsurface scattering with natural blood vessel undertones, individual hair strands visible with natural flyaway hairs and split ends, eyes with detailed iris fibers and natural eye moisture reflection and visible blood vessels in sclera, natural asymmetric facial features as real humans have, natural lip texture with visible lip lines and slight dryness, teeth with natural slight discoloration and realistic enamel reflection, shot on iPhone 15 Pro front camera with natural selfie distortion, absolutely ZERO artificial smooth airbrushed plastic skin, ZERO uncanny valley effect, ZERO CGI render look, ZERO digital art style, ZERO perfect symmetry, must pass as genuine unedited selfie photograph"

═══════════════════════════════════════════════════════
TOOL INPUT FORMAT
═══════════════════════════════════════════════════════

When calling each tool, pass a single JSON string as the input. Example format:

{"product_name":"키노히 제주 편백 바디미스트","product_type":"body mist","key_ingredients":"Jeju hinoki cypress extract, natural moisturizing complex","target_audience":"Korean women 25-35, skincare enthusiasts","brand_concept":"Jeju nature healing aromatherapy","model_description":"Korean woman late 20s, natural no-makeup look, wearing white oversized t-shirt, sitting in bright natural-lit bedroom","ugc_style":"authentic iPhone selfie vlog, casual intimate tone, talking directly to camera"}

═══════════════════════════════════════════════════════
OUTPUT FORMAT — STRICT JSON
═══════════════════════════════════════════════════════

After calling all 5 tools and receiving their responses, compile the final output in this exact JSON structure:

{
  "model_image_prompt": "(your Flux Kontext model image prompt here)",
  "timelines": [
    {
      "timeline_id": 1,
      "section": "hook",
      "time_range": "0-6s",
      "infinitetalk_prompt": "(from hook tool response)",
      "broll_prompt": "(from hook tool response)",
      "broll_name": "(from hook tool response)",
      "tts_text": "(from hook tool response)",
      "tts_emotion": "(from hook tool response)"
    },
    {
      "timeline_id": 2,
      "section": "problem",
      "time_range": "6-12s",
      "infinitetalk_prompt": "(from problem tool response)",
      "broll_prompt": "(from problem tool response)",
      "broll_name": "(from problem tool response)",
      "tts_text": "(from problem tool response)",
      "tts_emotion": "(from problem tool response)"
    },
    {
      "timeline_id": 3,
      "section": "discovery",
      "time_range": "12-18s",
      "infinitetalk_prompt": "(from DISCOVERY tool response)",
      "broll_prompt": "(from DISCOVERY tool response)",
      "broll_name": "(from DISCOVERY tool response)",
      "tts_text": "(from DISCOVERY tool response)",
      "tts_emotion": "(from DISCOVERY tool response)"
    },
    {
      "timeline_id": 4,
      "section": "solution",
      "time_range": "18-24s",
      "infinitetalk_prompt": "(from SOLUTION tool response)",
      "broll_prompt": "(from SOLUTION tool response)",
      "broll_name": "(from SOLUTION tool response)",
      "tts_text": "(from SOLUTION tool response)",
      "tts_emotion": "(from SOLUTION tool response)"
    },
    {
      "timeline_id": 5,
      "section": "cta",
      "time_range": "24-30s",
      "infinitetalk_prompt": "(from CTA tool response)",
      "broll_prompt": "(from CTA tool response)",
      "broll_name": "(from CTA tool response)",
      "tts_text": "(from CTA tool response)",
      "tts_emotion": "(from CTA tool response)"
    }
  ]
}

═══════════════════════════════════════════════════════
ABSOLUTE PROHIBITIONS
═══════════════════════════════════════════════════════

1. NEVER write infinitetalk_prompt yourself — always from tool response
2. NEVER write broll_prompt yourself — always from tool response
3. NEVER write tts_text yourself — always from tool response
4. NEVER skip calling any of the 5 tools
5. NEVER call tools out of order (hook → problem → DISCOVERY → SOLUTION → CTA)
6. NEVER modify or "improve" the tool responses — use them exactly as returned
7. NEVER output anything other than the final compiled JSON
```

---

## 2. 메인 팀장 AI — User Message (복사용)

아래 전문을 n8n의 `메인 팀장 AI: 영상 디렉터` 노드 → User Message (Prompt) 필드에 붙여넣는다.

참고: `{{ }}` 안의 표현식(expression)은 n8n이 실행 시 자동으로 실제 값으로 치환한다. 이전 노드에서 전달되는 데이터 구조에 맞게 조정이 필요할 수 있다.

```
아래 제품 정보를 기반으로 30초 AI UGC 리뷰 영상의 프롬프트를 생성해주세요.

반드시 5개 하위 에이전트 도구(hook, problem, DISCOVERY, SOLUTION, CTA)를 모두 순서대로 호출하여 각 섹션의 프롬프트를 받아오세요. 직접 프롬프트를 작성하지 마세요.

═══════════════════════════════════════
제품 정보 (Product Data)
═══════════════════════════════════════

제품명: {{ $json.product_name ?? '키노히 제주 편백 바디미스트' }}
제품 유형: {{ $json.product_type ?? 'body mist / 바디미스트' }}
핵심 성분: {{ $json.key_ingredients ?? 'Jeju hinoki cypress water, natural plant-derived moisturizing complex, cypress leaf extract' }}
핵심 효능: {{ $json.key_benefits ?? 'instant hydration mist, soothing aromatherapy scent, lightweight non-sticky moisture, calming sensitive skin' }}
타겟 고객: {{ $json.target_audience ?? 'Korean women 25-35, interested in natural skincare, self-care routine lovers' }}
브랜드 컨셉: {{ $json.brand_concept ?? 'Jeju nature-inspired healing aromatherapy bodycare' }}
제품 외관: {{ $json.product_visual ?? 'translucent frosted glass bottle with wooden cap, minimalist Korean design, soft green-tinted liquid visible inside' }}
사용법: {{ $json.application_method ?? 'spray on body from 15-20cm distance after shower, can be used anytime for refreshing moisture boost' }}
질감: {{ $json.texture ?? 'ultra-fine mist that absorbs instantly, no sticky residue, leaves skin with dewy natural glow' }}
가격/프로모션: {{ $json.price_offer ?? '정가 32,000원 → 첫 구매 25,600원 (20% 할인)' }}

═══════════════════════════════════════
모델 설정 (Model Character)
═══════════════════════════════════════

외모: {{ $json.model_appearance ?? 'Korean woman late 20s, shoulder-length slightly wavy dark brown hair, minimal natural makeup, healthy glowing skin' }}
의상: {{ $json.model_outfit ?? 'white oversized cotton t-shirt, comfortable casual home look' }}
촬영 환경: {{ $json.setting ?? 'bright naturally-lit bedroom with white/beige interior, morning sunlight through sheer curtains, cozy Korean apartment aesthetic' }}
UGC 스타일: {{ $json.ugc_style ?? 'authentic selfie vlog filmed on iPhone, casual intimate tone like talking to a close friend, Korean 반말 style' }}

═══════════════════════════════════════
실행 지시
═══════════════════════════════════════

1. 위 정보를 분석하세요
2. model_image_prompt를 직접 생성하세요 (Flux Kontext용 모델 레퍼런스 이미지 프롬프트)
3. hook 도구를 호출하세요 (0-6초 훅 섹션)
4. problem 도구를 호출하세요 (6-12초 문제 섹션)
5. DISCOVERY 도구를 호출하세요 (12-18초 발견 섹션)
6. SOLUTION 도구를 호출하세요 (18-24초 솔루션 섹션)
7. CTA 도구를 호출하세요 (24-30초 CTA 섹션)
8. 5개 도구 결과를 취합하여 최종 JSON을 출력하세요
```

---

## 3. Structured Output Parser — JSON Schema (복사용)

아래 JSON을 n8n의 `Structured Output Parser` 노드 → JSON Schema 필드에 붙여넣는다.

**중요**: n8n Structured Output Parser (typeVersion 1.3)의 JSON Schema 필드에는 아래 전체를 그대로 넣는다.

```json
{
  "type": "object",
  "properties": {
    "model_image_prompt": {
      "type": "string",
      "description": "Flux Kontext prompt for generating the AI model reference portrait photo. Must be in English. Must include hyper-realistic keywords."
    },
    "timelines": {
      "type": "array",
      "description": "Array of exactly 5 timeline sections, each generated by a sub-agent tool.",
      "items": {
        "type": "object",
        "properties": {
          "timeline_id": {
            "type": "number",
            "description": "Sequential ID from 1 to 5"
          },
          "section": {
            "type": "string",
            "description": "Section name: hook, problem, discovery, solution, or cta"
          },
          "time_range": {
            "type": "string",
            "description": "Time range string like 0-6s, 6-12s, 12-18s, 18-24s, 24-30s"
          },
          "infinitetalk_prompt": {
            "type": "string",
            "description": "English prompt for InfiniteTalk API to generate the talking avatar video clip for this section. Must describe facial expression, gesture, camera angle, and emotion."
          },
          "broll_prompt": {
            "type": "string",
            "description": "English prompt for Veo 3.1 API to generate the B-Roll cutaway video clip for this section. Must describe the visual scene, camera movement, lighting, and product interaction."
          },
          "broll_name": {
            "type": "string",
            "description": "Short descriptive filename for the B-Roll clip, like morning_skincare_routine or mist_spray_closeup"
          },
          "tts_text": {
            "type": "string",
            "description": "Korean TTS script text for Typecast API. Natural conversational Korean in 반말 style. Must be speakable within the 6-second time range."
          },
          "tts_emotion": {
            "type": "string",
            "description": "Emotion tag for Typecast TTS: one of happy, excited, calm, curious, confident, warm, urgent"
          }
        },
        "required": ["timeline_id", "section", "time_range", "infinitetalk_prompt", "broll_prompt", "broll_name", "tts_text", "tts_emotion"]
      }
    }
  },
  "required": ["model_image_prompt", "timelines"]
}
```

---

## 4. 5개 하위 에이전트(agentTool) 설정 확인사항

메인 에이전트가 도구를 호출할 때, 각 agentTool의 **Name**과 **Description** 필드가 정확해야 메인 에이전트가 어떤 도구를 언제 호출할지 판단할 수 있다.

### hook agentTool
- **Name**: `hook`
- **Description**: `Generate prompts for the HOOK section (0-6 seconds) of a 30-second AI UGC review video. This tool creates the attention-grabbing opening scene prompts including InfiniteTalk talking avatar prompt, Veo 3.1 B-Roll prompt, Korean TTS script, and emotion tag. Call this tool FIRST with product data JSON.`

### problem agentTool
- **Name**: `problem`
- **Description**: `Generate prompts for the PROBLEM section (6-12 seconds) of a 30-second AI UGC review video. This tool creates the pain-point and problem identification scene prompts including InfiniteTalk talking avatar prompt, Veo 3.1 B-Roll prompt, Korean TTS script, and emotion tag. Call this tool SECOND with product data JSON.`

### DISCOVERY agentTool
- **Name**: `DISCOVERY`
- **Description**: `Generate prompts for the DISCOVERY section (12-18 seconds) of a 30-second AI UGC review video. This tool creates the product discovery and unboxing scene prompts including InfiniteTalk talking avatar prompt, Veo 3.1 B-Roll prompt, Korean TTS script, and emotion tag. Call this tool THIRD with product data JSON.`

### SOLUTION agentTool
- **Name**: `SOLUTION`
- **Description**: `Generate prompts for the SOLUTION section (18-24 seconds) of a 30-second AI UGC review video. This tool creates the product application and visible result scene prompts including InfiniteTalk talking avatar prompt, Veo 3.1 B-Roll prompt, Korean TTS script, and emotion tag. Call this tool FOURTH with product data JSON.`

### CTA agentTool
- **Name**: `CTA`
- **Description**: `Generate prompts for the CTA section (24-30 seconds) of a 30-second AI UGC review video. This tool creates the final call-to-action and recommendation scene prompts including InfiniteTalk talking avatar prompt, Veo 3.1 B-Roll prompt, Korean TTS script, and emotion tag. Call this tool FIFTH and LAST with product data JSON.`

---

## 5. 왜 이전 버전이 실패했는가 — 기술적 원인 분석

### 원인 1: System Prompt가 "직접 생성"을 지시함
이전 System Prompt에 `"Create a MASTER DIRECTING PLAN"`, `"Generate hyper-realistic prompts for each scene"` 같은 문구가 있었다. GPT-4o는 이 지시를 따라 **직접** 프롬프트를 생성했고, 5개 도구를 호출할 필요를 느끼지 못했다.

**수정**: `"You are FORBIDDEN from writing any video prompt yourself"`, `"You MUST call ALL 5 tools"` 으로 금지와 의무를 명확히 분리.

### 원인 2: Parser 스키마가 도구 출력과 불일치
이전 스키마에 `global_visual_rules`, `camera_setup`, `physical_continuity_rules` 같은 필드가 있었는데, 이것들은 하위 에이전트가 반환하는 필드가 아니다. 메인 에이전트가 이 필드들을 채우려다 도구 호출 대신 직접 생성 모드로 빠졌다.

**수정**: 스키마를 `model_image_prompt` + `timelines[5]`로 단순화. timelines의 각 항목은 하위 에이전트가 반환하는 5개 필드(infinitetalk_prompt, broll_prompt, broll_name, tts_text, tts_emotion)에 section 메타데이터(timeline_id, section, time_range)만 추가.

### 원인 3: 도구 Description이 모호함
이전 agentTool의 Description이 짧거나 애매하면, GPT-4o가 "이 도구가 내 작업에 필요한가?"를 판단하지 못하고 건너뛴다. 각 도구의 Description에 **언제 호출해야 하는지 (FIRST/SECOND/...)**, **무엇을 반환하는지**를 명시해야 한다.

---

## 6. 적용 순서 체크리스트

1. [ ] `메인 팀장 AI: 영상 디렉터` 노드 열기
2. [ ] System Message 필드 → 섹션 1의 전문을 복사하여 붙여넣기 (기존 내용 전체 교체)
3. [ ] User Message (Prompt) 필드 → 섹션 2의 전문을 복사하여 붙여넣기 (기존 내용 전체 교체)
4. [ ] `Structured Output Parser` 노드 열기
5. [ ] JSON Schema 필드 → 섹션 3의 JSON을 복사하여 붙여넣기 (기존 내용 전체 교체)
6. [ ] 5개 agentTool 노드(hook, problem, DISCOVERY, SOLUTION, CTA) 각각 열기
7. [ ] 각 agentTool의 **Name** 필드 → 섹션 4의 정확한 이름 확인/수정
8. [ ] 각 agentTool의 **Description** 필드 → 섹션 4의 전문을 복사하여 붙여넣기
9. [ ] 워크플로우 저장
10. [ ] 테스트 실행하여 메인 에이전트가 5개 도구를 순서대로 호출하는지 확인

---

## 7. 트러블슈팅 — 만약 여전히 도구를 호출하지 않는다면

### 체크 1: agentTool의 Name 필드가 System Prompt의 도구명과 정확히 일치하는가?
- System Prompt에 `"hook"` 이라고 썼으면 agentTool의 Name도 정확히 `hook` 이어야 한다 (대소문자 구분)
- `DISCOVERY`와 `SOLUTION`은 대문자로 통일

### 체크 2: agentTool이 메인 에이전트의 ai_tool 포트에 올바르게 연결되어 있는가?
- 5개 agentTool 모두 `메인 팀장 AI: 영상 디렉터`의 ai_tool 입력에 연결되어야 함
- 연결이 끊어진 agentTool은 메인 에이전트의 도구 목록에 나타나지 않음

### 체크 3: OpenAI Chat Model의 모델이 gpt-4o 이상인가?
- gpt-3.5-turbo는 도구 호출(function calling) 성능이 낮아서 도구를 건너뛸 수 있음
- 반드시 gpt-4o 또는 gpt-4o-mini 이상 사용

### 체크 4: Structured Output Parser가 메인 에이전트의 ai_outputParser 포트에 연결되어 있는가?
- Parser가 연결 안 되어 있으면 JSON 형식 강제가 안 됨
- Parser가 연결되어 있으면 메인 에이전트는 반드시 해당 스키마 형태로 출력해야 함

### 체크 5: 메인 에이전트의 Max Iterations 설정
- 5개 도구를 호출하려면 최소 10회 이상의 iteration이 필요
- n8n 기본값이 낮으면 도구 호출 도중에 중단될 수 있음
- `메인 팀장 AI: 영상 디렉터` 노드 → Options → Max Iterations → 15 이상으로 설정

---

*끝. 이 문서의 3개 섹션(System Prompt, User Message, Parser Schema)을 각각 해당 n8n 노드에 복사하여 붙여넣으면 된다.*
