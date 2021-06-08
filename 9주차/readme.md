## 특성처리
### 범주형과 숫자형 특성
- `pandas.get_dummies()` = 숫자형 빼고 더미 변수를 생성
- `sklearn.preprocessing.OneHotEncoder` = 전부 범주형 데이터로 생각하여 더미 특성을 생성
- `sklearn.compose.ColumnTransformer`
- `sklearn.compose.make_column_transformer`
- - 특성마다 적용 방법 선택
- `sklearn.preprocessing.KBinsDiscretizer` = 구간 분할
- - (n_bins=구간수, strategy='' ... )
- - 결정트리는 특성별로 가장 좋은 구간을 학습해서 구간 나누기가 도움 되지 않음.
- - 고차원 데이터 선형 모델 구축시 유용

- `sklearn.preprocessing.PolynomialFeatures` = 다항식 추가
- - 데이터의 특성 강화
- - (degree = , include_bias= / 선형 회귀에서 곡선 회귀 가능해짐
- - ex) degree = 2, feature = 13이라면
- - transform 후 = (13 + 14C2 + 1) = 105
- - ` n = 13 + 2 - 1 / k = 2 `


- - 원본 추가 : 구간 기울기가 같음
- - 원본과의 곱 추가 : 구간별 기울기와 절편이 다름
- - 다항식 추가 

## 특성 자동선택
- 특성을 생성하면서 > 모델이 복잡해지고 과대 적합 가능성이 생김.



### 일변량 통계
- `sklearn.feature_selection.SelectPercentile, SelectKBest`
- 특성과 타깃 사이의 중요한 통계적 관계가 있는지 계산
- ANOVA ( Analysis of Variance 분산분석)
-  ! 집단간의 평균 제곱 / 집단내의 평균제곱 = F값 / 클수록 클래스별로 다른 것
- 각각의 특성을 독립적으로 계산
- `sklearn.feature_selection.f_classif , f_regression`
- `SelectPercentile(score_func=f_classif, percentile=50)`


### 모델 기반
- `sklearn.feature_selection  import SelectFromModel`
- 결정트리는 feature_importances_, 선형모델은 coef_로 판단
- 중요성이 기준치를 넘으면 추가함 / threshold = '...' median ( 절반 이상 ) 
- 모델 기반은 모든 특성을 고려한 결과로 특성 간의 상호작용을 반영함

### 반복적 특징 선택
- `sklearn.feature_selection.RFE`
- 특성을 하나씩 추가하면서 모델을 만듦. / 모든 특성에서 하나씩 제거하면서 생성
- 메모리와 시간
- 중요도가 높은(반복-추가) / 낮은(재귀-제거)
- `RFE(model, n_features_to_select)`

### 전문가 지식 활용//
- 항공료 예측과 같은 ...
- 자전거 대여 횟수 예측
- 날짜 + 시간 .. .astype(..)
- 시간
- 시간 + 요일
- 요일은 ohe..
- 상호작용 특성

## 검증 sklearn.model_selection
### 교차 검증
- `cross_val_score`
- `cross_validation` ( 점수와 시간을 dict로 반환 )
- 훈련 데이터를 k개 폴드로 분할하여 하나의 세트는 테스트/ 나머지는 훈련으로 사용

- train_test_split은 무작위로 데이터를 나눔.
- 교차 검증은 모든 폴드가 테스트 대상이라 공평함. 최악과 최선
- 하지만 연산 비용의 증가한다.

### 분할
- `StratifiedKFold` ( 클래스 비율 유지하며 분할 ) .split  계층ㅇ분할
- `KFold`
- `LeaveOneOut` ( 하나의 샘플이 하나의 폴드 )
- `shufflesplit, StratifiedShuffleSplit`
- train_size / test_size 를 설정함. 더한 비율이 1보다 작으면 서브 샘플링이 됨
- `GroupKFold`
- 타깃에 따라 폴드를 나누지 않고 입력에 따라 나눠야할 경우
- 그룹 정보를 추가로 제공하여 데이터 엿보기를 방지함  
- ex) cross_val_score(model, x, y, groups, cv= GroupKFold(n_splits=5))

### 매개변수 그리드
- GridSearchCV 
- 매개변수 튜닝용 / 가장 성능이 높은 조합 찾기
- param_grid를 사용
- 중첩 교차 검증 가능
- `cross_val_score(GridSearchCV( ... ) , cv=n) `
- cv_results_ : 그리드 서치 결과를 저장한 딕셔너리
- best_score_, best_params_, best_estimator_

- 연산에 많은 비용을 소모
- 병렬화! n_jobs로 cpu 코어수를 지정할 수 있음

- Validation set
- 최종 평가를 위해 파라미터 셋팅에 사용되지 않은 독립된 데이터 세트 필요!
- 검증 세트를 파라미터 셋팅에 사용하고 검증 + 훈련으로 모델을 만든 후 테스트 세트로 평가!

## 평가 지표
- 회귀 : R^2, MSE, MAE
- 분류 : accuracy, precision, recall, f1 score, roc

| | 음성예측 | 양성예측 |
| --- | --- | --- |
| 음성 | TN | FP |
|  양성 |FN | TP |

- `metrics confusion_matrix, classification_report`
- f1 score = 2(정밀도 * 재현률) / (정밀도 + 재현률)
- 정밀도(precision) = 양성으로 예측한 것들 중에 진짜 양성
- 재현률(recall) = 실제 양성인 것들 중에 양성으로 예측한 것
- ... f1은 f1_micro, macro, weighted 등 여러 종류가 있음
- ROC = TPR / FPR
- TPR = 재현률(진짜 양성 비율) , FPR = 거짓 양성 비율 = FP / (FP + TN)
- ROC_AUC = ROC커브 아래의 너비 / roc_auc_score( ..
- ROC 계열은 decision_function, predict_proba로 계산


- 불균형 데이터
- 한 클래스가 다른 클래스보다 샘플 수가 월등히 많을 경우..
- DummyClassifier로 분류해도 정확도가 높게 나옴 > 다른 평가 방법과 함께

- 비즈니스 임팩트
- 입원 환자 수를 줄이기 위해, 사이트 유입을 늘리기 위해..
