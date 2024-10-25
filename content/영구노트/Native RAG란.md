---
tags:
  - 완성
  - Python
  - Langchain
  - RAG
aliases:
  - Native RAG
date: 2024-10-04
title: Native RAG란
---
작성 날짜: 2024-10-04
작성 시간: 15:37


----
## 내용(Content)

### Native RAG

>[!summary]
>가장 기본적인 형태의 RAG이다. 질문 -> 검색 -> 프롬프트 -> LLM -> 응답 과정을 거친다.


### Native 동작 과정

![[Native RAG (draw).svg]]

Native RAG는 아주 기본적인 동작 과정을 거친다. 질문에 대해서 저장해놓은 DB에서 자료를 검색하고 자료(Document)를 받아서 Prompt를 업데이트한다. Prompt에는 자료들에 대한 설명과 LLM에게 어떤 역할을 줄 지 선택하도록 해주며, 이를 통해 조금 더 정확한 대답을 할 수있다.

Retriever는 크게 4가지 단계로 나누며, 단계는 다음과 같이 설명할 수 있다.

1. Load: 외부 데이터를 인 메모리에 로드하는 과정이다.
2. Split: 커다란 데이터를 조각내서 VectorStore에 저장하기 좋게 Chunk를 낸다.
3. Embed: Chunk된 데이터를 임베딩한다.
4. Store: VectorDB와 같은 저장소에 임베딩 데이터를 저장한다.

## 질문 & 확장

(없음)

## 출처(링크)

- [[RAG란|RAG]]
- ![[RAG from scratch_ Overview.pdf]]
## 연결 노트










