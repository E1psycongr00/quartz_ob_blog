---
tags:
  - 완성
  - JAVA
  - GC
aliases: 
title: Mark And Sweep
date: 2023-10-16
---
작성 날짜: 2023-10-16
작성 시간: 14:27


----
## 내용(Content)

> Mark
> 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업


> Sweep
> 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업

Stop The World를 통해 모든 작업을 중단시키면, GC는 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 각각이 어떤 객체를 참고하고 있는지를 탐색하게 된다. 그리고 사용되고 있는 메모리를 식별하는데, 이러한 과정을 Mark라고 한다. 이후에 Mark가 되지 않은 객체들을 메모리에서 제거하는데, 이러한 과정을 Sweep라고 한다.

## 질문 & 확장

(없음)

## 출처(링크)
- https://mangkyu.tistory.com/118
- https://d2.naver.com/helloworld/329631

## 연결 노트










