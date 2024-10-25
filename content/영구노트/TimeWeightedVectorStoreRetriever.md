---
tags:
  - 완성
  - Python
  - Langchain
  - RAG
  - Retriever
aliases:
  - 시간 가중 벡터저장소 리트리버
date: 2024-09-27
title: TimeWeightedVectorStoreRetriever
---
작성 날짜: 2024-09-27
작성 시간: 20:30


----
## 내용(Content)

### TimeWeightedVectorStoreRetriever

>[!summary]
> 의미론적 유사성과 시간에 따른 감쇠를 결합해 사용하는 검색기이다. `유사성`과 `최신` 데이터 를 모두 고려해서 문서를 제공한다.


### 문서 접근에 적용되는 스코어링 알고리즘

스코어링 알고리즘은 다음과 같다.

$$
semantic\_similarity + (1.0 - decay\_rate)^{hours\_passed}
$$


기본 유사도에서 감쇠율을 지나간 시간을 승수로 되어 있다. 시간은 **객체가 마지막으로 접근된 후부터 현재까지 경과한 시간**을 의미하고 **Hour 단위**로 동작한다.

감쇠율이 낮다는 것은 유사 검색과 거의 같아진다는 의미이고, 오래된 데이터를 보다 오래 기억한다는 것을 의미한다. 반대로 감쇠율이 높다는 것은 시간에 지남에 따라 스코어링이 점점 낮아지는 것을 의미하고 이는 오래된 데이터는 금방 잊고, 최대한 최신 데이터만 가져오려고 할 것이다.

### code

##### TimeWeightedVectorStoreRetrieve Retriever 설정하기

```python
from langchain.docstore import InMemoryDocstore
from langchain.retrievers import TimeWeightedVectorStoreRetriever
from langchain.vectorstores import FAISS
from langchain_openai import OpenAIEmbeddings
import faiss

embeddings = OpenAIEmbeddings(model="text-embedding-3-small")

embedding_size = 1536
index = faiss.IndexFlatL2(embedding_size)
vectorstore = FAISS(embeddings, index, InMemoryDocstore({}), {})

retriever = TimeWeightedVectorStoreRetriever(
    vectorstore=vectorstore, decay_rate=0.1, k=1
)
```

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










