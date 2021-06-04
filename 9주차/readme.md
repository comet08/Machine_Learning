## 특성처리
### 범주형과 숫자형 특성
- pandas.get_dummies() = 숫자형 빼고 더미 변수를 생성
- sklearn.preprocessing.OneHotEncoder = 전부 범주형 데이터로 생각하여 더미 특성을 생성
- sklearn.compose.ColumnTransformer
- sklearn.compose.make_column_transformer
- - 특성마다 적용 방법 선택
- sklearn.preprocessing.KBinsDiscretizer = 구간 분할
- - (n_bins=구간수, strategy='' ... )
- sklearn.preprocessing.PolynomialFeatures = 다항식 추가
- - (degree = , include_bias= / 선형 회귀에서 곡선 회귀 가능해짐


- 원본 추가, 원본과의 곱 추가, 다항식 추가, 구간분할


## 특성 자동선택
### 일변량 통계
- 특성과 타깃 사이의 중요한 통계적 관계가 있는지 계산
- ANOVA 집단간의 평균 제곱 / 집단내의 평균제곱 = F값 / 클수록 클래스별로 다른 것
- sklearn.feature_selection.SelectPercentile, SelectKBest

### 모델 기반
- sklearn.feature_selection  import SelectFromModel  
- 결정트리는 feature_importances_, 선형모델은 coef_로 판단
- 중요성이 기준치를 넘으면 추가함 / threshold
- 일변량 통계는 각각의 특성을 처리하는 반면 모델 기반은 모든 특성을 고려한 결과

### 반복적 특징 선택
- sklearn.feature_selection.RFE
- 특성을 하나씩 추가하면서 모델을 만듦. / 모든 특성에서 하나씩 제거하면서 생성
- 메모리와 시간

### 전문가 지식 활용//
- 시계열 데이터..

## 검증
### 교차 검증
- cross_val_score
- cross_validation ( 점수와 시간을 dict로 반환 )

### 분할
- StratifiedKFold ( 클래스 비율 유지하며 분할 ) .split  계층ㅇ분할
- KFold
- LeaveOneOut ( 하나의 샘플이 하나의 폴드 )
- shufflesplit, StratifiedShuffleSplit
- train_rate / test_rate 를 설정함. 더한 비율이 1보다 작으면 서브 샘플링이 됨
- GroupKFold / 그룹 정보를 추가로 제공하여 데이터 엿보기를 방지함  
- ex) cross_val_score(model, x, y, groups, cv= GroupKFold(n_splits=5))

### 매개변수 그리드
- GridSearchCV 
- 매개변수 튜닝용
- param_grid를 사용
- cv = 으로 중첩 지정
- cross_val_score(GridSearchCV( ... ) , cv=n) 


## 평가 지표
- MSE 
- MAE
- R^2
- confusion_matrix, classification_report
- f1 score 2(정밀도 * 재현률) / (정밀도 + 재현률)
- 정밀도(precision) = 양성으로 예측한 것들 중에 진짜 양성
- 재현률(recall) = 실제 양성인 것들 중에 양성으로 예측한 것
- ... f1은 f1_micro, macro, weighted 등 여러 종류가 있음
- ROC = TPR / FPR
- TPR = 재현률 , FPR = 거짓양성 = FP / (FP + TN)
- ROC_AUC = ROC커브 아래의 너비



- 비즈니스 임팩트
- 입원 환자 수를 줄이기 위해, 사이트 유입을 늘리기 위해..
