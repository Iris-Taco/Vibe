<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# EmotiVibe 디자인 시스템 및 UI/UX 가이드 v2.0 (최종 종합본)

**문서 버전:** 2.0 (Final)
**최종 수정일:** 2025-06-30
**동기화 기준:** 서비스 기획안 v2.0, 모든 부록(A-E), 기능 요구사항 v2.2, Stitch Design HTML 목업, `server.py` 코드, 전체 논의 히스토리

**문서 목적:** 이 문서는 EmotiVibe 웹사이트의 시각적 정체성과 사용자 경험을 정의하는 단일 소스 진실(Single Source of Truth)입니다. 개발팀이 일관되고 높은 품질의 인터페이스를 구현할 수 있도록 **디자인 원칙, 색상, 타이포그래피, 컴포넌트, 레이아웃, 인터랙션** 등 모든 시각적·경험적 요소를 상세히 기술합니다.

## 1. 디자인 철학 및 원칙

EmotiVibe의 디자인은 다음 4가지 핵심 원칙을 기반으로 합니다.

1. **세련된 미니멀리즘 (Modern Minimalism)**: 불필요한 장식 요소를 배제하고, 여백과 타이포그래피, 그리고 절제된 색상 사용을 통해 콘텐츠의 본질에 집중합니다. **이모지는 일절 사용하지 않으며**, 모든 시각적 정보는 목적성을 가진 아이콘과 텍스트로 전달합니다.
2. **직관적 명확성 (Intuitive Clarity)**: 사용자는 어떤 학습 없이도 서비스의 흐름을 자연스럽게 이해할 수 있어야 합니다. 모든 인터랙션과 정보 구조는 예측 가능하고 명확해야 합니다.
3. **데이터 기반 시각화 (Data-Driven Visualization)**: 복잡한 감정 분석 데이터를 VIBE 카드, PAD 막대그래프, 도넛 차트 등 이해하기 쉬운 시각적 형태로 변환하여 사용자에게 통찰력을 제공합니다.
4. **모바일 퍼스트 접근성 (Mobile-First Accessibility)**: 320px 너비의 모바일 화면부터 데스크톱까지 모든 환경에서 일관되고 쾌적한 경험을 제공하며, WCAG 2.1 AA 수준의 접근성을 준수합니다.

## 2. 디자인 토큰 (Design Tokens)

모든 시각 요소는 아래에 정의된 디자인 토큰을 기반으로 구현되어야 합니다. 이는 `tailwind.config.js` 파일에 직접 적용됩니다.

### 2.1 색상 팔레트 (Color Palette)

- **기본 색상**:
    - `text`: `#111418` (주 텍스트)
    - `secondary`: `#637488` (부 텍스트, 아이콘)
    - `border`: `#dce0e5` (경계선)
    - `background-light`: `#f0f2f4` (밝은 배경)
    - `background-dark`: `#111418` (다크모드 배경)
- **액센트 색상**:
    - `accent`: `#1978e5` (주요 버튼, 활성 상태, 링크)
- **VIBE 코드 색상**:
    - `VEO`: `#FFB400`
    - `VEY`: `#4ECDC4`
    - `VCO`: `#00B8D9`
    - `VCY`: `#0091EA`
    - `MEO`: `#FF6B6B`
    - `MEY`: `#F39C12`
    - `MCO`: `#9C27B0`
    - `MCY`: `#673AB7`
- **혼합 감정 (Ambiguous)**:
    - `AMB-bg`: `#e5e7eb` (배경)
    - `AMB-text`: `#4b5563` (텍스트)


### 2.2 타이포그래피 (Typography)

- **폰트 패밀리**: `Manrope` (제목), `Noto Sans` (본문)
- **크기 및 가중치**:
    - **H1 (페이지 제목)**: `text-[28px] font-bold`
    - **H2 (섹션 제목)**: `text-[22px] font-bold`
    - **H3 (컴포넌트 제목)**: `text-base font-bold` (16px)
    - **Body (본문)**: `text-base font-normal` (16px)
    - **Subtext (부연 설명)**: `text-sm text-secondary` (14px)


### 2.3 간격, 반경, 그림자

- **간격 (Spacing)**: 4px 그리드 시스템 (Tailwind 기본 스케일 `space-4` = 16px)
- **반경 (Radius)**: `rounded-xl` (16px)을 모든 카드와 모달에 기본 적용
- **그림자 (Shadow)**: `shadow-md` (`0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1)`)를 상호작용 가능한 카드에 적용


## 3. 컴포넌트 라이브러리

### 3.1 버튼 (Button)

- **Primary**: `bg-accent text-white px-6 py-3 rounded-full font-medium`
- **Secondary**: `bg-background-light text-text px-6 py-3 rounded-full font-medium`
- **Outline**: `border border-border text-text px-6 py-3 rounded-full font-medium`
- **States**: 모든 버튼은 `hover:opacity-80`, `focus:ring-2 focus:ring-accent` 스타일을 가집니다.


