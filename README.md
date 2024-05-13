### 최종 프로젝트 4개 진행
- Regression 프로젝트
  - <a href="https://github.com/SOYOUNGdev/project-machine_learning/wiki/First-Project-(Regression-%E2%80%90-Linear)"> 선형 </a> 
  - <a href="https://github.com/SOYOUNGdev/project-machine_learning/wiki/Second-Project-(LGBMRegressor)"> 비선형 </a>
  - <a href="https://github.com/SOYOUNGdev/project-machine_learning/wiki/Thrid-Project-(high-dimension-%E2%80%90-PCA)"> 고차원 </a>
- Classifier 프로젝트
  - <a href=""> 이진 분류 (고차원) </a>

---

### 데이터 탐색 (클러스터링)
- 상관관계 (corr())
  1. 종속 변수 (타겟 데이터)
     : 한 개의 독립 변수라도 종속 변수와의 상관관계성이 드러나야한다.
  2. 독립 변수 (타겟을 제외한 feature)
     : 다른 독립 변수와의 상관 관계성이 0에 가까워야한다.
     > 히트맵 (대각선만 가장 진한 색, 나머지는 흰색에 가까워야한다.)  
     > 만일 독립변수들 간의 상관관계성이 높다면,
     >> 1. fit()을 통해 feature_importance를 알아낸다.
     >> 2. 종속 변수와의 상관관계가 더 낮은 종속변수를 제거한다.
     >> 3. 분포도 확인한다.

### 데이터 전처리
- 종속 변수
  1. 언더 샘플링(sample(random_state))
  2. 오버 샘플링(SMOTE)
  ex) 타겟 데이터가 너무 많은 차이가 날 때는 (90% : 10%) 언더 샘플링을 해야한다.  
      아니라면 오버 샘플링도 괜찮다.
- 독립 변수
  : 언더 샘플링만 진행
  단, 비중이 맞지 않는 feature가 1~2개 정도라면 괜찮지만 그 이상일 경우, 너무 많은 데이터가 사라질 위험이 있다.  
  이 경우, 해당 독립 변수의 corr과 importance를 비교하여 제거하는 것이 좋을 수도 있다.

### 평가
[이진 분류]
- 재현율
- 정밀도
- 0.5는 랜덤이다 -> Trade-off (threshold를 조정하여 한쪽으로 치우치게끔 한다.)
[다중 분류]
- 재현율, 정밀도 조정 불가하다.
- 모든 타겟데이터를 꼭 들고 갈 필요없다.
  ex) 타겟데이터 0, 1, 2, 3, 4(3.7%) 라면 굳이 SMOTE를 통해 오버샘플링을 진행하지 않고 날려도 된다. 그 후, 4에 대한 데이터를 더 수집한다.
- F1 Score는 0.7에 가까우면 좋다.

#### 이상치 제거는
describe()를 통해 mean값과 50% 값을 비교하여, 두 수치의 차이가 10% 미만이라면 이상치 제거를 진행하지 않아도 된다.  
차이가 많이 난다면 제거해보는 것이 좋다.

#### feature engineering은
- 독립 변수들과 종속 변수의 상관 관계가 분명 있을 것이라 예상되지만, 훈련 후 모델의 성능이 높지 않을 경우 진행한다.
- 즉, Total score와 Target의 상관관계 파악
> Total score = 수치형(스케일링 진행 후의 값 * 가중치) + 범주형  
>> 가중치 = feature importance + 1  
>> Total score를 구한 뒤, Target과 비교한다.  

---
### 회귀(regression) 실습
- 데이터 탐색 후, 훈련 진행하여 R2 score 확인
- 선형, 다항, 비선형 모델 모두 R2 score가 낮다면 OLS와 VIF를 확인해보고 feature들의 비중을 맞추거나 알맞게 삭제한다.
- 이후 다시 훈련 진행하여 이전과 비교한다.

---
#### 과적합 유무 판단
- 파이토치 이용하여 loss값 비교!
loss_history = []
loss_history.append(loss.item())

- 시각화 진행
plt.plot(x=epoch, y=train_loss)
plt.plot(x=epoch, y=test_loss)
plt.show()
- 시각화 진행하였을 때, test의 loss값이 올라가기 시작하는 해당 부분까지 규제를 걸어주어야 한다.

