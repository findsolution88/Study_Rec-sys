# 논문 정리

## Abstract  & Introduction
- 기존 Linear Matrix Factorization의 한계점을 지적
	- User와 Item의 rating matrix를 구현하여 linear하게 학습
- Neural Net기반의 Collaborative Filtering으로 non-linear한 부분을 커버
- user와 item관계를 보다 복잡하게 모델링 할 수 있음

<br/><br/>

## Contributions
- user와 item의 latent features를 모델링 하기 위한  Neural Network 구조를 제안(General Framework NCF)
- Matrix Factorization은 NCF의 특별한 케이스임을 증명하고,  Multi-layer Perecptron을 사용
- 실제 데이터를 활용하여 다양한 실험을 진행했으며, 이를 통해 NCF의 효과 증명

<br/><br/>


## Learining from implicit data
<img src = "https://user-images.githubusercontent.com/48994965/187858321-69539ef0-8782-44d7-a331-c0e8cea08086.png" width="70%" height="70%">

- M과  N은 user와 item수, rating 데이터가 아니라 0과 1의 binary 데이터
- user가 item을 관측했는지에 따라 0또는 1로 표현 → interaction이 있는지 여부를 표시함
	- 선호, 비선호를 나타내는 것이 아님(implicit feedback data)
-Interaction function f를 정의하고, user와 item간 interaction이 있는지 확률을 예측하는 문제
-2가지 object function(Point-wise loss와 Pair-wise loss) 모두 사용가능
	- Point-wise : 실제값과 예측값 차이를 최소화 (regression 문제)
	- Pair-wise : 1이 0보다 큰 값을 갖도록 마진을 최대화

<br/><br/>


## Matrix Factorization
<img src = "https://user-images.githubusercontent.com/48994965/187858584-897253b7-3d87-40b6-aa4c-03acf05a4b80.png" width="60%" height="60%">

- User-Item Interaction Matrix의 한계점
	- u는 실제 userdlau u4는 u1과 유사도가 높고 다음으로 u3과 유사도가 높음
	- 하지만 user latent space에서는 p4는 p1과 p2와 유사도가 높으며, p3과의 유사도가 가장 낮음
	- u4 → u1 → u3 → u2
	- p4 → p1 → p2 → p3
- User와 Item의 복잡한 관계를 low dimension에 표현하면서 문제 발생
- Dimension 크기를 키우면 overfitting 발생
- Non-linear한 Neural Network를 사용해서 복잡한 상관관계 표현

<br/><br/>


## Neural Collaborative Filtering - General Framework
<img src = "https://user-images.githubusercontent.com/48994965/187858656-abd6ac94-16e0-42bf-bd0f-0c3984876ba7.png" width="65%" height="65%">

- Input Layer : user와 item을 one-hot vector로 표현
- Embedding Layer : Sparse one-hot vector를 dense vector로 매핑
- Neural CF Layer : User Latent Vector와 Item Latent Vector를 concat해서 Layer를 통과
- Output Layer : User u와 Item i의 상관관계를 0과 1사이의 점수로 나타냄

<br/><br/>


## Learning NCF
<img src = "https://user-images.githubusercontent.com/48994965/187860126-0b367433-f2c7-4365-a697-3eeeadff0327.png" width="50%" height="50%">

- Label이 Binary이기 때문에, Bernoulli Distribution을 사용
- 아래 두 집단이 있음
	- $\Upsilon = y_u\,_i = 1$
	- $\Upsilon^- = y_u\,_i = 0$
- Loss Function은 Binary cross entropy를 사용
- 위의 L을 최소화 하는 파라미터를 찾음
- 학습은 SGD를 사용

<br/><br/>

## Generalized Matrix Factorization
- Matrix Factorization이 Neural CF의 특별 케이스가 됨
<img src = "https://user-images.githubusercontent.com/48994965/187860408-3b1030b5-3d27-48f2-94e1-aa97f92c599d.png" width="30%" height="30%">

- p와 q는 user와 item의 latent vector인데, 이를 element-wise곱하고, weight를 곱해준다

<img src = "https://user-images.githubusercontent.com/48994965/187860687-9be2beb0-89f0-4294-9dca-c6c4489e6c29.png" width="30%" height="30%">

- a는 non-activation function(sigmoid)
- h^T는 내적할 때, 가중치역할을 하여 latent vector를 학습할 수 있게 만들고 중요도를 조절 하도록 함
- a가 1이고, h^T가 uniform vector이면 Matrix Factorization이 된다.

<br/><br/>

## Multi-layer Perceptron
<img src = "https://user-images.githubusercontent.com/48994965/187860781-c8d2a5c0-1ddd-4136-be03-a6d112438685.png" width="60%" height="60%">

- GMF보다 더 간단하게 user-item interaction 학습 가능
- W_x, b_x, a_x는 순서대로 weight matrix, bias vector, x번째 층 activation function임
- φ_1은 user와 item의 latent vector를 concat
- 이후 모든 φ_L은 weight matrix와 bias vector로 표현
- 최종식은 GMF와 동일한 구조

<br/><br/>

## Fusion of GMF and MLP
<img src = "https://user-images.githubusercontent.com/48994965/187860865-f22381eb-7481-4051-81f3-54f1d59c551a.png" width="100%" height="100%">

- GMF와 MLP에서 사용하는 latenti vector dimension이 다를 수 있음
- 최종 score는 MLP와 GMF의 output을 concat하여 사용
- MF의 linearity와 MLP의 non-linearity를 결합하여 장점만 취함

<br/><br/>

## Experiments
<img src = "https://user-images.githubusercontent.com/48994965/187860937-8992db77-9fc3-42fa-ab8e-ab338685ee45.png" width="70%" height="70%">

- MovieLens와 Pinterest 데이터를 사용
- Pinterest의 경우 데이터셋이 크지만 굉장히 Sparse함
	- 사용자의 20%가 pin 하나만 존재함
	- 최소 20개 이상의 pin을 가진 user로 필터해서 사용
- baseline
	- ItemPop: Popularity에 따라 아이템 순위를 매긴다
	- ItemKNN: Standard Item-based CF 방법을 implicit feedback에 적용
	- BPR: MF모델 최적화
	- eALS: 아이템 추천에서 state-of-the-art MF 방법

<br/><br/>

## Experiments: Performance Comparison
<img src = "https://user-images.githubusercontent.com/48994965/187861012-77fc4678-f81e-48b7-84ea-476c9e6fcd84.png" width="80%" height="80%">

- 모든 부분에서 좋은 성능을 보여줌

<br/><br/>

## Conclusion
- Gerneral Framework NCF를 제안함; GMF, MLP, NeuralMF
- Linear 모델의 한계를 neural network를 사용해서 해결함
- MGF와 MLP의 장점을 합하여 NeuralMF를 제안했으며, 성능 향상에 기여
- User-Item interaction이 다루는 Collaborative Filtering 아이디어에 집중함
