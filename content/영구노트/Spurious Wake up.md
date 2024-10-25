---
tags:
  - 완성
  - OS
  - Synchronization
aliases: 
title: Spurious Wake up
date: 2024-01-20
---
작성 날짜: 2024-01-20
작성 시간: 17:13


----
## 내용(Content)
### 정의
[spurious wake up 위키](https://en.wikipedia.org/wiki/Spurious_wakeup)를 참고

멀티 쓰레딩 도중 조건 충족하지도 않았는데 conditional variable를 기다리다 꺠어날 때 아무 이유 없이 깨는 문제가 발생한다. 이는 JVM이나 OS 특성 상 발생할 수 있는 문제이다.

그렇기에 Spurious wake up에 의한 오작동 문제를 방지하기 위해서 한번이 아닌 지속적으로 조건이 충족하는지 검사해야 한다.

## 질문 & 확장

(없음)

## 출처(링크)
- https://en.wikipedia.org/wiki/Spurious_wakeup

## 연결 노트










