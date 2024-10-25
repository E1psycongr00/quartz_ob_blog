---
tags:
  - Python
  - Langchain
  - Retriever
  - LLM
aliases: 
date: 2024-10-04
title: Query Translation
---
작성 날짜: 2024-10-04
작성 시간: 15:56

#미완 #Python #Langchain #Retriever #LLM 

----
## 내용(Content)

![[Pasted image 20241004160109.png]]

### Query Translation

>[!summary]
> 질문을 다양한 관점에서 재작성하여 보다 효과적인 문서 검색(query)을 하는 방법

사용자의 질문이 모호하거나 제대로 구조화되지 않은 경우, 문서에서 keyword 또는 유사 검색과정에서 원치 않는 문서를 얻을 가능성이 높아진다. 그로 인해 LLM이 할루시네이션, 또는 엉뚱한 대답을 할 가능성이 높아진다. Query Translation은 질문을 다양한 관점에서 분석하고 재작성해서 쿼리 성능을 높이는데 중점을 둔 방법이라 할 수 있다.

### Query Translation 기법

대표적으로 3가지 기법을 통해 각각 질문들을 추가하거나 변경해서 문서 검색 성능을 올릴 수 있다.

1. Query Rewriting(쿼리 재작성)
	- 질문을 다양한 관점에서 분석 및 작성을 해서 검색 성능을 올리는 방법이다. 
	- ex) 다중 쿼리 (Multi Query)
2. Sub Question(하위 질문 생성)
	- 복잡하고 추상적인 질문을 더 작고 구체적인 세부 하위 질문으로 분해하는 방법이다. 이를 통해 좀 더 정확한 문서 검색이 가능하다.
	- ex) cursor의 composer, google의 least to most 기법
3. Abstract Query(추상적인 질문 생성)
	- 질문을 높은 수준으로 추상화 및 검색하는 방법
	- Sub Question과 반대로 세부 질문 포함한 더 광범위한 문서를 가져와서 문맥 파악 능력을 향상시키는 방법이다.(Stepback prompting)
	- ex) ParentDocumentRetriever

### Query Translation 방법 5가지

- [[다중 쿼리 기법 | Multi Query]]
- [[RAG Fusion]]
- 분해
- Stepback Prompting(단계적 후퇴)
- HyDE

## 질문 & 확장

(없음)

## 출처(링크)

- [[다중 쿼리 기법]]
- https://velog.io/@euisuk-chung/RAG-From-Scratch-5-9#part-5-%EB%8B%A4%EC%A4%91-%EC%BF%BC%EB%A6%AC
- ![[RAG from scratch_ Overview.pdf]]

## 연결 노트










