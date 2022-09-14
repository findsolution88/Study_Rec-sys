## Abstract
- Factorization Machine은 SVM과 Factorization Model의 장점을 합친 모델
  - SVM은 큰 데이터를 Support Vector를 찾아서 예측(커널함수 적용)
  - Factorization Model의 예시: Matrix Factorization, Parallel factor analysis, specialized model(SVD++, PIRF or FPMC)
- Real valued Feature Vector를 활용한 **General Predictor**
  - Classification, Regression 문제 모두 활용 가능
- Factorization Machine의 식은 linear time임(계산복잡도가 높지 않음)
- 일반적인 추천 시스템은 special input data, optimization algorithm등이 필요함
- 반면, Factorization Machine은 어드 곳에서든 쉽게 적용 가능

<br/>

## Introduction
- Factorization Machine은 general predictor이며, high sparsity에서도 relable parameters를 예측할 수 있음
- Sparse한 상황에서 SVM은 부적적함
  - ***Cannot learn reliable parameters(hyperplanes) in complex(non-linear) kernel space***
- FM은 복잡한 interaction도 모델링하고, factorized parameterization을 사용
- Linear Complexity이며, linear number of parameters임

<br/>

## Contributions
<img src = "https://user-images.githubusercontent.com/48994965/190154279-bcfa44e1-fdc1-4f24-bae5-d17aedc1eb39.png" 
     width="60%"  height="60%">

- Factorization Machine은 위와같은 장점이 있음
- Sparse data(일반적인 상황)에서 parameter 학습이 가능
- Linear Complexity
- Gerneral Predictor로써 추천시스템 이외 다른 머신러닝에서 사용 가능

<br/>

## Prediction Under Sparsity
<img src = "https://user-images.githubusercontent.com/48994965/190155787-5664426d-17bb-4706-9b79-21429485d435.png" width="60%"  height="60%">

- 일반적으로 볼 수 있는 영화 평점 데이터의 설명
- Matrix Factorization은 user와 movie, 그리고 해당 rating망 사용함
- FM은 아래와 같이 더 많은 데이터를 활용할 수 있음

<img src = "https://user-images.githubusercontent.com/48994965/190156362-fe75e44e-5ef3-4bec-9284-2e5db86d0e7c.png" width="80%"  height="80%">

<br/>

## Factorization Machine(FM)
### Model Equatation (1)
<img src = "https://user-images.githubusercontent.com/48994965/190156802-137f750a-bc4d-45e9-9f6f-1119b34b320f.png" width="70%"  height="70%">

- $w_0\in \mathbb{R}, w\in \mathbb{R}^2, V\in \mathbb{R}^{n\times k} \to$ 추정해야할 파라미터
- 2-way FM(degree=2): 개별 변수 또는 변수간 interaction 모두 모델링
  - n-way도 가능
- 다항 회귀와 매우 흡사하지만, coefficient 대신 파라미터 마다 embedding vector를 만들어서 내적

<br/>

### Model Equatation (2)
<img src = "https://user-images.githubusercontent.com/48994965/190158283-a2378a9d-cbc1-437a-b0cd-62918650f5bc.png" width="70%"  height="70%">

<br/>

### Pairwise Interaction Computation
<img src = "https://user-images.githubusercontent.com/48994965/190160012-a4a999fc-f73b-4972-a127-fbf44d17c5cb.png" width="70%"  height="70%">

- Factorization of pairwise interaction
- 2개 변수에 직접적으로 연관있는 model parameter가 없음
- Pairwise interaction식을 정리하면 위와 같음

<br/>

## Factorization Machine as Predictor
<img src = "https://user-images.githubusercontent.com/48994965/190160881-c3ba6076-d106-48a6-93e4-6011259f3621.png" width="70%"  height="70%">

- FM은 Regression, Binary classification, Ranking문제를 해결할 수 있는 Gerneral Predictor
- 모든 케이스에서 L2 regurlarization terms를 사용하여 오버피팅을 방지할 수 있음

<br/>

## Learning Factorization Machine
<img src = "https://user-images.githubusercontent.com/48994965/190161517-c08b8777-754a-4fc9-a695-618127cd1548.png" width="70%"  height="70%">

- FM은 Stochastic gradient descent를 사용하여 학습하며 각각 추정해야하는 파라미터에 대해 미분하면 (4)와 같이 정의할 수 있음
- time complexity 또한 감소할 수 있음

<br/>

## ***d-way*** Factorization Machine
<img src = "https://user-images.githubusercontent.com/48994965/190162300-43d6e8e9-1a38-40b3-8394-1009e36ffaaa.png" width="50%"  height="50%">

- 2-way FM을 d-way FM으로 일반화 할 수 있음
- d-way FM 또한 computation cost는 linear함
- http://www.libfm.org에 다양하게 활용할 수 있음 module을 공개함

<br/>

## Additional Experiments
<img src = "https://user-images.githubusercontent.com/48994965/190162657-ab7b6bb9-21bd-490b-9229-8a3498a48f14.png" width="80%"  height="80%">

- Fig.2. Dimensionallity가 커질 수록 FM은 성능을 보장하나 SVM은 학습에 실패함
- Fig.3. PITF는 그 당시 SOTA로서 FM이 동일한 성능을 냄
  - PITF는 ECML Discovery Challenge 2009, Task2에만 적합함
  - 결국, FM은 Gerneral Predictor이기 SOTA와 성능은 비슷하나 활용성이 더 넓음

<br/>

## Conclusion
- Factorized Interaction을 사용하여 feature vector x의 모든 가능한 interaction을 모델링함
- (High) Sparse한 상황에서 interaction을 추정할 수 있음
  - Unobserved interaction에 대해서도 일반화 할 수 있음
- 파라미터 수, 학습과 예측 시간 모두 linear함(Linear Complexity)
- 이는 SGD를 활용한 최적화를 진행할 수 있고 다양한 loss function을 사용할 수 있음
- SVM, matrix, tensor and spcialized factorization model보다 더 나은 성능을 증명함




