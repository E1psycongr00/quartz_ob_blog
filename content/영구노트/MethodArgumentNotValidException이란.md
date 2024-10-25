---
tags:
  - 완성
  - JAVA
  - Error
aliases: 
title: MethodArgumentNotValidException이란
date: 2023-11-10
---
작성 날짜: 2023-11-10
작성 시간: 20:13


----
## 내용(Content)

> Exception to be thrown when validation on an argument annotated with @Valid fails. Extends BindException as of 5.3.

- 기본적으로 Spring에서는 MethodArgumentNotValidException은 400으로 응답되도록 처리하고 있다.
- MethodArgumentNotValidException은 BindingException을 상속받는다. 그 말은 BindingResult 정보를 호출할 수 있다는 뜻이다.
- BindingException을 활용해서 좀 더 구체적으로 예외를 처리할 수 있다.
## 질문 & 확장

- BindingException과 BindingResult에 대해서 공부해야 할 것 같은 느낌이 든다. 이것이 유효성 객체를 담고 있는 핵심이기 때문이다.

## 출처(링크)
- https://velog.io/@imcool2551/Spring-%EA%B2%80%EC%A6%9D1-BindingResult-MessageCodesResolver#1-bindingresult-fielderror-objecterror

## 연결 노트










