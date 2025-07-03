
# EmotiVibe의 PAD 이론 및 VIBE 코드 매핑 원리

## 1. PAD 모델의 이론적 배경

### 1.1. 다차원 감정 분석의 출발점

- **의미미분법(Semantic Differential)**
1957년 Osgood, Suci, Tannenbaum은 Good–Bad, Strong–Weak, Active–Passive와 같은 양극성 형용사 쌍을 사용해 단어나 개념의 의미를 연속 척도로 평가하는 방법을 개발함.
요인분석 결과, 세 가지 보편적 차원(Evaluation, Potency, Activity)이 도출되어 감정의 다차원적 측정 이론의 토대가 됨.


### 1.2. PAD(Pleasure–Arousal–Dominance) 모델 공식화

- **Mehrabian \& Russell (1974)**
Osgood의 연구를 바탕으로 PAD 모델을 환경심리학·사회심리학에 도입.
18개의 양극성 형용사 척도(각 차원별 6개)로 환경·자극에 대한 감정 반응을 정량적으로 평가함.
    - **Pleasure(쾌락–불쾌):** 감정의 긍정성/부정성
    - **Arousal(각성–비각성):** 감정의 활성화 수준
    - **Dominance(지배–순종):** 상황에 대한 통제감
- 모든 감정은 이 3차원 공간의 한 점으로 표현 가능하며, 환경·커뮤니케이션·대인관계 등 다양한 맥락에서 감정 반응을 미세하게 분석할 수 있음.


### 1.3. 감정 원형 모델과 PAD의 확장

- **Russell (1980, 2003)**
Pleasure–Arousal 2차원만으로 감정의 원형(circumplex) 구조를 제시.
이후 Dominance 차원을 상황 평가(appraisal)로 재해석하여, 감정 에피소드와 심리적 구성 이론에 PAD를 통합함.


## 2. PAD 어휘 사전의 발전과 신뢰도

| 어휘집 | 연도 | 규모 | 측정법/신뢰도 |
| :-- | :-- | :-- | :-- |
| ANEW | 1999 | 1,034개 단어 | SAM 그림 척도 9점, V=0.95, A=0.78, D=0.78 |
| Warriner et al. | 2013 | 13,915개 단어 | 의미미분법 9점, V=0.95, A=0.90, D=0.90 |
| NRC VAD Lexicon v2.1 | 2025 | 55,001개 단어·구 | Best–Worst Scaling, V=0.99, A=0.98, D=0.96 |

- **ANEW**: 1,034개 영어 단어에 대해 SAM(Self-Assessment Manikin) 그림 척도로 PAD 점수 제공. 심리학·NLP 표준 어휘집.
- **Warriner et al.**: 13,915개 단어로 확장, 크라우드소싱 기반 9점 척도, 높은 신뢰도와 인구통계별 분석 제공.
- **NRC VAD Lexicon v2.1**:
    - 44,928개 단어 + 10,073개 다중어구(MWE) = 총 55,001개 항목.
    - **Best–Worst Scaling** 방식으로 편향 최소화, 평균 8명 이상 응답, split-half reliability V=0.99, A=0.98, D=0.96.
    - 감정 분석, 텍스트 마이닝, 사회과학 등 다양한 분야에서 활용.
    - EmotiVibe는 이 사전의 PAD 계수를 4개 감정(anger, happy, panic, sadness)에 직접 적용함.


## 3. EmotiVibe의 PAD 벡터 계산 및 VIBE 코드 매핑

### 3.1. 감정 확률 예측

- EfficientNet-B0 기반 AI 모델이 얼굴 이미지를 분석하여 **anger, happy, panic, sadness** 4개 감정 클래스의 softmax 확률값 산출함.


### 3.2. PAD 계수 가중합

- NRC VAD Lexicon v2.1 부록에서 제공하는 공식 PAD 계수를 사용함.

| 감정 | Pleasure(P) | Arousal(A) | Dominance(D) |
| :-- | :-- | :-- | :-- |
| anger | –0.666 | +0.730 | +0.314 |
| happy | +0.985 | +0.470 | +0.390 |
| panic | –0.876 | +0.876 | –0.200 |
| sadness | –0.896 | –0.424 | –0.672 |

- **PAD 벡터 계산 공식**
각 감정의 softmax 확률을 해당 PAD 계수에 곱해 합산함.

$$
\text{PAD}_d = \sum_{e \in \{\text{anger, happy, panic, sadness}\}} P_e \times C_{e,d}
$$

(여기서 $d$는 P, A, D 중 하나, $P_e$는 감정 e의 softmax 확률, $C_{e,d}$는 해당 감정의 PAD 계수)
- 결과 PAD 벡터는 $[-1, +1]$ 범위의 실수값으로 정규화됨.


### 3.3. VIBE 코드 부호화

