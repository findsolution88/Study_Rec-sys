# I. Intro

## Latent Factor Model
- 사용자/아이템 특성을 벡터로 간략화(요약)하는 모델링
- 사용자/아이템 특성 간 복잡한 관계를 학습
- 사용자/아이템 행렬에서 사용자와 아이템을 Factor로 나타내는 방법
- 사용자와 아이템이 같은 vector공간에 표현됨
- 사용자와 아이템을 모르는 차원(demension)에 표현하고, 몇 개의 차원인지 모름
- 같은 vector 공간에서 사용자와 아이템이 가까우면 유사, 떨어져 있으면 유사하지 않음
<img src = "https://user-images.githubusercontent.com/48994965/188262984-58599ce3-103a-4986-9f82-24495b16d653.png" width="60%" height="60%">

<br/>

## Singular Value Decomposision 개요
$A=U \Sigma V^T$

- U는 고유값 분해로 얻dms mXm 직교행렬
  - $AA^T = U(\Sigma \Sigma^T)U^T$
  - U의 열벡터는 A의 left singular vector
- V는 고유값 분해로 얻은 nXn 직교행렬
  - $A^TA = V(\Sigma^T \Sigma)V^T$
  - V의 열벡터는 A의 right singular vector
- $\Sigma$ 는 mXn 대각행렬
  - 고유값분해해서 나온 eigenvalue의 제곱근이 대각원소
  - 대각원소 = A의 특이값

<p align="center">
<img src = "https://user-images.githubusercontent.com/48994965/188263418-05e88a5b-6ca3-460e-8337-be1e9bf86ff2.png" width="30%" height="30%">
<p/>

- 차원축소기법(Dimensionality Reduction) 중 하나
- 사용자와 아이템 간 데이터를 행렬 R로 나타냄
- 행렬 U는 사용자 latent factor, V는 아이템과 latent factor
- 행렬 U와 V의 모든 열벡터는 **특이벡터(Singular vector)**, 모든 특이벡터는 서로 직교
  - $U^TU = I, V^TV=I$
- 행렬 $\Sigma$ 의 대각성분은 M의 특이값
- 사용자와 아이템의 관계를 2차원 직교좌표계로 표현
  - 사용자와 아이템의 고유값 계산 -> 고유값으로 기존 평점 데이터를 다시 계산

<br/>

## Singular Value Decomposision 정리
- 데이터 차원 축소
  - 노이즈 제거, Sparse matrix 형태로 큰 데이터를 축소함
- 행렬 U는 user와 latent factor간의 관계
- 행렬 V는 item과 latent factor간의 관계
- 행렬 $\Sigma$는 대각행렬로, latent factor의 중요도
- Latent factor는 user와 item이 공통으로 갖는 특징
- 단, latent factor의 뜻을 이해하기 어렵기 떄문에 추천에 대한 구체적인 설명이 어려움

<br/><br/>

# II. Matrix Factorization
- Latent Factor Model을 구현하는 방법
- Rating Matrix를 분해하는 과정
- |U| x |I| : user-item rating matrix (rank k<n)
- P -> |U| x k : matrix of user factors
- Q -> |I| x k : matrix of item factors
<img src = "https://user-images.githubusercontent.com/48994965/188263921-fe6d9138-23e1-4954-aca3-d24df95dd00c.png" width="50%" height="50%">

<br/>

## Matrix Factorization 정리
<img src = "https://user-images.githubusercontent.com/48994965/188264010-d06cfed9-950d-42e4-9578-42d13a3b3e0e.png" width="60%" height="60%">

- 분해한 행렬X와 Y를 곱하여 평점을 예측
- 임의의 차원 수 f는 직접 정함
- $r_{ui} = x_u\^T$ x $y_i$
- R(원래 rating matrix)과 R'(예측 matrix)이 서로 유사하도록 학습하는 과정
  - 관측된 데이터만 사용

<br/>

## Matrix Factorization - Objective Function
<img src = "https://user-images.githubusercontent.com/48994965/188269123-cd1dc8b5-83bb-4cc2-ba3d-6d90ac6b3785.png" width="60%" height="60%">

