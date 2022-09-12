# Bayesian Personalized Ranking from Implicit Feedback

## Abstract
<img src = "https://user-images.githubusercontent.com/48994965/188893694-e89d2add-94c8-4968-a143-ca56391fe048.png" width="35%" height="35%">

- Implicit Feedback으로 추천알고리즘을 다루는 논문
- Matrix Factorization과 (adaptive)K-nearest-neighbor(KNN)으로 personalized rainking 실험
- Bayesian을 활용한 최적화 기법(BPR-Opt)을 제시
- 제안된 기법을 적용하여 기존의 MF, KNN보다 더 우수한 성능 증명

<br/>

## Contribution
<img src = "https://user-images.githubusercontent.com/48994965/188894811-af9764d2-5ce7-4697-a6ce-70f1830597a7.png"  width="40%" height="40%">

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
- 이때, 각 user별 personalized total ranking( $<_u\subset I^2$ )을 구하는 것
- $>_u$ 는 다음과 같은 특징을 가짐

<img src = "https://user-images.githubusercontent.com/48994965/189561873-8a92f00e-4a9b-48de-bcae-91199b939718.png" width="50%" height="50%">

- totality : i와 j가 다르다면 u는 i를 j보다 더 좋아하거나 j를 i보다 더 좋아함
- antisymmetry : u가 i보다 j를 더 좋아하고, j를 i보다 더 좋아한다면 i와 j는 같음
- transitivity : u가 j보다 i를 더 좋아하고, j를 k보다 더 좋아한다면 u는 k보다 i를 더 좋아함

### Analysis of Problem setting(1)
<img src = "https://user-images.githubusercontent.com/48994965/189562691-df442882-f15b-44c0-b9e4-253cef65b8e5.png" width="50%" height="50%">

- 기존에 많이 사용하던 데이터 처리 방법
- ?는 관측되지 않은 데이터, +는 관측된 데이터
- 0은 관측되지 않은 데이터 = 선호하지 않음
- 1은 관측된 데이터 = 선호함
- Implicit data를 다루는 중요한 문제점이 있음


### Analysis of Problem setting(2)
<img src="https://user-images.githubusercontent.com/48994965/189563191-c63e2fac-40af-4175-b03a-8e41346941e6.png" width="50%" height="50%">

- 저자가 제안하는 방법
- 전체 데이터를 Pairwise preference $i >_u j$ 로 나타냄
- +는 user가 item j에 비해 item i를 선호
- -는 user가 item i에 비해 item j를 선호
- user가 보지 못한 item j에 비해 관측한 item i를 더 선호한다고 가정
- user가 (i,j)모두 관측 했거나, 관측하지 않닸다면 어떤 선호도도 추론할 수 없음

<img width="403" alt="image" src="https://user-images.githubusercontent.com/48994965/189563640-d0356981-d5a6-4948-9006-afb1d6f86cc3.png">

- User u는 item j보다 i를 더 선호함
- $>_u$는 antisymmetric이기 떄문에 negetive는 implicit이라고 가정함


### Analysis of Problem setting(3)
- 저자가 제안하는 방법의 장점
  - 학습셋 $D_s$ 은 positive, negative, missing value로 구성됨
  - 아이템 쌍 (i,j)에서 관측되지 않은 결측값은 테스트셋이 됨
  - 학습셋과 테스트셋은 disjoint함(겹치는 부분이 없음)
  - 학습셋은 실제 랭킹 목적에 맞게 만들어진다
  - 관측된 데이터의 부분집합인 $D_s$ 을 학습데이터로 사용함

<br/>

## Bayesian Personalized Ranking (BPR)
- 주어진 학습 데이터 $D_s$ 로 Bayesian Personalized Ranking 구하는 방법을 설명함
- $P(i>_uj|\Theta )$ 에 대한 likelihood function과 model parameter $p(\Theta)$ 에 대한 prior probability를 사용한 베이지안 문제로 볼 수 있음
- 다음과 같은 순서로 section이 구성됨
  - BPR Optimization Criterion
    - Analogies to AUC optimization
  - BPR Learning Algorithm
  - Learning models with BPR
    - Matrix Factorization
    - Acaptive K-Nearest Neighbor
    
