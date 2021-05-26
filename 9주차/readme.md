## 특성처리
### 범주형과 숫자형 특성
- pandas.get_dummies() = 숫자형 빼고 더미 변수를 생성한다
- sklearn.preprocessing.OneHotEncoder = 전부 범주형 데이터로 생각하여 더미 특성을 생성한다
- sklearn.compose.ColumnTransformer
- sklearn.compose.make_column_transformer
- - 특성마다 적용 방법 선택
- sklearn.preprocessing.KBinsDiscretizer = 구간 분할
- - (n_bins=구간수, strategy='' ... )
- sklearn.preprocessing.PolynomialFeatures = 다항식 추가
- - (degree = , include_bias= / 선형 회귀에서 곡선 회귀 가능해짐


## 특성 자동선택
### 일변량 통계
- 특성과 타깃 사이의 중요한 통계적 관계가 있는지 계산
- ANOVA 집단간의 평균 제곱 / 집단내의 평균제곱 = F값 / 클수록 클래스별로 다른 것
- sklearn.feature_selection.SelectPercentile, SelectKBest

### 모델 기반
- sklearn.feature_selection  import SelectFromModel 
- 결정트리는 feature_importances_, 선형모델은 coef_를 보고 모든 특징을 한꺼번에 고려함
- 
### 반복적 특징 선택
- sklearn.feature_selection.RFE
- 특성을 하나씩 추가하면서 모델을 만듦. / 모든 특성에서 하나씩 제거하면서 생성

### 전문가 지식 활용//


## 검증
### 교차 검증
- cross_val_score
- cross_validation ( 점수와 시간을 dict로 반환 )

### 분할
- StratifiedKFold ( 클래스 비율 유지하며 분할 ) .split
- KFold
- LeaveOneOut ( 하나의 샘플이 하나의 폴드 )
- shufflesplit, StratifiedShuffleSplit
- GroupFold / 그룹 정보를 추가로 제공하여 데이터 엿보기를 방지함

### 매개변수 그리드
- GridSearchCV 
- 매개변수 튜닝용
- param_grid를 사용
