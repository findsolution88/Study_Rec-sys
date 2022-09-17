<font color='dodgerblue'> 예쁜 파랑 </font>

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
<img width="600" alt="image" src="https://user-images.githubusercontent.com/48994965/190840995-fd0dd70e-ed28-439f-9206-ec7d0c0810cc.png">

<br/>

### Recommender System Overview
<img width="600" alt="image" src="https://user-images.githubusercontent.com/48994965/190841016-109e1e32-0139-430b-b1fe-6a15cc41d062.png">

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


<span style="color:blue"> Cross-product feature의 k번째 요소 </span>

- <span style="color:red">Raw feature의 i번째 요소가 true인지 여부</span>







