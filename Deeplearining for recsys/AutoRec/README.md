## AutoEncoder
<img width="700" alt="image" src="https://user-images.githubusercontent.com/48994965/193022943-33e04ece-ded3-46b1-b214-c49a37b27778.png">


## Abstract
<img width="450" alt="image" src="https://user-images.githubusercontent.com/48994965/193020787-2b6475e9-88f2-4cc5-ae99-cbe627c6d9c9.png">

- Autoencoder를 Collagorative Filtering에 적용한 논문
- Movielens와 Netflix데이터셋에 대해서 좋은 성능을 입증

<br/>

## Introduction
<img width="450" alt="image" src="https://user-images.githubusercontent.com/48994965/193020952-86796e92-6ee2-4fcd-950a-efbb621f61c4.png">

- Autoencoder를 Collaborative Filtering에 적용한 논문
- 최근 Vision과 Speech분야에서 성공을 거둔 neural network를 적용한 논문
- Representation과 computation에서 모두 장점이 있음

<br/>

## AutoRec Model
<img width="600" alt="image" src="https://user-images.githubusercontent.com/48994965/193021282-1a2279e6-16ef-455a-8827-bd790baf44cc.png">

- RBM-CF(Restricted Boltzmann machines for collaborative filtering)와 비교
  - RBM-CF는 resticted Boltzmann machine을 사용한 일반화된 확률 모델이지만, AutoRec는 discriminative하고 autoencoder를 활용한 모델
  - RBM-CF눈 log likelihood를 최대화하고, AutoRec는 RMSE를 최소화함
  - 학습할 때, RBM-CF는 대조발산(Boltzmann machine), AutoRec은 비교적 빠른 gradient-based 역전파를 사용
  - RBM-CF는 discrete rating에 적합하고 각 rating값에 대해 파라미터를 추정하지만, AutoRec은 더 적은 파라미터가 필요하고 오버피팅의 확률 낮음
- MF와 비교
  - MF는 linear representation이지만, AutoRec은 non-linear함
  - MF는 user, item 모두 latent space에 두지만, (item-based)AutoRec은 item만 embed함

<br/>

## Experiments
<img width="600" alt="image" src="https://user-images.githubusercontent.com/48994965/193022166-3f35f380-3eff-4f5d-a72a-8953daf31da7.png">

- Item-based가 user-based보다 성능이 우수함(item 데이터가 더 많기 때문)
- Hidden layer의 non-linearity가 효과 있음
- AutoRec이 모든 Baseline데 대해 우수한 성능을 보여줌


