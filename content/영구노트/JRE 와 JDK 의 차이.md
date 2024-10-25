---
tags:
  - 완성
  - IT
  - JAVA
aliases: 
title: JRE 와 JDK 의 차이
date: 2023-10-03
---
작성 날짜: 2023-10-03
작성 시간: 17:08


----
## 내용(Content)

### JRE

![[Pasted image 20231003191214.png]]

JRE는 Java Runtime Environment의 약자로 자바로 만들어진 프로그램을 실행시키는데 필요한 라이브러리와 API, JVM이 포함되어 있다.

JRE는 개발은 안되며 읽기 전용으로 쓰인다.

### JDK

![[Pasted image 20231003191242.png]]

JDK는 Java Development Kit의 약자로 개발자들이 Java를 개발하는데 사용된다.  JDK 안에는 Java를 실행시키기 위한 JRE가 내장되어 있고, 그 이외에 javac, javadoc등의 개발 도구들이 포함되어 있다.

### JDK, JRE 구분이 크게 의미가 없다

java 11 버전 이후로는 oracle에서는 jdk 하나만 제공한다. 아마 내 생각에는 초보 개발자들이나 *" java를 처음 사용하는 사람은 JRE를 다운로드하고 왜 개발이 안되지?"* 하며, 혼선을 주는 것을 최대한 방지하려고 하는 것 같다.

**java 8부터 10 download (JRE, JDK)**
- https://www.oracle.com/kr/java/technologies/javase/javase8-archive-downloads.html
- https://www.oracle.com/kr/java/technologies/javase/javase9-archive-downloads.html
- https://www.oracle.com/kr/java/technologies/java-archive-javase10-downloads.html

**java 11 이후 (JDK)**
- https://www.oracle.com/kr/java/technologies/javase/jdk11-archive-downloads.html
- https://www.oracle.com/kr/java/technologies/javase/jdk12-archive-downloads.html
- https://www.oracle.com/kr/java/technologies/javase/jdk14-archive-downloads.html
- https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html

## 질문 & 확장

(없음)

## 출처(링크)
- https://www.marcobehler.com/guides/a-guide-to-java-versions-and-features
- https://coding-factory.tistory.com/826
- https://www.javatpoint.com/difference-between-jdk-jre-and-jvm
## 연결 노트










