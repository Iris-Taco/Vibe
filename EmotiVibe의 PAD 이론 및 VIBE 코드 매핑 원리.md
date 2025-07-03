
# EmotiVibe의 PAD 이론 및 VIBE 코드 매핑 원리

## 1. PAD 모델의 이론적 배경

### 1.1. 다차원 감정 분석의 출발점: 의미미분법

- 1957년 Osgood, Suci, Tannenbaum은 **의미미분법(Semantic Differential)**을 개발함.
- Good–Bad, Strong–Weak, Active–Passive와 같은 양극성 형용사 쌍을 사용해 단어나 개념의 의미를 연속 척도로 평가.
- 대규모 요인분석 결과, 세 가지 보편적 차원 도출:
    - **Evaluation (평가):** 긍정–부정
    - **Potency (힘):** 강함–약함 (지배–종속)
    - **Activity (활동):** 활동적–수동적 (각성–비각성)
- 이 세 차원은 문화와 맥락을 초월해 일관되게 나타나며, 이후 감정의 다차원적 측정 이론의 토대가 됨.


### 1.2. PAD(Pleasure–Arousal–Dominance) 모델 공식화

- 1974년 Mehrabian \& Russell은 Osgood의 연구를 바탕으로 **PAD 모델**을 환경심리학과 사회심리학에 도입.
- 각 차원별 6쌍의 양극성 형용사(총 18쌍, 36개 단어)로 환경·자극에 대한 감정 반응을 의미미분법 방식으로 평가.
- 세 차원 정의:
    - **Pleasure (쾌락–불쾌):** 감정의 긍정성/부정성
    - **Arousal (각성–비각성):** 감정의 활성화 수준
    - **Dominance (지배–순종):** 상황에 대한 통제감
- 모든 감정은 이 3차원 공간의 한 점으로 표현 가능하며, 환경, 커뮤니케이션, 대인관계 등 다양한 맥락에서 감정 반응을 미세하게 분석할 수 있음.


### 1.3. 감정 원형 모델과 PAD의 확장

- 1980년 Russell은 **Pleasure–Arousal** 2차원 원형(circumplex) 모델을 제안.
- 2003년에는 Dominance 차원을 **상황 평가(appraisal)**로 재해석하여, 감정 에피소드와 심리적 구성 이론에 PAD를 통합.


## 2. PAD 어휘 사전의 발전과 신뢰도

| 어휘집 | 연도 | 규모 | 측정법 및 신뢰도 |
| :-- | :-- | :-- | :-- |
| ANEW | 1999 | 1,034개 단어 | SAM 그림 척도 9점, V=0.95, A=0.78, D=0.78 |
| Warriner et al. | 2013 | 13,915개 단어 | 의미미분법 9점, V=0.95, A=0.90, D=0.90 |
| NRC VAD Lexicon v2.1 | 2025 | 55,001개 단어·구 | Best–Worst Scaling, V=0.99, A=0.98, D=0.96 |

- **ANEW**: 1,034개 단어에 대해 SAM(Self-Assessment Manikin) 그림 척도로 PAD 점수 제공. 심리학 및 NLP 분야에서 표준 어휘집으로 활용됨.
- **Warriner et al.**: 13,915개 단어로 확장, 크라우드소싱 기반 9점 척도, 높은 신뢰도와 인구통계별 분석 제공.
- **NRC VAD Lexicon v2.1**:
    - 44,928개 단어 + 10,073개 다중어구(MWE) 포함 총 55,001개 항목.
    - Best–Worst Scaling 방식으로 편향 최소화, 평균 8명 이상 응답, split-half reliability가 V=0.99, A=0.98, D=0.96로 매우 높음.
    - 감정 분석, 텍스트 마이닝, 사회과학 등 다양한 분야에서 활용.
    - EmotiVibe는 이 사전의 PAD 계수를 4개 감정(anger, happy, panic, sadness)에 직접 적용함.


## 3. EmotiVibe의 PAD 벡터 계산 및 VIBE 코드 매핑

### 3.1. 감정 확률 예측

- EfficientNet-B0 기반 AI 모델이 얼굴 이미지를 분석하여 **anger, happy, panic, sadness** 4개 감정 클래스의 softmax 확률값 산출.


### 3.2. PAD 계수 가중합

