# 논문 정리

## Abstract  & Introduction
- 기존 Linear Matrix Factorization의 한계점을 지적
	- User와 Item의 rating matrix를 구현하여 linear하게 학습
- Neural Net기반의 Collaborative Filtering으로 non-linear한 부분을 커버
- user와 item관계를 보다 복잡하게 모델링 할 수 있음

## Contributions
- user와 item의 latent features를 모델링 하기 위한  Neural Network 구조를 제안(General Framework NCF)
- Matrix Factorization은 NCF의 특별한 케이스임을 증명하고,  Multi-layer Perecptron을 사용
- 실제 데이터를 활용하여 다양한 실험을 진행했으며, 이를 통해 NCF의 효과 증명

## Learining from implicit data
- M과  N은 user와 item수, rating 데이터가 아니라 0과 1의 binary 데이터
- user가 item을 관측했는지에 따라 0또는 1로 표현 → interaction이 있는지 여부를 표시함
	- 선호, 비선호를 나타내는 것이 아님(implicit feedback data)
-Interaction function f를 정의하고, user와 item간 interaction이 있는지 확률을 예측하는 문제
-2가지 object function(Point-wise loss와 Pair-wise loss) 모두 사용가능
	- Point-wise : 실제값과 예측값 차이를 최소화 (regression 문제)
	- Pair-wise : 1이 0보다 큰 값을 갖도록 마진을 최대화

## Matrix Factorization
- User-Item Interaction Matrix의 한계점
	- u는 실제 userdlau u4는 u1과 유사도가 높고 다음으로 u3과 유사도가 높음
	- 하지만 user latent space에서는 p4는 p1과 p2와 유사도가 높으며, p3과의 유사도가 가장 낮음
	- u4 → u1 → u3 → u2
	- p4 → p1 → p2 → p3
- User와 Item의 복잡한 관계를 low dimension에 표현하면서 문제 발생
- Dimension 크기를 키우면 overfitting 발생
- Non-linear한 Neural Network를 사용해서 복잡한 상관관계 표현

## Neural Collaborative Filtering - General Framework
- Input Layer : user와 item을 one-hot vector로 표현
- Embedding Layer : Sparse one-hot vector를 dense vector로 매핑
- Neural CF Layer : User Latent Vector와 Item Latent Vector를 concat해서 Layer를 통과
- Output Layer : User u와 Item i의 상관관계를 0과 1사이의 점수로 나타냄

## Learning NCF
- Label이 Binary이기 때문에, Bernoulli Distribution을 사용
- 아래 두 집단이 있음

- Loss Function은 Binary cross entropy를 사용
- 위의 L을 최소화 하는 파라미터를 찾음
- 학습은 SGD를 사용

## Generalized Matrix Factorization
- p와 q는 user와 item의 latent vector인데, 이를 element-wise곱하고, weight를 곱해준다
- a는 non-activation function(sigmoid)
- h^T는 내적할 때, 가중치역할을 하여 latent vector를 학습할 수 있게 만들고 중요도를 조절 하도록 함
- a가 1이고, h^T가 uniform vector이면 Matrix Factorization이 된다.


## Multi-layer Perceptron
- GMF보다 더 간단하게 user-item interaction 학습 가능
- W_x, b_x, a_x는 순서대로 weight matrix, bias vector, x번째 층 activation function임
- φ_1은 user와 item의 latent vector를 concat
- 이후 모든 φ_L은 weight matrix와 bias vector로 표현
- 최종식은 GMF와 동일한 구조


## Fusion of GMF and MLP
- GMF와 MLP에서 사용하는 latenti vector dimension이 다를 수 있음
- 최종 score는 MLP와 GMF의 output을 concat하여 사용
- MF의 linearity와 MLP의 non-linearity를 결합하여 장점만 취함


## Experiments
- MovieLens와 Pinterest 데이터를 사용
- Pinterest의 경우 데이터셋이 크지만 굉장히 Sparse함
	- 사용자의 20%가 pin 하나만 존재함
	- 최소 20개 이상의 pin을 가진 user로 필터해서 사용
- baseline
	- ItemPop: Popularity에 따라 아이템 순위를 매긴다
	- ItemKNN: Standard Item-based CF 방법을 implicit feedback에 적용
	- BPR: MF모델 최적화
	- eALS: 아이템 추천에서 state-of-the-art MF 방법


## Experiments: Performance Comparison
- 모든 부분에서 좋은 성능을 보여줌


## Conclusion
- Gerneral Framework NCF를 제안함; GMF, MLP, NeuralMF
- Linear 모델의 한계를 neural network를 사용해서 해결함
- MGF와 MLP의 장점을 합하여 NeuralMF를 제안했으며, 성능 향상에 기여
- User-Item interaction이 다루는 Collaborative Filtering 아이디어에 집중함