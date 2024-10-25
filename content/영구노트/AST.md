---
tags:
  - 완성
  - CS
  - SyntaxTree
aliases:
  - Abstract Syntax Tree
title: AST
date: 2024-02-23
---
작성 날짜: 2024-02-23
작성 시간: 11:54


----
## 내용(Content)
### Abstract Syntax Tree
>[!summary] Abstract Syntax Tree
>컴퓨터 과학에서 프로그램이나 코드 조각의 구조를 나타나는데 사용되는 데이터 구조

AST는 이름 그대로 트리 구조로 이루어져 있다. 어떻게 보면 계산기에 사용되는 expression tree도 ast라 할 수 있다.

AST의 특징은 문장을 어떤 특징을 이용해 쪼개서 Tree 형태로 관리 할 수 있다는 점이다.
이를 잘 활용하는 것이 Compiler이다. 

>[!info]- 간단한 컴파일러 동작 과정
>![[Pasted image 20240226234041.png]]
>컴파일러는 어휘 분석 프로세스를 활용해 토큰이라는 작은 덩어리로 분리한다. 그 다음 구문 분석기에 의해 AST 구조를 만든다.


### AST 활용
AST는 컴파일러 뿐만 아니라 Java 파서, ANTLR, UNIST, Linter 등 다양한 응용 프로그램에서 활용되어 문장을 토큰화하고 구문 트리를 생성하는 데 사용된다.




## 질문 & 확장

(없음)

## 출처(링크)
- https://ko.wikipedia.org/wiki/%EC%B6%94%EC%83%81_%EA%B5%AC%EB%AC%B8_%ED%8A%B8%EB%A6%AC
- https://dev.to/balapriya/abstract-syntax-tree-ast-explained-in-plain-english-1h38

## 연결 노트
- [[unist]]