- NRC VAD Lexicon v2.1 부록에서 제공하는 공식 PAD 계수를 사용.

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

- 각 PAD 차원의 부호(sign, 0 기준)에 따라 3문자 VIBE 코드 생성.

| 차원 | 0 이상(+) | 0 미만(–) | 이니셜 의미(영문/한글) |
| :-- | :-- | :-- | :-- |
| Pleasure | V | M | Vibrant(긍정) / Mellow(부정) |
| Arousal | E | C | Energized(각성) / Calm(이완) |
| Dominance | O | Y | Own(주도) / Yielding(수용) |

- 예시: PAD = (0.25, 0.30, –0.10) → **VEY** (긍정·고각성·수용)
- 8가지 VIBE 코드 각각에는 EmotiVibe만의 이름, 색상, 설명이 매핑되어 시각화.


## 4. 신뢰성 및 과학적 근거

- PAD 계수는 NRC VAD Lexicon v2.1(2025) 부록 공식값을 사용하며, split-half reliability가 V=0.99, A=0.98, D=0.96으로 검증됨.
- 모델은 EfficientNet-B0로, 8,393장 데이터셋에서 F1-Score 74.2%, 120 FPS의 실시간 추론 성능을 달성.
- 전체 파이프라인은 학계 표준 이론(PAD)과 최신 대규모 어휘 사전, 실험적 검증을 결합하여 신뢰할 수 있는 감정 분석 결과를 제공.


## 5. 부록: 관련 핵심 논문 요약 및 팩트체크

### 5.1. Osgood, Suci \& Tannenbaum (1957) – The Measurement of Meaning

- 의미미분법 개발. Good–Bad, Strong–Weak, Active–Passive 등 양극성 형용사 쌍을 7점 척도로 평가.
- 요인분석 결과, 평가(Evaluation), 힘(Potency), 활동(Activity) 3차원이 문화·언어를 초월해 보편적으로 나타남.
- 감정뿐 아니라 태도, 개념, 인상 평가 등 다양한 분야에 적용되며, 이후 PAD 모델의 이론적 토대가 됨.


### 5.2. Mehrabian \& Russell (1974) – Pleasure–Arousal–Dominance (PAD) Model

- PAD 3차원 감정 모델 공식화.
- 18개 양극성 형용사 척도(각 차원별 6개)로 환경·자극에 대한 감정 반응 정량 평가.
- 모든 감정은 3차원 공간의 한 점으로 표현 가능.
- 환경심리학, 마케팅, HCI 등 다양한 분야에서 표준 도구로 자리잡음.


### 5.3. Russell (1980, 2003) – Circumplex Model and Core Affect

- Pleasure–Arousal 2차원 원형(circumplex) 모델 제안.
- 2003년에는 Dominance 차원을 상황 평가(appraisal)로 재해석, 감정 에피소드와 심리적 구성 이론에 PAD를 통합.


### 5.4. Bradley \& Lang (1999) – Affective Norms for English Words (ANEW)

- 1,034개 영어 단어에 대해 SAM(Self-Assessment Manikin) 그림 척도로 PAD 점수 제공.
- 9점 척도, 높은 상관관계, 심리학·NLP 표준 어휘집으로 활용.


### 5.5. Warriner, Kuperman \& Brysbaert (2013) – Norms for 13,915 Lemmas

- ANEW를 확장해 13,915개 영어 단어에 대해 9점 척도 기반 PAD 점수 제공.
- 크라우드소싱, calibrator 단어, 신뢰도 검증, 인구통계별 분석 등 고도화된 방법론 적용.
- ANEW와의 상관 Valence r=.95, Arousal r=.76, Dominance r=.80, split-half reliabilities: V=.91, A=.69, D=.77.


### 5.6. Mohammad (2025) – NRC VAD Lexicon v2.1

- 55,001개 단어·구(44,928개 단어 + 10,073개 다중어구)에 대해 Best–Worst Scaling 방식으로 PAD 점수 산출.
- 평균 8건의 비교문항, split-half reliability V=0.99, A=0.98, D=0.96.
- 감정 분석, 텍스트 마이닝, 사회과학 등 다양한 분야에서 활용.
- EmotiVibe는 이 사전의 PAD 계수를 4개 감정(anger, happy, panic, sadness)에 직접 적용.

## 6. EmotiVibe 용어 설명집

