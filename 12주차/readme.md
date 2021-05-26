## PipeLine
- 여러 처리 단계를 하나의 추정기로 묶어주는 클래스
- sklearn.pipeline.Pipeline 
- sklearn.pipeline.make_pipeline

- 파이프라인과 그리드 서치를 함께 사용시 param_grid의 매개변수 이름을 svm__C 의 형식으로 바꿔야 함
- 파이프라인에 들어갈 추정기들은 마지막을 제외하고 transform method를 가지고 있어야 함.

- 각 단계에는 pipe.named_steps["단계명"] 으로 접근한다.



## Text Data
- 텍스트 데이터를 처리하는 방법

### BOW(bag of words)
1. 토큰화
2. 어휘 사전 구축(알파벳 순서로)
3. 인코딩 / 사전의 단어의 빈도수를 확인
- sklearn.feature_extraction.text.CountVectorizer
- 출력은 각 문서에서 나타난 단어의 횟수가 담긴 벡터
- 희소 행렬로 나옴
- get_feature_names()로 어휘 사전의 단어들을 리스트로 가져올 수 있음
- min_df로 최소 문서 설정 가능
- stop_words로 불용어 사용 가능 / 불용어란 너무 빈번하게 사용하여 유용하지 않은 단어들

- 간단한 방법.
- 하지만 의미 없는 특성(숫자들)을 많이 생성하고



### Tf-idf
- tfidf를 구하고 L2 정규화
- sklearn.feature_extraction.text.TfidfVectorizer
- tf idf가 낮은 특성은 전체 문서에 걸쳐서 매우 많이 나타나거나, 조금만 사용되거나, 매우 긴 문서에서만 사용
- tf idf가 높은 특성은 특정한 것을 나타내는 경우가 많음
- idf 가 낮으면 자주 나타나서 덜 중요하다고 생각되는 단어


### ngram
- ngram_range 매개변수로 토큰의 범위를 설정
- be / 유니그램  be fool / 바이그램 to be fool / 트라이그램


### 어간 추출, 표제어 추출
- 어간을 추출하는 방법
- 표제어 추출(단어 형태 사전을 사용하여 문장에서 단어 역할을 고려하는 처리 방식)
- nltk, spacy
- nltk의 PorterStemmer
- spacy의 load ... 
- spacy = spacy.load_..(document) / token.lemma_
- stemmer.stem(token.norm_) for token in spacy
- 표제어의 성능이 더 좋은..


### LDA
- sklearn.decomposition.LatenDirichletAllocation
- 단어의 그룹(토픽)을 찾음
- n_components로 그룹 수 설정 가능
- 
