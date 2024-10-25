---
tags:
  - 완성
  - Langchain
  - LLM
aliases: 
date: 2024-09-24
title: Langchain이란
---
작성 날짜: 2024-09-24
작성 시간: 14:37


----
## 내용(Content)

### Langchain이란

>[!summary]
> Langchain이란 LLM을 기반으로 다양한 어플리케이션을 구축하고 관리하는 오픈소스 도구이다.

Langchain을 이용하면 단순 LLM에게 RAG와 결합하여 언어 성능을 높일 수 있고, 프로젝트 목적에 따라 외부 데이터를 쉽게 인식하거나, 타 시스템과 쉽게 상호 작용할 수 있도록 개발된 프레임워크라 할 수 있다.

Langchain의 주요 특징은 다음과 같다.

1. 모듈화: LangChain은 데이터 전처리, 모델 호출, 후처리 등 다양한 구성 요소를 독립적인 모듈로 관리한다.
2. 체인: 여러 작업을 일련의 단계로 연결하여 하나의 파이프라인을 구성합니다. LCEL로 인해 쉽게 chain을 구성할 수 있다.
3. 통합: 다양한 데이터 소스와 외부 API를 통합하여 모델의 성능과 유연성을 높인다.
4. 에이전트: 특정 작업을 자동화하거나 사용자가 정의한 목표를 달성하기 위해 필요한 여러 단계를 관리
5. 메모리: 대화형 어플리케이션에서 컨텍스트를 유지하고, 이전 상호작용을 바탕으로 더 나은 응답을 제공한다.

### Langchain 구조

![[Langchain 구조(draw).svg]]

langchain을 이용해 RAG LLM을 구성한 모습이다.

사용자가 질문을 하면 질문한 내용을 Embedding 한 후 의미 분석에 들어간다. vector store에는 pdf가 청크단위로 분리되고 분리된 텍스트들은 임베딩되어서 vector store에 보관된다. 이들 중 가장 질문과 연관성이 높은 result를 뽑아내고, LLM이 result를 바탕으로 적절하게 판단해서 답변하게 된다.

>[!tip]
>pdf 외에 Web, 단순 text, 이미지 등등 여러 입력 데이터들이 임베딩되어 vector store에 보관될 수 있다.

## 질문 & 확장

(없음)

## 출처(링크)

- https://aws.amazon.com/ko/what-is/langchain/
- https://yongeekd01.tistory.com/92
- about:blank
- https://rudaks.tistory.com/entry/%EB%9E%AD%EC%B2%B4%EC%9D%B8LangChain%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80
- https://www.langchain.com/
## 연결 노트










