## Abstract
<img width="350" alt="image" src="https://user-images.githubusercontent.com/48994965/193014115-a7f7e072-d56a-46dc-9165-8f39fdd79371.png">

- Click Though Rate(CTR)을 예측하는 모델
- Low와 High-order interactions 모두 학습 가능
- Factorization Machine의 장점과 Deep Learning의 장점을 모두 합친 모델이 DeepFM
- 추가 feature engineering없이 raw feature를 그대로 사용할 수 있음
- 벤치마크 데이터와 commercial데이터에서 모두 실험하여 성능 입증

<br/>

## Introduction (1)
1. CTR: user가 추천된 항목을 click할 확률을 예측하는 문제
    - CTR(estimated probability)에 의해 user가 선호할 item랭킹을 부여함
2. Learn Implicit Feature Interaction
    - App category와 Timestamp관계: 음식 배달 어플은 식사시간 근처에 다운로드가 많다
    - User gender와 Age 관계: 남자 청소년들은 슈탱과 RPG게임을 선호
    - 숨겨진 관계: 맥주와 기저귀를 함께 구매하는 사람들이 많다
    - -> low와 high-order feature interation을 모두 고려해야 함
    - -> Explicit과 Implicit feature를 모두 모델링할 수 있음

## Introduction (2): 현재까지 추천알고리즘 연구 정리
1. Gerneralized Linear Model(ex.FTRL)
    - 당시 성능은 좋은 모델이었으나, high-order feature interaction을 반영하기 어려움
2. Factorization Machine
    - Latent vector간의 내적을 통해 pairwise feature interaction을 모델링
    - Low와 high-order 모두 모델링이 가능하지만, high-order의 경우 complexity가 증가함
3. CNN and RNN for CTR Prediction
    - CNN-based는 주변 feature에 집중하지만, RNN-based는 sequential해서 더 적합함
4. Factorization-machine supported Neural Network(FNN), Product-based Neural Network
    - Neural Network 기반으로 high-order 가능하지만 low-order는 부족함
    - Pre-trained FM 성능에 의존할 수 있음
5. Wide & Deep
    - Low와 high-order 모두 가능하지만, wide component에 feature engineering 필요

<br/>

## Contributions
<img width="350" alt="image" src="https://user-images.githubusercontent.com/48994965/193016725-a7849d2a-5b9b-4733-b26a-11519aefa439.png">

- DeepFM 모델 구조 제안
  - Low-order는 FM, High-order는 DNN
  - End-to-end 학습가능
- DeepFM은 다른 비슷한 모델보다 더 효율적으로 학습 가능
  - Input과 embedding vector를 share함
- DeepFM은 benchmark와 commercial데이터의 CTR Prediction에서 의미있는 성능향상을 이룸

<br/>

## DeepFM
<img width="800" alt="image" src="https://user-images.githubusercontent.com/48994965/193017257-e61c07f6-f476-40ee-a772-f09909b7fb2c.png">

<br/>

### FM Component
<img width="400" alt="image" src="https://user-images.githubusercontent.com/48994965/193017458-39efba21-8cd0-449e-85e8-186a06bf658b.png">

<img width="400" alt="image" src="https://user-images.githubusercontent.com/48994965/193017640-bff4d4d9-6987-4ed2-a354-bc007ce873d1.png">

- embedding vector의 내적을 order-2의 가중치로 사용하는 것이 포인트

<br/>

### Deep Component
<img width="850" alt="image" src="https://user-images.githubusercontent.com/48994965/193017900-083adcc2-bc58-4a63-9b73-d1ae24e906aa.png">

<br/>

### Realtionship with the other Neural Network
<img width="850" alt="image" src="https://user-images.githubusercontent.com/48994965/193018111-3164fd47-50eb-42ab-bbfa-e5f01588cf15.png">

<br/>

## Experiments
1. Criteo Dataset
    - 45 million users' click record
    - 13 continuous features, 26 categorical ones
    - 90% for training and 10% for testing
2. Company Dataset
    - 7 consecutive days of users' click records from Company App Store(game center)
    - Next 1 day for testing
    - Approcimately 1 billion records
    - App features(identification, category)
    - User features(user's downloaded app)
    - Context features(aperation time)
3. Evaluation Metrics
    - AUC(Area Under ROC) and Loggloss(Cross entropy)


### Efficiency Comparison
<img width="600" alt="image" src="https://user-images.githubusercontent.com/48994965/193019113-d1e37392-811b-43f8-ba05-91b5b68e89c3.png">

- Linear model 대비 각 모델이 학습에 걸린 시간을 나타냄
- FNN은 pre-training에 많은 시간을 할애함
- IPNN과 OPNN은 hidden layer에서 inner product를 하면서 시간이 오래 걸림

### Effectiveness Comparison
<img width="600" alt="image" src="https://user-images.githubusercontent.com/48994965/193019412-165c3714-b5b1-44df-b633-3f2805c5d392.png">

<br/>

### Hyper-Parameter Study
<img width="500" alt="image" src="https://user-images.githubusercontent.com/48994965/193019592-17b95406-6446-467d-a314-b96173ba26d2.png">

<br/>

<img width="897" alt="image" src="https://user-images.githubusercontent.com/48994965/193019689-20d64171-77f1-42bc-bca4-2c45e2393a50.png">

<br/>


## Conclusions
- DeepFM
  - deep component와 FM component를 합쳐서 학습
  - Pre-training이 필요하지 않음
  - High와 Low-order feature interactions 둘 다 모델링
  - Input과 embedding vector를 share함
- From experiment
  - CTR task에서 더 좋은 성능을 얻을 수 있음
  - 다른 SOTA모델보다 AUC와 LogLoss에서 성능이 뛰어남
  - DeepFM이 가장 efficient한 모델
