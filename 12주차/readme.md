## PipeLine
- 여러 처리 단계를 하나의 객체로 묶어주는 클래스
- 정보 누설 방지!
- `sklearn.pipeline.Pipeline `
- `sklearn.pipeline.make_pipeline`

- pipe.steps
- 파이프라인과 그리드 서치를 함께 사용시 param_grid의 매개변수 이름을 svm__C 의 형식으로 바꿔야 함
- 파이프라인에 들어갈 추정기들은 마지막을 제외하고 transform method를 가지고 있어야 함.

- 각 단계에는 pipe.named_steps["단계명"] 으로 접근한다.

-  전처리가 있는 매개변수 선택에서는 올바른 교차 검증을 위해 필수
-  모델의 매개변수 + 전처리 과정의 매개변수를 동시 탐색 / 가능한 모든 조합 시도
-  탐색 범위가 커지면 시간이 많이 소요됨

```
pipe = Pipeline([('preprocessing', StandardScaler()), ('classifier', SVC())])
param_grid = [
  {'classifier': [SVC()], 'preprocessing': [StandardScaler()], 'classifier__gamma': 
  [0.001, 0.01, 0.1, 1, 10, 100], 'classifier__C': [0.001, 0.01, 0.1, 1, 10, 100]},
  {'classifier': [RandomForestClassifier(n_estimators=100)],
  'preprocessing': [None], 'classifier__max_features': [1, 2, 3]}]
```



## Text Data
- 텍스트 데이터를 처리하는 방법
- 스팸, 부정거래탐지, 감정분석


### BOW(bag of words)
1. 토큰화(공백, 구두점 기준)
2. 어휘 사전 구축(알파벳 순서로)
3. 인코딩 / 사전의 단어의 빈도수를 확인

- `sklearn.feature_extraction.text.CountVectorizer`
- 출력은 각 문서에서 나타난 단어의 횟수가 담긴 벡터
- 희소 행렬로 나옴 / vocabuluary_ 에 저장
- min_df로 최소 문서 설정 가능 / 특성의 개수가 줄어 처리속도 개선. 희귀 단어나 철자 오류 삭제

- stop_words로 불용어 사용 가능 / 불용어란 너무 빈번하게 사용하여 유용하지 않은 단어들

- 간단한 방법.
- 하지만 의미 없는 특성(숫자들)을 많이 생성
- 스팸, 감정분석

### Tf-idf
![image](https://user-images.githubusercontent.com/51084402/121146964-65d62e00-c87b-11eb-894e-47982ce8ab8e.png)

- tfidf를 구하고 L2 정규화
- 특정 문서에 자주 나타나는 단어는 그 문서를 잘 설명하는 단어! 가중치 높임


- `sklearn.feature_extraction.text.TfidfVectorizer`
- tf idf가 낮은 특성은 전체 문서에 걸쳐서 매우 많이 나타나거나, 조금만 사용되거나, 매우 긴 문서에서만 사용
- tf idf가 높은 특성은 특정한 것을 나타내는 경우가 많음
- idf 가 낮으면 자주 나타나서 덜 중요하다고 생각되는 단어


### ngram
- `ngram_range` 매개변수로 토큰의 범위를 설정
- 변별력!
- be / 유니그램  be fool / 바이그램 to be fool / 트라이그램


### 어간 추출, 표제어 추출
- 어간을 추출하는 방법
- 표제어 추출(단어 형태 사전을 사용하여 문장에서 단어 역할을 고려하는 처리 방식)
- `nltk, spacy`
- `nltk의 PorterStemmer`
- spacy의 load ... 
- `spacy = spacy.load_..(document) / token.lemma_`
- `stemmer.stem(token.norm_.lower()) for token in spacy`
- 표제어의 성능이 더 좋은..


### LDA
- sklearn.decomposition.LatenDirichletAllocation
- 단어의 그룹(토픽)을 찾음 / 토픽 모델링
- n_components로 그룹 수 설정 가능
- 훈련 샘플이 적고 특성이 많을때 유용
- 지도 학습을 위한 압축 표현으로 사용
