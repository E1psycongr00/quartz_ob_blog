---
tags:
  - 완성
  - ast
  - SyntaxTree
aliases: 
title: unified & remark & rehype 동작 과정
date: 2024-02-28
---
작성 날짜: 2024-02-28
작성 시간: 16:32


----
## 내용(Content)
기본적으로 해당 라이브러리는 다음과 같이 동작한다.

![[ast를 활용해 새로운 문법 생성 과정(draw).svg]]

input은 text이다. 이를 Parser는 넣으면 Parser는 구문을 분석하고 Abstract Syntax Tree를 생성한다. 이를 다시 Compiler를 통해 우리가 활용할 수 있는 결과로 리턴한다.

조금 더 구체적인 동작 방식은 다음과 같다.

![[unified 동작 과정(draw).svg]]

이 때 컴파일러는 remark-stringify나 rehype-stringify가 된다.  이렇게 중간에 transformer 통해 ast를 조작함으로써 html 렌더링에 필요한 새로운 문법을 적용할 수 있게 된다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://github.com/remarkjs
- https://github.com/rehypejs/rehype

## 연결 노트
- [[AST]]
-
- [[markdown에 커스텀 문법 적용하기]]









