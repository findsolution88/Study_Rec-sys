# Bayesian Personalized Ranking from Implicit Feedback

## Abstract
<img src = "https://user-images.githubusercontent.com/48994965/188893694-e89d2add-94c8-4968-a143-ca56391fe048.png" width="30%" height="30%">

- Implicit Feedback으로 추천알고리즘을 다루는 논문
- Matrix Factorization과 (adaptive)K-nearest-neighbor(KNN)으로 personalized rainking 실험
- Bayesian을 활용한 최적화 기법(BPR-Opt)을 제시
- 제안된 기법을 적용하여 기존의 MF, KNN보다 더 우수한 성능 증명

<br/>

## Contribution
<img src = "https://user-images.githubusercontent.com/48994965/188894811-af9764d2-5ce7-4697-a6ce-70f1830597a7.png"  width="30%" height="30%">

- Maximum Posterior Estimator에 기반한 최적화 기법인 BPR-OPT를 제안
- BPR-OPT를 위한 LearnBPR을 제안하며 기존 SGD보다 성능이 우수함
- LearnBPR을 당시 최신 모델에 적용
- BPR-OPT가 다른 방법보다 우수함을 증명

<br/>

## Introduction & Related Work
- 추천시스템을 구현하기 위해 2가지 형태가 일반적 (Explicit data & Implicit data)
- 보통 Implicit 데이터가 더 많은 비중을 차지하고, 더 어려운 문제
- Implicit data로 user의 선호도 또는 취향을 파악해야함
- Implicit feedback으로 ranking릏 추천할 수 있는 알고리즘을 제시
- 저자가 정의한 ranking을 추천하기 위한 optimization을 아래와 같음
  - Item i와 j가 있고 user가 item i보다 j를 선호한다 → item i > item j 
  - 이 때, 학슥할 파라미터를 최적화하는 것이 목적

<br/>

## Personalized Ranking
- personalized Ranking이란
  - User에게 ranked list of items를 제안
  - Item Recommendation이라고 부름
- 본 논문은 Implicit Feedback으로 user의 취향을 고려
  - Implicit feedback은 주로 positive feedback이 대부붕
  - Non-observed item을 다루는 것이 중요한 포인드
  - Non-observed item = real negative feedback + missing values

### Formalization
- U는 모든 User의 집합, I는 모든 Item의 집합
- 이때, 각 user별 personalized total ranking을 구하는 것
- 