### BPR Optimization Criterion

#### BPR Optimization Criterion (1)

<img width="450" alt="image" src="https://user-images.githubusercontent.com/48994965/189565233-df503e8d-4fba-4a18-b280-674bdccdd8ed.png">

- Bayesian optimization이란, 위의 사후 확률을 최대화 하는 파라미터 Θ를 찾는 것
- Totality와 Antisymmetry에 따라 다음과 같이 정리할 수 있음

#### BPR Optimization Criterion (2)
<img width="350" alt="image" src="https://user-images.githubusercontent.com/48994965/189566062-aa7f3606-3527-44a3-8ea0-e3062c7199fd.png">

- $\hat{x_{uij}}(\Theta)$ 는 user u와 item i,j의 관계를 나타내는 모델 파라미터 벡터의 실제 함수
- 즉, $\hat{x_{uij}}(\Theta)$ 를 추정하면서 u,i,j사이의 관계를 모델링 함
- 전체 순서를 모델링하는 것이 비교적 간단해짐

<img width="350" alt="image" src="https://user-images.githubusercontent.com/48994965/189566328-3cfa10be-e34c-47ae-b40c-93917fd107ef.png">

- 사전확률분포 $p( \Theta ) ~ N(0,\Sigma_\Theta)$ (정규분포)로 나타내고, $\Sigma_\Theta=\lambda_\Theta I$로 정함 
- Likelihood와 prior 모두 정의 했으므로, posterior를 공식에 따라 구할 수 있음

#### BPR Optimization Criterion (3)
<img width="500" alt="image" src="https://user-images.githubusercontent.com/48994965/189566897-824b12c8-5ef8-44dd-bfba-410a79430951.png">

<br/>

### BPR Learning Algorithm
<img width="435" alt="image" src="https://user-images.githubusercontent.com/48994965/189567166-6a166ace-863b-444e-88a0-678f66734528.png">

- 미분가능하기 때문에 gradient descent로 optimization 가능
- SGD가 적절한 선택지가 아니기 때문에 LEARN-BPR을 제안
 - Triples를 학습하는 Bootstrap기반 Stochastic Gradient-descent알고리즘
- 랜덤하게 triples를 선택하는 SGD알고리즘을 사용
 - 동일한 쌍의 데이터 선택할 확률이 적음
 - 데이터 전수가 아닌 bootstrap sampling만 해도 데이터가 많기 때문에 충분함
 
<br/>

### Learning models with BPR
<img width="700" alt="image" src="https://user-images.githubusercontent.com/48994965/189567683-762cbd6c-1e71-40ec-b7b2-df03dfc24220.png">

- LearnBPR최적화를 위해 모든 모델 파라미터 $\Theta$ 에 대한 $\hat{x_{uij}}$ 의 gradient를 정의하고 어떻게 활용할 것인가에 대한 적용

<br/>

## Evaluation
<img width="652" alt="image" src="https://user-images.githubusercontent.com/48994965/189567979-1351d138-99e9-4cec-86a2-2cd0ad3c164d.png">

- BPR을 적용한 MF와 KNN이 성능이 좋다고 주장함

<br/>

## Conclusion
- 사후확률을 최대화 하는 베이지안에 의한 새로운 방법을 제안
- Personalized Ranking을 위한 optimization기법(BPR-OPT)을 제안
- Bootstrap을 통한 Stochastic Gradient Descent를 사용하여 모델 파라미터를 업데이트 함
- 기존 Matrix Factorization, k-Nearest Neighbor 모두 적용하였으며 성능이 우수함
- 랭킹을 추천하는 개념을 제안함