아래는 EmotiVibe의 PAD 이론 및 VIBE 코드 매핑 원리에서 등장하지만 본문에서 충분히 설명되지 않았거나, 오해의 소지가 있을 수 있는 주요 용어들을 정리한 것입니다.

### 6.1. 의미미분법 (Semantic Differential)

- **정의:** Osgood 등이 개발한 심리 측정법. Good–Bad, Strong–Weak, Active–Passive 등 양극성 형용사 쌍을 사용해 개념이나 감정의 의미를 연속 척도로 평가함.
- **용도:** 감정, 태도, 인상 등 다양한 심리적 특성의 다차원적 측정에 활용.


### 6.2. Best–Worst Scaling (BWS)

- **정의:** 여러 항목 중 ‘가장’과 ‘최소’에 해당하는 것을 선택하게 하는 비교 평가 방식.
- **특징:** 단순 평점 방식보다 응답자 편향이 적고, 신뢰도 높은 정량 데이터 확보에 유리함.
- **활용:** NRC VAD Lexicon v2.1의 감정 점수 산출에 사용됨.


### 6.3. Split-Half Reliability (SHR)

- **정의:** 신뢰도 검증 방법 중 하나. 전체 응답을 무작위로 두 집단으로 나눠 각 집단의 평균값 상관계수를 반복 측정해 일관성을 평가함.
- **의미:** 값이 1에 가까울수록 측정 결과의 재현성과 신뢰도가 높음을 의미.


### 6.4. Self-Assessment Manikin (SAM)

- **정의:** 감정 평가를 위한 비언어적 그림 척도. 표정, 자세 등으로 Pleasure, Arousal, Dominance를 9점 척도로 시각적으로 평가할 수 있게 설계됨.
- **용도:** 언어적 편향 없이 감정 상태를 직관적으로 측정할 때 사용.


### 6.5. Multi-Word Expression (MWE)

- **정의:** 두 개 이상의 단어로 이루어진 고정적 의미 단위(예: “give up”, “hot dog”).
- **활용:** NRC VAD Lexicon v2.1에서는 단일 단어뿐 아니라 약 10,000개의 다중어구에 대해서도 감정 점수를 제공함.


### 6.6. Circumplex Model (감정 원형 모델)

- **정의:** Russell이 제안한 감정 구조 이론. Pleasure(Valence)와 Arousal 두 축을 원형(circumplex)으로 배열해 감정 간 유사성과 차이를 시각적으로 표현함.
- **특징:** 감정이 연속적이고 다차원적으로 분포함을 강조.


### 6.7. Appraisal (상황 평가)

- **정의:** 감정 발생 시 개인이 상황을 어떻게 해석·평가하는지에 관한 심리학적 개념.
- **PAD와의 관계:** Russell(2003)은 Dominance 차원을 appraisal(상황 평가)로 재해석함.


### 6.8. Gold Standard (정답 문항)

- **정의:** 데이터 품질 관리를 위해 사전에 정답이 정해진 문항.
- **용도:** 크라우드소싱 평가에서 응답자 신뢰도 검증 및 실시간 피드백 제공에 사용.


### 6.9. Softmax 확률

- **정의:** 다중 클래스 분류에서 각 클래스에 속할 확률을 0~1 사이의 값으로 산출하는 함수.
- **활용:** EmotiVibe의 감정 분류 모델에서 4개 감정 클래스별 예측 확률 산출에 사용.


### 6.10. PAD 계수 (PAD Coefficient)

- **정의:** 특정 감정 단어가 Pleasure, Arousal, Dominance 각 차원에서 갖는 정량적 점수(–1~+1).
- **출처:** NRC VAD Lexicon v2.1 부록 등 공식 감정 어휘 사전에서 제공.


### 6.11. VIBE 코드

- **정의:** PAD 3차원 벡터의 부호(+,–) 조합을 3문자 코드(V/M, E/C, O/Y)로 변환한 EmotiVibe만의 감정 요약 체계.
- **예시:** VEO(긍정·각성·주도), MCY(부정·이완·수용) 등.


### 6.12. F1-Score

- **정의:** 분류 모델의 정밀도(Precision)와 재현율(Recall)의 조화평균. 불균형 데이터에서 모델 성능을 평가할 때 신뢰도가 높음.
- **활용:** EmotiVibe의 감정 분류 모델 성능 평가에 사용.
