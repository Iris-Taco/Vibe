<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# EmotiVibe 서비스 기획안 v2.0 및 부록 A–E (최종 종합본)

아래는 EmotiVibe 서비스 기획안 v2.0과 모든 부록(A, B, C, D, E)의 최종 내용을 통합한 문서입니다[1]. 이 문서는 실제 구현 코드(`server.py`)와 논의된 모든 요구사항을 100% 반영하여, 개발 및 운영의 기준이 되는 **단일 소스 진실(Single Source of Truth)** 역할을 합니다[2][3][4][5][6][7][8][9][10][11][12][13][14][15][16][17][18][19][20][21][22][23][24][25].

**문서 버전:** 2.0
**최종 수정일:** 2025-06-30
**런칭 목표:** 2025년 7월 초 (전체 기능 포함)[13]

### **1. 서비스 개요 (Overview)**

EmotiVibe는 사용자의 얼굴 이미지를 분석하여 8가지 VIBE 코드로 감정을 시각화하고, 이를 **Journal Studio**라는 통합 편집 허브에서 창의적인 일기와 감정 카드로 제작·저장·공유하는 AI 기반 올인원 디지털 셀프케어 플랫폼입니다[13].

- **과학적 기반**: EfficientNet-B2 모델과 NRC-VAD v2.1에 기반한 PAD(Pleasure-Arousal-Dominance) 이론을 통해 신뢰도 높은 감정 분석을 제공합니다[13][20].
- **직관적 경험**: 복잡한 감정 데이터를 VIBE 코드, 차트, AI 저널로 시각화하여 누구나 쉽게 자신의 감정 상태를 이해할 수 있도록 돕습니다[21].
- **창의적 표현**: Journal Studio의 강력한 편집 도구를 통해 사용자가 자신의 감정을 자유롭고 아름답게 기록하고 표현하도록 지원합니다[13].
- **데이터 주권**: 사용자가 자신의 데이터를 직접 관리하고, GDPR 등 개인정보 보호 규정을 철저히 준수하여 데이터 프라이버시를 보장합니다[8][13].


### **2. 정보 구조(IA) 및 내비게이션 (최종안)**

- **모바일 하단 탭 바 (4 Tabs)**: `Home` | `Analyze` | `My Vibe` | `Profile`[2]
- **데스크톱 상단 헤더 바 (5 Tabs + Profile Dropdown)**: `Home` | `Analyze` | `My Vibe` | `Learn` | `[사용자 닉네임] ▾`[2]
- **My Vibe 내부 구조**: 페이지 진입 후 `Diary` | `Journal Studio` | `Dashboard` 3개의 2차 탭(모바일) 또는 사이드바 메뉴(데스크톱)로 구성됩니다[2][13].


### **3. 주요 기능 상세 (최종안)**

- **Analyze \& Result View**: 드래그 앤 드롭 또는 웹캠으로 이미지를 입력받아 VIBE 코드 카드, PAD 막대그래프, 감정 분포 도넛 차트, AI 저널 텍스트 초안을 포함한 종합 결과 뷰를 제공합니다[2]. 결과 뷰 하단에는 **"Journal Studio로 편집하기"**라는 명확한 CTA 버튼을 제공합니다[5].
- **Journal Studio (통합 일기·카드 제작소)**: 템플릿 라이브러리, Fabric.js 기반 캔버스 에디터(이미지 편집, 콘텐츠 추가), AI 보조 도구(태그 추천, 질문 제안)를 제공합니다[3].
- **My Vibe (나의 기록)**: Journal Studio에서 제작한 결과물을 보여주는 `Diary`(FullCalendar)와 누적 데이터를 시각화하는 `Dashboard`(비교 모드, AI 리포트)로 구성됩니다[3].
- **Learn \& Profile**: PAD 이론 및 VIBE 변환 로직을 설명하는 인터랙티브 시뮬레이터(`Learn`)와 프로필 관리 및 앱 설정을 위한 `Profile` 페이지를 제공합니다[2].


### **4. 기술 스택 및 아키텍처 (최종안)**

- **AI/ML**: `EfficientNet-B2`(감정 분석), `Google Gemini`(기본)와 `OpenAI GPT-4o`(폴백)를 활용한 생성형 AI[10][13].
- **백엔드**: Flask, SQLAlchemy, PostgreSQL, Flask-Login[3].
- **프론트엔드**: Vue 3 기반의 컴포넌트 SPA, Tailwind CSS 디자인 시스템, Workbox 기반 PWA[4][13].


### **5. 보안 및 컴플라이언스**

