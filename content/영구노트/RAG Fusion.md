---
tags:
  - 완성
  - Python
  - Langchain
  - Retriever
  - LLM
aliases: 
date: 2024-10-04
title: RAG Fusion
---
작성 날짜: 2024-10-04
작성 시간: 20:08


----
## 내용(Content)

### RAG Fusion

>[!summary]
> 다중 쿼리 혹은 검색 결과를 통합하는 과정에서 Reciprocal Rank Fusion(RRF) 알고리즘을 사용하여 검색된 문서를 재정렬하는 방식

RRF를 활용하면 검색 결과를 보다 효율적으로 재결합하고 정렬한다.
RRF의 핵심 개념은 여러 독립적인 문서들 간의 관계에서 각 문서들의 랭킹을 계산하고 , 상호 순위를 매겨 문서들의 최종 순위를 결정하는 것이다.

### RRF(Reciprocal Rank Fusion)

RRF는 순위 기반으로 각 검색 결과 문서들을 재 평가합니다. 이 과정에서 최종적으로 어떤 문서가 중요한지 판단한다.

$$
score = \sum{\frac{1}{k + rank(d)}}
$$
k는 상수(평균 60), rank(d)는 문서d가 검색 결과에서 차지한 순위이다.

![[Pasted image 20241004203933.png]]


rank(d)가 역수이기 때문에, 최고 순위(작을수록) score는 커지며, 그 반대는 score가 작아진다.


>[!info] Info : 순위를 매기는 이유
>Score로 하는 경우, 검색 결과 점수 스케일 기준이 모두 다를 수 있기 떄문에 문서의 관련성 순서 기준으로 계산한다.

### 어디서 사용할까

실제로 RRF 알고리즘을 구현해서 여러 쿼리나 문서 결과에 적용할 필요가 없다. Lanchain의 EnsembleRetriever가 RRF 알고리즘을 활용해서 문서의 중요도를 결정하기 때문이다.

EnsembleRetriever는 여러 Retriever를 결합하고, Retriever간에 weight(중요도) + rank(d)를 바탕으로 RRF Fusion을 이용해 문서의 중요도를 결정한다.

![[Pasted image 20241004204334.png]]

## 질문 & 확장

(없음)

## 출처(링크)

- https://velog.io/@euisuk-chung/RAG-From-Scratch-5-9#part-6-rag-fusion
- 
## 연결 노트










