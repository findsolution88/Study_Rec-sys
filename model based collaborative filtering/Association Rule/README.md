## 상호 연관관계 예시
|User|구매목록|
|---|---|
|1|우유, 쥬스, 샌드위치|
|2|우유, 빵, 샌드위치|
|3|아메리카노, 초콜렛, 쿠키|
|4|아메리카노, 우유, 초콜렛, 샌드위치|
|5|라떼, 샌드위치|

- 위 테이블을 통해 상호 연관관계를 확인할 수 있음
  - 구매목록: {우유, 샌드위치}, {아메리카노, 초콜렛}
<br><br><br><br>
## Association Rule
- 데이터의 모델
  - **데이터의 관계, 접근과 흐름** 파악을 위한 추상화된 모형
- 데이터의 여러 특징을 파악해서 모형화 -> 모델링
- **데이터 간의 연관 법칙**을 찾는 Data mining기법 중 하나
- 기존 데이터를 기반으로 Association Rule을 만든다
<br><br><br>
### Association Rule Mining
1. 정의
    - Minimum Support와 Minimum Confidence 값을 넘는 Rule을 찾는 과정
    - 데이터에서 흥미로운 관계를 찾는 Rule-Based Machine Learning 기법중 하나
    - 특정 Measure를 통해 흥미도(interestingness)를 평가하여 주요 Rule을 찾는 과정
2. Association Rule의 Support(지지도)
    - 데이터 관계 설정을 위해 아이템이 동시에 발생할 확률
    - 전체 데이터 중 규칙(A,B)를 포함하는 데이터 비율
3. Association Rule릐 Confidence(신뢰도)
    - 특정 아이템 A가 선택된 상태에서 다른 아이템 B를 선택할 확률
    - (A,B)의 관계를 가정하고, A를 선택한 사람이 B를 선택할 비율
4. Association Rule의 Lift(향상도)
    - (A,B)의 관계를 직접적으로 나타내는 measurement
    - 1보다 크면, 이어서 B를 선택할 확률이 높고, 1보다 작다면 확률이 높지 않음

<br><br>
#### Association Rule Mining - Support
- Support <br>
$$support(A \to  B) = \frac{number\ of\ \left(A\cap B\right)}{number\ of\ data}$$
- Support가 1에 가까울수록 A와 B의 관계가 중요하다는 것을 의미함
- 0에 가까운 연관관계를 먼저 제거 -> 자주 발생하지 않는 Case 제거
- $support(A \to  B)$와 $support(B \to  A)$의 차이점 파악 불가

<br><br>
#### Association Rule Mining - Confidence
- Support와 같이 0~1 사이 값
$$confidence(A \to  B) = \frac{number\ of\ \left(A\cap B\right)}{number\ of\ A}$$
- A를 선택했을 때, B를 선택할 확률
- 1에 가까울 수록 A는 B에 많은 영향을 받는다 -> minimum support중 가장 큰 Confidence를 선택
- $support(A \to  B)$와 $support(B \to  A)$와 다르게 A와 B사이의 관계 파악이 가능

<br><br>
#### Association Rule Mining - Lift
- Lift
$$lift(A \to  B) = \frac{confidence\left(A \to B\right)}{support\ \left(B\right)}$$
- 0과 1사이의 확률값이 아닌 A와 B사이의 관계를 파악하는 용도로 사용됨
- lift(A→B)<1 : 상호대체 -> A와 B는 반비례
- lift(A→B)>1 : 상호보완 -> A와 B는 정비례
- lift(A→B)=1 : 독립 -> A와 B는 서로에게 영향을 끼치지 않는다

<br><br>
#### Association Rule Mining 예시
- Data set


|User|Movie Watched|
|---|---|
|1|A,B,C|
|2|A,C|
|3|A,D|
|4|B,E,F|
|5|B,C,D|
- Support


|Frequent Pattern|Support|
|---|---|
|A|3/5 = 0.6|
|B|3/5 = 0.6|
|A,C|2/5 = 0.4|
|B,C|2/5 = 0.4|

- (A → C)
- support(A→C) = 0.4
- support(A) = 0.6
- $confidence=\frac{suppot\left(A\to B\right)}{support\left(A\right)} = 0.67$


<br><br>
### More on Association Rule Mining
- Brute Force
  - 모든 연관관계를 평가
  - Minimun Support Threshold, Minimun Confidence Threshold
- Frequent Itemset Generation
  - 빈도수가 높은 관계 위주로 후보군을 축소하여 Rule mining 수행
  - Apriori Principle: 데이터 발생빈도를 바탕으로 데이터간의 연관관계 파악에 사용
- 이산형 변수로 데이터 Profile을 통해 association rule 적용 가능