- **데이터 보안**: HTTPS/TLS 1.3 암호화, Salted Hash 비밀번호, AES-256 데이터 암호화[13].
- **이미지 처리**: EXIF 데이터 즉시 제거, 30일 후 서버 영구 파기[3][8].
- **사용자 프라이버시**: GDPR, CCPA 준수 및 DSR(Data Subject Request) 기능 제공[8][13].
- **시스템 보안**: CSRF 보호, API Rate-Limiting, CSP 헤더 설정[3][13].


## **부록 A: PAD–VIBE 변환 체계 v2.1 (Final \& Synchronized)**

### **1. End-to-End 변환 파이프라인**

1. **이미지 분석 및 모델 추론**: `EfficientNet-B2` 모델을 통해 4가지 기본 감정(`anger`, `happy`, `panic`, `sadness`)의 확률값을 계산합니다[12].
2. **PAD 벡터 계산**: 각 감정의 확률값에 미리 정의된 PAD 계수(NRC-VAD v2.1 기반)를 가중합하여 3차원 PAD 벡터를 생성합니다[12][20].
3. **VIBE 부호화**: 계산된 PAD 벡터의 각 차원 값이 0 이상인지 미만인지에 따라 VIBE 코드를 생성합니다. (초기 기획의 Dead-Zone 개념은 최종 구현에서 제외됨)[12].
4. **결과 매핑 및 AI 저널 생성**: 생성된 VIBE 코드에 해당하는 이름, 색상, 설명을 매핑하고, AI 저널 텍스트를 생성하여 최종 JSON 응답을 구성합니다[6].

### **2. 알고리즘 및 상수 정의 (서버 코드 기준)**

- **4가지 기본 감정 클래스**: `anger`, `happy`, `panic`, `sadness`[12]
- **NRC-VAD v2.1 기반 PAD 계수**:[12][20]

```json
{
  "anger":   {"pleasure": -0.666, "arousal":  0.730, "dominance":  0.314},
  "happy":   {"pleasure":  0.985, "arousal":  0.470, "dominance":  0.390},
  "panic":   {"pleasure": -0.876, "arousal":  0.876, "dominance": -0.200},
  "sadness": {"pleasure": -0.896, "arousal": -0.424, "dominance": -0.672}
}
```

- **VIBE 부호화 로직**:[12]

```python
def generate_vibe_code(pad_values):
    pleasure_sign = 'V' if pad_values['pleasure'] >= 0 else 'M'
    arousal_sign  = 'E' if pad_values['arousal']  >= 0 else 'C'
    dominance_sign= 'O' if pad_values['dominance'] >= 0 else 'Y'
    return pleasure_sign + arousal_sign + dominance_sign
```


### **3. 최종 응답 데이터 구조 (JSON)**

`/api/analyze` 엔드포인트는 다음 JSON 형식으로 응답합니다[12]:

```json
{
  "success": true,
  "analysis_id": 137,
  "emotions": {"anger":0.12, "happy":0.63, "panic":0.05, "sadness":0.20},
  "pad_values": {"pleasure":0.25, "arousal":0.30, "dominance":-0.10},
  "vibe_code": "VEY",
  "vibe_name": "기쁨의 폭포",
  "vibe_color": "#4ECDC4",
  "vibe_description": "생동감 넘치고 활기차지만 수용적인 상태로...",
  "confidence": 63.0,
  "journal_text": "온화한 행복이 당신을 감싸고 있어요.",
  "timestamp": "2025-07-04T09:41:12Z"
}
```


## **부록 B: 서버 아키텍처 및 API 명세 v2.1 (Final \& AI 공유 확장)**

### **1. API 엔드포인트 요약 (OpenAPI 3.0 기반)**

- **Auth**: `POST /api/auth/register`, `POST /api/auth/login`, `GET /api/auth/me`, `POST /api/auth/logout`, `POST /api/auth/merge-anonymous`, `GET /api/auth/oauth/{provider}/callback`[11]
- **Analyze**: `POST /api/analyze`[3]
- **Diary**: `GET /api/diary`, `POST /api/diary`, `GET /api/diary/{id}`, `PUT /api/diary/{id}`, `DELETE /api/diary/{id}`[11]
- **AI Chat**: `GET /api/diary/{id}/insight`, `GET /api/diary/{id}/share-emotion/stream` (SSE), `POST /api/diary/{id}/chat`[3][11]
- **Cards**: `GET /api/cards`, `POST /api/cards`, `GET /api/cards/{id}`[11]
- **Dashboard**: `GET /api/dashboard/stats`[3]
- **Admin \& Health**: `GET /api/admin/users`, `GET /api/admin/stats`, `POST /api/admin/report`, `GET /api/health`[11]


### **2. 에러 핸들링**

모든 4xx/5xx 에러는 일관된 `ApiError` 스키마로 응답합니다[11]:

