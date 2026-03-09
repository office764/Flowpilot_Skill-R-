# FlowPilot 크리덴셜 환경변수 템플릿

이 파일은 FlowPilot의 모든 서버/서비스 접속 정보 구조를 정의한다.
실제 값은 로컬 `.env` 파일에만 저장하고, 절대 SKILL.md나 코드에 하드코딩하지 않는다.

---

## .env 파일 구조

```env
# ============================================
# FlowPilot 환경변수 (.env)
# ============================================
# 이 파일은 절대 Git에 커밋하거나 공유하지 않는다.
# .gitignore에 반드시 .env를 추가할 것.
# ============================================

# ------------------------------------------
# [서버 1] 홈페이지 백엔드 (flowpilots.org)
# ------------------------------------------
# 호스팅: Hostinger
# OS: Ubuntu 24.04 LTS
# 위치: Malaysia - Kuala Lumpur
# 용도: FlowPilot 메인 홈페이지, 랜딩페이지, 대시보드 프론트엔드
# ------------------------------------------
WEB_SERVER_HOST=76.13.180.41
WEB_SERVER_USER=root
WEB_SERVER_PASSWORD=
WEB_SERVER_DOMAIN=flowpilots.org
WEB_SERVER_PROVIDER=hostinger
WEB_SERVER_LOCATION=malaysia

# ------------------------------------------
# [서버 2] n8n 자동화 백엔드 (n8n.flowpilots.org)
# ------------------------------------------
# 호스팅: Vultr
# OS: Ubuntu
# 위치: Seoul, Korea
# 용도: n8n 워크플로우 엔진, API 백엔드, 자동화 실행
# ------------------------------------------
N8N_SERVER_HOST=158.247.245.248
N8N_SERVER_USER=root
N8N_SERVER_PASSWORD=
N8N_SERVER_DOMAIN=n8n.flowpilots.org
N8N_SERVER_PROVIDER=vultr
N8N_SERVER_LOCATION=seoul

# ------------------------------------------
# [서비스 3] Supabase 클라우드 DB/인증/API
# ------------------------------------------
# 프로젝트: office@flowpilots.org's Project
# Project ID: lvbanhlhnvhombiovwfi
# 용도: PostgreSQL DB, Auth(인증), Storage(파일), Realtime, Edge Functions
# 대시보드: https://supabase.com/dashboard/project/lvbanhlhnvhombiovwfi
# ------------------------------------------
SUPABASE_PROJECT_ID=lvbanhlhnvhombiovwfi
SUPABASE_URL=https://lvbanhlhnvhombiovwfi.supabase.co

# 프론트엔드(브라우저)에서 사용 — RLS로 보호됨, 노출 OK
SUPABASE_ANON_KEY=

# 서버사이드(n8n, 백엔드)에서만 사용 — 절대 프론트엔드에 노출 금지!
# RLS를 우회하므로 관리자 권한과 동일
SUPABASE_SERVICE_ROLE_KEY=

# Supabase 내부 키 (특수 목적)
SUPABASE_PUBLISHABLE_KEY=
SUPABASE_SECRET_KEY=

# JWT — 커스텀 토큰 발급/검증 시에만 사용. 절대 외부 노출 금지.
SUPABASE_JWT_SECRET=

# ------------------------------------------
# [서비스 URL 요약]
# ------------------------------------------
WEB_URL=https://flowpilots.org
N8N_URL=https://n8n.flowpilots.org
SUPABASE_DASHBOARD_URL=https://supabase.com/dashboard/project/lvbanhlhnvhombiovwfi
SUPABASE_API_URL=https://lvbanhlhnvhombiovwfi.supabase.co

# ------------------------------------------
# [API Keys - 필요 시 추가]
# ------------------------------------------
# OPENAI_API_KEY=
# ANTHROPIC_API_KEY=
# GOOGLE_API_KEY=
# META_ACCESS_TOKEN=
# SLACK_BOT_TOKEN=
```

---

## 사용 규칙

1. **하드코딩 절대 금지**: 모든 스킬, 모든 코드에서 IP, 비밀번호, API Key를 직접 적지 않는다
2. **환경변수 참조만 허용**: `process.env.N8N_SERVER_HOST`, `$N8N_SERVER_HOST` 형태로만 사용
3. **인프라 구분 필수**: WEB_SERVER_* / N8N_SERVER_* / SUPABASE_* 를 절대 혼동하지 않는다
4. **.env 파일 위치**: 프로젝트 루트 디렉토리에 단 1개만 존재
5. **백업**: .env 파일은 암호화된 별도 저장소에 백업

## Supabase Key 사용 가이드

| Key 종류 | 사용 위치 | 브라우저 노출 | 용도 |
|---------|----------|-------------|------|
| `SUPABASE_ANON_KEY` | 프론트엔드 (React/Next.js) | OK (RLS 보호) | DB 조회, Auth 로그인, Storage 접근 |
| `SUPABASE_SERVICE_ROLE_KEY` | 서버사이드 (n8n, 백엔드) | 절대 금지 | 관리자 권한 DB 접근, RLS 우회, 사용자 관리 |
| `SUPABASE_JWT_SECRET` | 서버사이드 전용 | 절대 금지 | 커스텀 JWT 토큰 발급/검증 |
| `SUPABASE_PUBLISHABLE_KEY` | 프론트엔드 | OK | Supabase 퍼블릭 식별자 |
| `SUPABASE_SECRET_KEY` | 서버사이드 전용 | 절대 금지 | Supabase 시크릿 인증 |

### 어디서 어떤 키를 쓰는가?

```
[flowpilots.org 프론트엔드]
  → SUPABASE_URL + SUPABASE_ANON_KEY
  → createClient(SUPABASE_URL, SUPABASE_ANON_KEY)

[n8n.flowpilots.org 워크플로우]
  → SUPABASE_URL + SUPABASE_SERVICE_ROLE_KEY
  → HTTP Request 노드 Headers:
      apikey: $SUPABASE_SERVICE_ROLE_KEY
      Authorization: Bearer $SUPABASE_SERVICE_ROLE_KEY

[커스텀 백엔드/Edge Functions]
  → SUPABASE_JWT_SECRET (토큰 검증 시)
  → SUPABASE_SERVICE_ROLE_KEY (DB 관리 시)
```