- 각 PAD 차원의 부호(sign, 0 기준)에 따라 3문자 VIBE 코드 생성함.

| 차원 | 0 이상(+) | 0 미만(–) | 이니셜 의미(영문/한글) |
| :-- | :-- | :-- | :-- |
| Pleasure | V | M | Vibrant(긍정) / Mellow(부정) |
| Arousal | E | C | Energized(각성) / Calm(이완) |
| Dominance | O | Y | Own(주도) / Yielding(수용) |

- 예시: PAD = (0.25, 0.30, –0.10) → **VEY** (긍정·고각성·수용)
- 8가지 VIBE 코드 각각에는 EmotiVibe만의 이름, 색상, 설명이 매핑되어 시각화함.


## 4. 신뢰성 및 과학적 근거

- **PAD 계수**는 NRC VAD Lexicon v2.1(2025) 부록 공식값을 사용하며, split-half reliability가 V=0.99, A=0.98, D=0.96으로 검증됨.
- **모델**은 EfficientNet-B0로, 8,393장 데이터셋에서 F1-Score 74.2%, 120 FPS의 실시간 추론 성능을 달성함.
- **전체 파이프라인**은 학계 표준 이론(PAD)과 최신 대규모 어휘 사전, 실험적 검증을 결합하여 신뢰할 수 있는 감정 분석 결과를 제공함.


## 5. 부록: 관련 핵심 논문 요약 및 팩트체크

### Osgood, Suci \& Tannenbaum (1957) – The Measurement of Meaning

- 의미미분법(semantic differential) 개발. Good–Bad, Strong–Weak, Active–Passive 등 양극성 형용사 쌍을 7점 척도로 평가.
- 요인분석 결과, **평가(Evaluation), 힘(Potency), 활동(Activity)** 3차원이 문화·언어를 초월해 보편적으로 나타남을 확인.
- 이 세 차원은 감정뿐 아니라 태도, 개념, 인상 평가 등 다양한 분야에 적용되며, 이후 PAD 모델의 이론적 토대가 됨.


### Mehrabian \& Russell (1974) – Pleasure–Arousal–Dominance (PAD) Model

- Osgood의 연구를 바탕으로 **PAD 3차원 감정 모델**을 공식화. 18개 양극성 형용사 척도(각 차원별 6개)로 환경·자극에 대한 감정 반응을 정량적으로 평가.
- 모든 감정은 이 3차원 공간의 한 점으로 표현 가능. 환경심리학, 마케팅, HCI 등 다양한 분야에서 표준적 감정 측정 도구로 자리잡음.


### Russell (1980, 2003) – Circumplex Model and Core Affect

- Pleasure–Arousal 2차원만으로 감정의 원형(circumplex) 구조를 제시. 감정이 원형상에 배열된다는 이론을 발표.
- 2003년에는 Dominance 차원을 **상황 평가(appraisal)**로 재해석, 감정 에피소드와 심리적 구성 이론에 PAD를 통합.


### Bradley \& Lang (1999) – Affective Norms for English Words (ANEW)

- 1,034개 영어 단어에 대해 SAM(Self-Assessment Manikin) 그림 척도로 Pleasure, Arousal, Dominance 점수를 제공.
- 9점 척도, r>.90의 높은 상관, 심리학·NLP 표준 어휘집으로 활용.


### Warriner, Kuperman \& Brysbaert (2013) – Norms for 13,915 Lemmas

- ANEW를 확장해 13,915개 영어 단어에 대해 9점 척도 기반 PAD 점수 제공.
- 크라우드소싱, calibrator 단어, 신뢰도 검증, 인구통계별 분석 등 고도화된 방법론 적용.
- ANEW와의 상관 Valence r=.95, Arousal r=.76, Dominance r=.80, split-half reliabilities: V=.91, A=.69, D=.77.


### Mohammad (2025) – NRC VAD Lexicon v2.1

- 55,001개 단어·구(44,928개 단어 + 10,073개 다중어구)에 대해 Best–Worst Scaling 방식으로 PAD 점수 산출.
- 각 단어·구에 대해 평균 8건의 비교문항, split-half reliability V=0.99, A=0.98, D=0.96.
- 감정 분석, 텍스트 마이닝, 사회과학 등 다양한 분야에서 활용.
- EmotiVibe는 이 사전의 PAD 계수를 4개 감정(anger, happy, panic, sadness)에 직접 적용함.

**이 문서는 EmotiVibe의 감정 분석 로직과 이론적 근거, 신뢰성, 그리고 최신 어휘 사전의 활용 방식을 체계적으로 정리한 Notion용 버전임.**

<div style="text-align: center">⁂</div>

[^1]: EmotiVibe-balpyo-camgo-jaryo.docx

[^2]: Paper-VAD-v2-2025.pdf