### 3.2 카드 (Cards)

- **VIBE 카드**: `w-[280px] h-[360px] p-6 rounded-xl shadow-md` 구조. 배경색은 VIBE 코드 색상으로 동적 적용.
- **기능 카드**: `bg-white dark:bg-background-dark p-4 rounded-xl border border-border` 구조. 호버 시 `shadow-md` 효과.
- **KPI 카드**: `bg-white dark:bg-background-dark p-4 rounded-xl` 구조. 미니멀한 디자인.


### 3.3 모달 (Modals)

- `fixed inset-0 bg-black/50 flex items-center justify-center` 백드롭.
- `bg-white dark:bg-background-dark p-6 rounded-xl shadow-lg` 컨텐츠 영역.
- 닫기 버튼은 우측 상단에 아이콘 형태로 배치.


### 3.4 내비게이션 (Navigation)

- **데스크톱 헤더**: `bg-white dark:bg-background-dark border-b border-border` 스타일.
- **모바일 하단 탭**: `fixed bottom-0 w-full bg-white dark:bg-background-dark border-t border-border` 스타일. 활성 탭 아이콘과 텍스트는 `text-accent` 색상.


### 3.5 폼 요소 (Form Elements)

- **Input**: `border border-border rounded-lg p-3`, `focus:ring-accent`.
- **Toggle/Switch**: 둥근 형태의 스위치, 활성 시 `bg-accent`.


## 4. 페이지별 레이아웃 및 UX 패턴

### 4.1 Home 페이지

- **레이아웃**: 단일 컬럼, 중앙 정렬. 사용자가 서비스의 핵심 가치와 CTA에 즉시 집중하도록 설계.
- **CTA**: 두 개의 버튼(`Analyze`, `Learn`)을 나란히 배치하여 사용자의 다음 행동을 명확히 유도.
- **GuestBanner**: `fixed bottom-4 left-4 right-4` 형태로 화면 하단에 고정하여 스크롤과 무관하게 노출.


### 4.2 Analyze 페이지

- **업로드 영역**: `border-dashed` 스타일로 인터랙티브 영역임을 시각적으로 암시.
- **진행 상태**: `ProgressOverlay`는 전체 화면을 덮는 반투명 레이어로, 사용자가 현재 다른 작업을 할 수 없음을 명확히 전달.


### 4.3 Result 페이지

- **시각적 계층**: VIBE 카드를 가장 상단에 크게 배치하여 핵심 결과를 먼저 인지시킨 후, PAD 바, 도넛 차트, AI 저널 순으로 상세 정보를 제공.
- **혼합 감정**: VIBE 코드가 `AMB`일 경우, VIBE 카드는 중립적인 회색(`bg-AMB-bg`) 배경과 함께 "혼합 감정" 뱃지를 표시.


### 4.4 My Vibe 페이지

- **탭 구조**: `Diary`, `Studio`, `Dashboard`는 명확히 구분된 탭으로 제공. 데스크톱에서는 사이드바 메뉴로 전환하여 넓은 화면을 효율적으로 활용.
- **Journal Studio**: 3단 레이아웃 (좌: 템플릿/도구, 중앙: 캔버스, 우: AI/속성)으로 구성하여 제작 흐름을 체계적으로 지원.


### 4.5 Learn 페이지

- **아코디언**: 이론 콘텐츠는 아코디언 컴포넌트로 제공하여 사용자가 원하는 정보만 선택적으로 탐색할 수 있도록 함.
- **시뮬레이터**: 슬라이더와 실시간 결과 카드를 나란히 배치하여 인터랙션의 결과를 즉각적으로 확인할 수 있도록 설계.


### 4.6 Profile/Auth 페이지

- **폼 디자인**: 각 입력 필드와 레이블은 충분한 간격을 두어 가독성을 높이고, 실시간 유효성 검사 피드백을 인라인 메시지로 제공.


## 5. 접근성 및 다크 모드

- **접근성**: 모든 인터랙티브 요소는 `focus-visible:ring-2 ring-accent` 스타일을 통해 키보드 포커스를 명확히 표시. 모든 아이콘 버튼은 `aria-label`을 포함.
- **다크 모드**: `darkMode: 'class'` 전략을 사용. `dark:` 변형을 통해 배경, 텍스트, 경계선 색상을 토큰 기반으로 일괄 전환.

이 디자인 시스템 및 UI/UX 가이드는 EmotiVibe가 시각적으로 뛰어나고, 사용하기 쉬우며, 기술적으로 견고한 플랫폼으로 완성되기 위한 최종 청사진입니다. 이 문서를 기준으로 모든 컴포넌트와 페이지를 구현하면, 기획된 모든 내용이 완벽하게 반영된 최종 결과물이 나올 것입니다.

