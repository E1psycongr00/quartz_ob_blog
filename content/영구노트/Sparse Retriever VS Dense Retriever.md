---
tags:
  - 완성
  - Python
  - Langchain
  - RAG
  - Retriever
aliases: 
date: 2024-09-27
title: Sparse Retriever VS Dense Retriever
---
작성 날짜: 2024-09-27
작성 시간: 09:28


----
## 내용(Content)

### Sparse Retriever

>[!summary]
>문서와 쿼리(질문)를 이산적인 **키워드 벡터로 변환하여 처리**하고 전통적인 정보 검색 기법을 사용한다.

**장점:**

- 효율적인 메모리 사용
- 빠른 검색 속도
- 높은 확장성
- 질문과 같은 단어만 선택

**단점:**

- 낮은 정확도 
- 의미론적 정보 손실


### Dense Retriever

>[!summary]
>Dense Retriever는 최신 딥러닝 기법을 사용하여 문서와 쿼리를 연속적인 고차원 벡터로 인코딩해서 사용한다.

BERT와 같은 신경망 모델을 이용해서 텍스트 데이터를 임베딩한다.

**장점:**

- 높은 정확도
- 의미론적 정보 유지 (단어가 다른표현으로 진행해도 가능하다)

**단점:**

- 높은 메모리 사용
- 느린 검색 속도
- 낮은 확장성

### Sparse vs Dense

|         | Sparse Retriever | Dense Retriever |
| ------- | ---------------- | :-------------: |
| 정확도     | 낮음               |       높음        |
| 의미론적 정보 | 손실               |       유지        |
| 메모리 사용  | 효율적              |       높음        |
| 검색 속도   | 빠름               |       느림        |
| 확장성     | 높음               |       낮음        |

## 질문 & 확장

(없음)

## 출처(링크)

- https://wikidocs.net/233779

## 연결 노트