```json
{
  "status": 400,
  "error": "Bad Request",
  "message": "요청 형식이 잘못되었습니다.",
  "details": { "field": "email", "issue": "Invalid format" }
}
```


## **부록 C: 생성형 AI 연동 사양 v2.1 (Final \& Synchronized)**

### **1. AI 연동 아키텍처**

- **서버 중심 아키텍처**: 클라이언트는 서버를 통해서만 LLM API를 호출합니다[10].
- **전용 서비스 모듈**: `app/services/ai_service.py`에서 LLM 로직을 중앙 관리합니다[10].
- **LLM 호출 전략**: `Google Gemini 1.5 Flash`(무료)를 기본 모델로, `OpenAI GPT-4o`를 폴백 및 프리미엄 모델로 사용합니다[10].


### **2. API 키 적용 및 관리**

API 키는 프로젝트 루트의 `.env` 파일에 저장하고 `os.getenv()`를 통해 불러옵니다[10].

```
# .env file
GOOGLE_API_KEY="AIzaSyAFE3flot-QAzl6CeXkDtEn87Mf57q3AR8"
OPENAI_API_KEY="sk-..."
```


### **3. 프롬프트 엔지니어링**

- **GenerationConfig 제어**: `compassionate`, `motivational`, `reflective` 등 대화 톤별로 `temperature`와 `top_p`를 조절합니다[10].
- **마스터 프롬프트 템플릿**: VIBE 코드, 톤별 가이드라인, 사용자 감정 분석 요약, 이전 대화 기록, 사용자 질문을 조합하여 동적으로 프롬프트를 생성합니다[10].


### **4. 보안 및 프라이버시**

- **데이터 비식별화**: PII(개인 식별 정보)를 감지하고 마스킹 처리합니다[10].
- **명시적 동의 강제**: `ai_chat_enabled` 플래그가 `True`인 경우에만 AI 관련 엔드포인트가 동작합니다[8][10].


## **부록 D: 이용약관 및 개인정보처리방침 v2.0 (Final)**

### **1. 이용약관 주요 내용**

- **서비스의 정의**: AI 기반 감정 분석, Journal Studio, Dashboard, Learn 등 모든 제공 기능을 포함합니다[9].
- **게시물의 저작권**: 사용자가 작성한 게시물의 저작권은 사용자에게 귀속됩니다[9].
- **면책조항**: EmotiVibe의 분석 결과 및 AI 콘텐츠는 의학적 진단이나 전문적인 심리 상담을 대체할 수 없음을 명시합니다[9].


### **2. 개인정보처리방침 주요 내용**

- **수집 항목**: (필수) 이메일, 비밀번호, 닉네임, (선택) 얼굴 이미지, Journal Studio 콘텐츠, (자동) IP 주소, 쿠키 등[8][9].
- **보유 및 이용 기간**: 회원 탈퇴 시까지. 단, 분석 이미지는 **30일 보관 후 영구 파기**합니다[8][9].
- **AI 기능 이용 시 개인정보 처리**: 명시적 동의(`enable_ai_chat`) 기반으로 동작하며, 전송된 데이터는 응답 생성 목적으로만 사용되고 AI 모델 학습에는 사용되지 않음을 명시합니다[8][9].
- **개인정보 보호책임자**: `OOO` (추후 확정 필요), `privacy@emotivibe.com`[8].


## **부록 E: i18n 키셋 및 콘텐츠 관리 v2.0 (Final)**

### **1. i18n 키셋 구조**

- **파일 구조**: `/locales` 디렉터리 내 `ko.json`, `en.json`으로 관리[6][7].
- **키 명명 규칙**: `페이지명.컴포넌트명.상태` 형식 (예: `analyze.upload_prompt`)[6][7].


### **2. VIBE 코드 콘텐츠 관리 (이모지 배제)**

- **server.py 내 VIBE_CODES 정의**: 이모지를 제거하고, 이름, 색상, 태그라인만 포함합니다[6][7].

```python
VIBE_CODES = {
    'VEO': {'name': '태양 폭발', 'color': '#FFB400', 'tagline': '활기찬 긍정'},
    'VEY': {'name': '기쁨의 폭포', 'color': '#4ECDC4', 'tagline': '온화한 행복'},
    # ... (나머지 6개 코드) ...
}
```

- **VIBE 코드 상세 설명**: i18n 키셋(`vibe_descriptions.VEO`)에 포함하여 다국어를 지원합니다[6][7].


### **3. AI 생성 콘텐츠 관리**

- `generate_journal_text` 함수(in server.py)는 VIBE 코드별 사전 정의된 문구 라이브러리에서 무작위로 텍스트 초안을 생성합니다[6][7].
- 이 문구 라이브러리 또한 다국어 지원을 위해 i18n 키셋으로 분리하여 관리할 수 있습니다[6][7].

