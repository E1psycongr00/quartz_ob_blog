---
tags:
  - 완성
  - 솔루션
  - JAVA
  - VSCode
aliases: 
date: 2024-10-06
title: vscode java 패키지 import 제대로 안되는 경우 해결방법
---
작성 날짜: 2024-10-06
작성 시간: 13:37


----

## 문제 & 원인

List나 Objects등 자바의 기본적인 package import 자동 완성이 지원되지 않는다. 뭔가 `java.util.*`을 사용하면 import가 알아서 예쁘게 바뀌긴 하지만, 제대로 동작은 하지 않는다

## 해결 방안

### vscode workspace cache 문제

부분적으로 동작하고, 어떤 부분은 제대로 동작하지 않았을 때, vscode workspace cache에 쌓인 정보들 때문에 충돌하면서 문제가 발생할 수 있다. 이럴 땐 cache을 깨끗하게 지우면 된다.

![[Pasted image 20241006134229.png]]


## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
