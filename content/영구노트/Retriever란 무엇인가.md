---
tags:
  - 완성
  - Python
  - Langchain
  - RAG
  - Retriever
aliases:
  - Retriever
  - 정보 검색기
date: 2024-09-27
title: Retriever란 무엇인가
---
작성 날짜: 2024-09-27
작성 시간: 08:59


----
## 내용(Content)

### Retriever

>[!summary]
> Retriever(검색기)는 저장된 벡터 데이터베이스에서 사용자의 질문과 관련된 문서를 검색하도록 도와주는 인터페이스이다. 

### Retriever의 중요성

Retriever의 목표는 사용자 요구에 맞추어 가장 정확한 정보를 신속하게 찾아내는 것이 목적이다. Retriever는 RAG에서 정보 검색과 질을 결정짓는 핵심적인 역할을 수행한다.

질 좋은 Retriever를 사용하면 다음과 같은 이점을 얻을 수 있다.

1. **정확한 정보 제공**
	- 사용자 질문과 가장 관련 높은 정보를 제공 가능
2. **응답 시간 단축**
	- 효율적인 알고리즘으로 정보를 빠르게 검색해서 활용
3. **최적화**
	- 필요한 정보만 추출

### 동작 과정

![[Retriever 동작과정(draw).svg]]

1. **질문의 벡터화**: 사용자의 질문을 벡터 형태로 변환한다.
    
2. **벡터 유사성 비교**: 저장된 문서 벡터들과 질문 벡터 사이의 **유사성을 계산**한다. 이는 주로 코사인 유사성(cosine similarity), Max Marginal Relevance(MMR) 등의 수학적 방법을 사용하여 수행된다.
    
3. **상위 문서 선정**: 계산된 유사성 점수를 기준으로 **상위 N개의 가장 관련성 높은 문서를 선정**한다. 이 문서들은 다음 단계에서 사용자의 질문에 대한 답변을 생성하는 데 사용된다.
    
4. **문서 정보 반환**: 선정된 문서들의 정보를 다음 단계(프롬프트 생성)로 전달한다. 이 정보에는 문서의 내용, 위치, 메타데이터 등이 포함될 수 있다.

## 질문 & 확장

Retriever 종류로는 크게 Sparse Retriever와 Dense Retriever로 나눌 수 있다.
- Sparse Retriever는 이름 그대로 질문을 이산적은 Keyword 벡터로 변환해서 처리한다.
- Dense Retriever는 최신 딥러닝 기법을 적용해서 문서와 쿼리를 고차원 벡터로 임베딩한다. 

## 출처(링크)

- https://www.youtube.com/watch?v=J2AsmUODBak&t=1796s
- https://mobicon.tistory.com/616
- https://wikidocs.net/233779
## 연결 노트

- [[RAG란|RAG]]
- [[Sparse Retriever VS Dense Retriever]]