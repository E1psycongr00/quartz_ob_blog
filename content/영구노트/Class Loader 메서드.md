---
tags:
  - 완성
  - JAVA
aliases: 
title: Class Loader 메서드
date: 2023-10-10
---
작성 날짜: 2023-10-10
작성 시간: 15:37


----
## 내용(Content)
java.lang.ClassLoader의 메서드를 알아보자

### loadClass(String name, boolean resolve)
JVM에서 참조하는 클래스를 로드할 때 사용

### defineClass()
바이트 배열을 클래스의 인스턴스로 정의하는데 사용
### findClass(String name)
JVM이 참조하는 클래스가 이전에 로드되었는지 여부를 확인하는데 사용
### Class.forName(String name, boolean initialize, ClassLoader loader)
클래스를 로드하고 초기화하는데 사용 ClassLoader가 null인 경우 BootStrap ClassLoader가 사용됨
## 질문 & 확장

예시가 있었으면 좋을 것 같기도 한데

## 출처(링크)


## 연결 노트










