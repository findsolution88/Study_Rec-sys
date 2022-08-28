## Model based collaborative filtering


### 특징
- 머신러닝을 활용한 추천알고리즘의 일종
- 주어진 데이터를 활용하여 모델을 학습함
학습과정(Training)에서 모델이 데이터를 배워서 데이터 정보를 압축한다.
- 항목간 유사성에서 벗어나, 데이터의 패턴을 학습
- 데이터 크게 또는 데이터의 특징(Feature)을 동적으로 활용가능함
- 유저의 잠재적 특성을 파악하는 모델(Latent Factor Model)


### 장점
- 모델의 크기
	- 수많은 데이터로 구성된 행렬(ex. Rating matrix)보다 압축된 형태로 저장
- 모델의 학습과 예측 속도
	- 데이터 전처리와 학습과정으로 미리 모델 준비 -> 준비된 모델로 예측
- 모델의 과적합 방지
	- 데이터를 다양하게 학습할 수 있으며, 새로운 추천 가능성이 있음


### 단점
- Sparse data
	- 유저-아이템 간 데이터 중 사용 가능한 데이터는 전체 데이터 중 비율이 작음
	- 유저-아이템 간 데이터에서 표시되지 않은 항목은 새로운 추천을 하기 어렵다(Cold-start)
- 공통의 유저 또는 아이템이 선택된 데이터를 확보하기 어렵다


### 종류
- Association Rule Mining
- Matrix Factorization
	- SVD(Singular Value Decomposition), ALS(Alternating Least Square)
- Probablistic Model
	- Clustering
	- Bayes Rules
- Etc.
	- SVM, Regression Methods(Logistc regression), Deep Learning