- $x_u,y_i$: user와 item latent vector
- $r_{ui}$: user u가 item i에 부여한 실제 rating값
- $\hat{r_{ui}}=x_u^Ty_i$: user u가 item i에 대해서 줄 예측 rating값
- $\lambda (||x_u||^2+||y_i||^2)$: 정규화 (ragulariztion)
  - 제한적인 학습데이터에 의존하는 overfitting을 피하기 위해 eroor term 사용(Generalize)

<br/>

## Matrix Factorization - Optimization
### Stochasic Gradient Descent
- 학습 데이터의 모든 rating을 다 탐색
- Error term
  - 실제 평점(true rating)과 예측 평점(predicted rating)의 차이를 error항으로 정의
  - $e_{ui} = r_{ui}-x_i^Ty_u$
- 현재 gradient 반대방향으로 $x_u,y_i$를 update
  - $x_u \leftarrow  x_u + \gamma(e_{ui} \cdot y_i - \lambda \cdot x_u)$
  - $y_i \leftarrow  y_i + \gamma(e_{ui} \cdot x_u - \lambda \cdot y_i)$
- 구현이 쉽고, 계산이 빠름

### Alternating Least Squares
- 일반적으로 $x_u$와 $y_i$를 둘다 알 수 없는 경우가 많음
- 그렇다면 loss function이 convex하지 않음
  - Convex하다 = 최적해를 좀 더 빠르고 정확하게 찾을 수 음
- $x_u$와 $y_i$ 둘 중 하나를 고정하고 식을 quadratic(2차)식으로 최적화 문제를 풀 수 있음
- $x_u$와 $y_i$를 번갈아 고정시키면서, least-square(최소제곱)문제를 풀게 됨
- 병렬처리에 사용할 수 있음
   - $x_u$와 $y_i$를 독립적으로 계산하기 때문에 병렬화하는 데 장점이 있음
 - Implicit feedback 처리에 유리함
  - Explication data에 비해 dense하기 때문에 연산량이 많아짐(병렬처리 가능)

<br/>

## More on Matrix Factorization
- Matrix Completion 문제
- 여러 버전의 SVD 사용 가능
  - SVD++, thin SVD, compact SVD, truncated SVD, etc..
- Adding bias
  - $\hat{r_{ui}} = x_u^T$ x $y_i \rightarrow b_{ui} = \mu + b_i + b_u \rightarrow \hat{r_{ui}} = \mu + b_i + b_u + x_u^Ty_i$
- user u와 item i의 상호관계를 파악(기존 방법)
- user u와 item i의 개별 특성을 함께 표현하기 위해 bias term을 추가

<img src = "https://user-images.githubusercontent.com/48994965/188270362-9e52188d-c8a6-4609-b424-c937d23ee1f3.png" width="68%" height="68%">

### Additional Input Source
  - Behavior Information 등 추가 정보를 활용한 모델링 가능
  - $\Sigma_{i \in N(u)}y_i$: user u의 item i에 대한 implicit feedback
    - $N(u)$: 전체 item에 대한 user u의 implicit  feedback
  - $\Sigma_{a \in A(a)}x_a$: user u의 personal or non-item related information
    - 성별, 나이, 주소 등

<img src = "https://user-images.githubusercontent.com/48994965/188270544-14c04ed9-3b08-4a38-b8e2-eb003028582d.png" width="68%" height="68%">

### Temporal Dynamics
- 데이터를 시간의 변화에 따라 동적으로 반영하는 모델링
- t는 시간의 변화를 나타냄

<img src = "https://user-images.githubusercontent.com/48994965/188270630-425ae43d-7900-4b1e-9543-ad31c06cd84c.png" width="68%" height="68%">

### Input with Varying Confidence Lavels
- 데이터(ex.평점)가 통일한 가중치 또는 신뢰도가 아닌 상황을 모델링
- 대규모 광고에 영향을 받은 item이 자주 선택되는 경우
- Implicit feedback데이터에서 user가 실제로 선호하는지 판단하기 어려운 경우
  - 몇 번 지속적으로 tv를 시청했는지, 몇 번 구매가 이루어젺는지 등 

<img src = "https://user-images.githubusercontent.com/48994965/188270726-31d89c1d-9ec1-43e7-9238-3265985fda79.png" width="68%" height="68%">
