---
tags:
  - 완성
  - JAVA
  - IT
aliases: 
title: JVM 구조
date: 2023-10-08
---

작성 날짜: 2023-10-08
작성 시간: 18:36


----
## 내용(Content)
JVM은 자바 가상 머신으로 자바 어플리케이션을 실행하는 가상 머신이다. 실제 컴퓨터로 부터 JAVA application 실행을 위한 메모리를 할당 받아 Runtime Data Area를 구성한다.

- JVM은 인터프리터와 JIT 컴파일러를 통해 바이트 코드를 각 운영체제에 맞는 기계어로 해석해서 실행하고 가비지 컬렉터를 활용해 어플리케이션의 동적 메모리를 관리한다.
- [JVM Specification](https://docs.oracle.com/javase/specs/jvms/se11/jvms11.pdf) 와 [JVM Guide](https://docs.oracle.com/en/java/javase/11/vm/java-virtual-machine-technology-overview.html#JSJVM-GUID-982B244A-9B01-479A-8651-CB6475019281) 참고하자

### JVM 구조
JVM 명세에서는 반드시 지켜야 할 구조만을 명시하고 내부적인 구현은 구현하려는 개발자 재량으로 남겨두었다. 알아야 할 핵심 구조를 짚어보자

### Runtime Data Area
![[Runtime Data Area 구성(draw).svg|700]]


#### 라이프 사이클

**Heap & Method**
- JVM 시작시 생성되며 JVM 종료시 모두 소멸한다.
- 모든 Thread에 정보를 공유한다.

**PC Register & Native Method Stack**
JVM이 동작하기 시작하면 Heap 영역과 Method 영역이 생성되며 해당 영역은 모드 Thread들과 공유한다.
각 쓰레드는 생성 마다 PC Register, Native Method Stack이 생성되며 스레드가 종료되면 사라진다.

#### PC Register
- 자바 가상 머신이 현재 실행 중인 명령어 주소를 저장

> **PC Register**
>컴퓨터가 다음에 실행할 명령어가 어디에 있는지 가리키는 포인터 역할
>명령어의 메모리 주소를 저장하고, CPU의 다음 명령어를 찾아서 실행할 때 사용한다.
>프로그램 흐름을 제어하고, 명령어를 순차적으로 실행하는데 필요한 구성 요소 

#### Stack
- Frame이라는 자료 구조를 저장함
- C와 같이 전통적인 언어의 Stack과 역할이 비슷하다.(지역변수, 함수의 실행 결과를 저장)

#### Frame
- 함수가 호출될 때 생성되고 함수가 종료되면 사라진다.
- 데이터 및 반환 데이터를 저장하는 자료구조
- 각 프레임은 지역변수 배열, Operand Stack, Runtime Constant Pool 에 대한 참조 값을 가진다.

#### Native Method Stack
- Java 이외의 다른 언어로 작성된 코드를 실행할 때 사용하는 스택이다.

#### Heap
- Heap 영역은 클래스의 인스턴스들과 배열이 저장되는 공간이다.
- Heap 영역은 가비지 컬렉션과 동적 메모리 관리 시스템에 의해 관리된다.


#### Method
- Runtime Constant Pool , field, function, code 등 클래스와 인터페이스 구조가 저장되는 공간이다

#### Runtime Constant Pool
 
- Class Constant Pool에서 읽어온 상수값과 클래스 메타 데이터 저장
- 클래스에 포함되어 있던 값이 런타임시에 저장됨
- Constant Pool에 관련된 정보는 class 파일에 있지만 이를 로드하면서 runtime constant pool에 저장하게 됨
-  Runtime Constant Pool을 통해 해당 메소드나 필드의 실제 메모리 상 주소를 찾아 참조한다.

## 질문 & 확장

- Runtime Constant Pool 은 뭔가 이해가 잘 안되는 느낌이야. 비교해서 정리할 필요가 있겠어
## 출처(링크)
- https://velog.io/@sgwon1996/JAVA%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC%EC%99%80-JVM-%EA%B5%AC%EC%A1%B0
- https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-2/
- https://docs.oracle.com/javase/specs/jvms/se11/jvms11.pdf
- https://deveric.tistory.com/123
## 연결 노트
- [[영구노트/String Constant Pool]]
- [[Class Loader]]









