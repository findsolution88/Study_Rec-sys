## Abstract
<img width="425" alt="image" src="https://user-images.githubusercontent.com/48994965/190840325-20816b05-b629-45d4-93aa-11d79a504afc.png">

- Wide: Memorization
  - Cross-product feature transformation
  - More feature enginneering effot
- Deep: Gerneralization
  - Gerneralize to unseen feature combinations
  - Less feature engineering effort
  - Over-Gerneralize
- Wide & Deep
  - Joint wide linear models and deep neural networks

<br/>

## Introduction (1)
- Memorization의 정의
  - Frequent co-occurrence of items of features
  - Exploit correlation available in the historical data
- Generalization의 정의
  - Expore new feature combinations
- 추천시스템에서 Memorization과 Gerneralization
  - Memorization: more topical and directly relevant to the items
  - Gereralization: improve diversity of the recommendations
- Google Play Store의 apps recommendation 실험에서 gerneral하게 적용 가능 확인

<br/>

## Introduction (2)]
- 기존 추천 모델의 한계
  1. Gernalized Linear Model
      - Logistic Regression과 같은 모델에 다양한 features를 만들어 학습시칸다
      - Memorization(주어진 데이터 기억)에 특화된다
      - 새로운 또는 관측되지 않은 데이터(unseen data)에는 취약함
      - 오버피팅 발생 가능
  2. Embedding based Model
      - Factorization Machine, Deep Neural Network 방법을 활용
      - Gerneralization(unseen data)에 특화
      - Non-zero prediction으로 인해 섬세한 추천이 불가능

<br/>

## Contributions
- Wide & Deep Learning framework를 제안
  - Jointly training feed-forward neural network and linear model
- Google Play에 상용화된 Wide & Deep 추천시스템의 평가와 구현내용을 공개
  - 모바일 앱스토어에서 App구매, 다운로드 등 향상
  - 학습과 서비스 속도를 충족
- Tensorflow기반의 API 오픈소스 형태로 제공

<br/>

## Wide & Deep Leaning Framework
### Model Overview
<img width="750" alt="image" src="https://user-images.githubusercontent.com/48994965/190840995-fd0dd70e-ed28-439f-9206-ec7d0c0810cc.png">

<br/>

### Recommender System Overview
<img width="750" alt="image" src="https://user-images.githubusercontent.com/48994965/190841016-109e1e32-0139-430b-b1fe-6a15cc41d062.png">

<br/>

### The Wide Component
$y=w^tx+b$

- Gerneralized Linear model
- y → prediction(유저행동여부)
- $x$ = [ $x_1, x_2,...,x_d$ ] 
  - size d의 feature vector
  - Raw input features & cross-product feature
- $w$ = [ $w_1, w_2,...,w_d$ ] → model parameters
- b → bias

<img width="450" alt="image" src="https://user-images.githubusercontent.com/48994965/190841215-66c85c74-74ee-4ca8-9ef0-ac98cf2f0733.png">

- $\phi_k(X)$ : Cross-product feature의 k번째 요소 
- $C_{ki} \in$ {0,1} : Raw feature의 i번째 요소가 true인지 여부
- Feature innteraction 반영
- 모델에 Non-linearity 적용
  - 예시: feature가 {gender, language}로 구성되어 있고, cross-product transformation(feature)의 k번째를 AND(female, en)일때, 모두 만족하면 1 아니면 0

<br/>

### The Deep Component
<img width="467" alt="image" src="https://user-images.githubusercontent.com/48994965/190841784-7ad82cc1-f3d4-4c01-9b8d-9abc9de329bc.png">

- Features를 임베딩과 뉴럴넷에 학습
  - **Gerneralization**
$a^{l+1}= f(W^{(l)}a^{(l)}+b^{(l)}$
- l은 number layers
- f는 activation function(ReLU)
- $a^{(l)},b^{(l)},W^{(l)}$
  - Activation, bias and model weights at l-th layer
- **Dense Embeddings**
  - Nominal Feature에 해당하는 임베딩을 랜덤 초기화(random initialize)하고 모델 전체 학습

<br/>

### Joint Traing of Wide & Deep Model(1)
<img width="700" alt="image" src="https://user-images.githubusercontent.com/48994965/190841951-7a592e4e-0cb3-4361-a2ed-855915ea8c87.png">

### Joint Traing of Wide & Deep Model(2)
<img width="700" alt="image" src="https://user-images.githubusercontent.com/48994965/190841968-4e34cead-cf15-4856-be46-de5776fd390d.png">

### Joint Traing of Wide & Deep Model(3)
<img width="700" alt="image" src="https://user-images.githubusercontent.com/48994965/190842011-9586238a-1ddf-4922-8e68-62a8d6fa7a77.png">

- P(Y=1|X)는 app을 다운받을 확률
- Sidmoid 함수의 input으로 Wide와 Deep을 합한 값을 넣어주고, 그 output이 최종결과가 됨

<br/>

### System Inplementation
- pipe line은 아래와 같음
<img width="700" alt="image" src="https://user-images.githubusercontent.com/48994965/190842078-6cab48e5-f0d6-4be5-8007-4dd6e9a85e5f.png">

<br/>

## Experiments
### Experiments(1)
<img width="550" alt="image" src="https://user-images.githubusercontent.com/48994965/190842108-379525ff-fc1b-45c8-84a8-62762914f80c.png">

- Offline AUC
  - Test set의 사용자 행동결과로 성능 측정
  - AUC는 클수록 좋은 점수(=ROC 커브의 아래 면적)
- Online Acquisition Gain
  - 실제 사용자들의 action을 추적
  - 기존 모델 대비 application 실제 다운로드 수 증가

### Experiments(2)
<img width="550" alt="image" src="https://user-images.githubusercontent.com/48994965/190842188-9d39c60e-c625-4be0-9d6d-ad698d549797.png">

- Multithreading, Split batch into smaller batchs -> latency를 14ms 줄일 수 있음
- 추천알고리즘의 성능 뿐만 아니라 Commercial Mobile App Store의 서비스까지 신경씀

<br/>

## Conclusions
- Memorization의 Wide와 Gerneralization의 Deep을 결합한 모델을 제안
- Linear model과 embedding-based model의 장점을 조합
- 좋은 추천알고리즘을 실제 서비스 환경에서 작동할 수 있도록 구현
- Open source로 Tensorflow API를 구현하여 다양하게 활용 가능





