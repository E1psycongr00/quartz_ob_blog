---
tags:
  - 완성
  - JAVA
aliases:
  - Java에서 외부 스크립트 사용하기
  - zt-exec
date: 2024-05-16
title: Java에서 외부 프로그램 사용하기
---
작성 날짜: 2024-05-16
작성 시간: 16:18


----
## 내용(Content)

### zt-exec

>[!summary]
>Java의 ProcessBuilder 기능보다 훨씬 더 직관적으로 쉽고 편하게 동작 가능하고, 기능도 더 풍부하다. 해당 라이브러리를 사용하면 보다 쉽게 외부 프로그램들을 실행할 수 있다.

zt-exec 라이브러리는 **java.lang.ProcessBuilder** and [Apache Commons Exec](http://commons.apache.org/proper/commons-exec/).  2개를 보완하기 위해 만들어진 라이브러리이다. 쉽고 직관적이기 때문에 잘 이해가 가능하고, ProcessExecutor라는 클래스를 이용해 쉽게 외부 프로그램을 실행 가능하다.

### 의존성 등록하기

Gradle + Kotlin DSL 기준으로 설명하겠다.

```kotlin
// zt-exec  
implementation("org.zeroturnaround:zt-exec:1.12")  
  
// SLF4J binding  
implementation("ch.qos.logback:logback-classic:1.2.9")
```

만약 SLF4j가 의존성에 이미 등록되어 있다면 zt-exec:1.12만 등록해주면 된다.

Github에선 다음과 같이 의존성이 필요하다고 설명하고 있다.

- Minimal required Java version is 6 (tested also with 8, 11 and 17).
- The only 3rd-party dependency is [SLF4J](https://www.slf4j.org/).

### 사용 예시

#### JS Script 실행하기

Java에서 JS를 실행하기 이전에 JS Script를 Terminal에서 실행하는 과정을 먼저 살펴보자.
우선 terminal을 킨 뒤 node ㅇㅇ.js 라고 치면 node 엔진에서 js를 실행한다. 이 과정을 JAVA에서 해주면 된다.

```java
Future<ProcessResult> helloJs = new ProcessExecutor()  
    .command("node", "src/scripts/js/hello.js")  
    .redirectOutput(Slf4jStream.ofCaller().asInfo())  
    .readOutput(true)  
    .start()  
    .getFuture();

helloJs.get().outputUTF8();
```

해당 코드는 비동기 코드로 동기 코드를 짤 수도 있다.

```java
String helloJs = new ProcessExecutor()  
    .command("node", "src/scripts/js/hello.js")  
    .redirectOutput(Slf4jStream.ofCaller().asInfo())  
    .readOutput(true)  
    .execute() 
    .outputUTF8();
```


#### python 스크립트 실행하기

python 스크립트의 경우 miniconda로 가상환경을 구성하고 환경변수를 등록해두었다. 여기서 문제는 conda로 가상 환경을 관리하는 경우 터미널에서 실행시 `activate [환경 변수]`를 선행해주어야 한다. 어떻게 하면 python 스크립트를 실행 가능한지 코드 예시를 보자

```java
Future<ProcessResult> helloPython = new ProcessExecutor()  
    .command("conda.bat", "activate", "pythonProject", "&&", "python", "src/scripts/python/hello.py")  
    .redirectOutput(Slf4jStream.ofCaller().asInfo())  
    .readOutput(true)  
    .start()  
    .getFuture();
```

`&&`으로 명령어를 연결해서 command에 넣어주면 된다.
## 질문 & 확장

(없음)

## 출처(링크)

- https://ch4njun.tistory.com/214
- https://github.com/zeroturnaround/zt-exec
## 연결 노트



