---
tags:
  - 완성
  - JAVA
aliases: 
title: Constant Pool vs Runtime Constant Pool
date: 2023-10-13
---
작성 날짜: 2023-10-13
작성 시간: 16:29


----
## 내용(Content)
Runetime Constant Pool 과 Constant Pool이 헷갈리기 쉬워서 정리했다.

### Constant Pool
Constant pool은 클래스 파일 내부에 존재하는 영역이다. 클래스 로더에 의해 로드 될 때 메모리에 로드한다. 주로 클래스 경로 및 메타 데이터를 저장하고 있다.

예를 들어 다음과 같은 class 코드가 있다고 가정하자
```java
class Hello {
	public static void main(String[] args) {
		for (int i = 0; i < 10; i++) {
			System.out.println("Hello from Hello.main!");
		}
	}
}
```

해당 클래스를 컴파일한 후, constant pool을 열어보면 다음과 같이 써있다.

```text
#1 = Methodref #6.#16 // java/lang/Object."<init>":()V
#2 = Fieldref #17.#18 // java/lang/System.out:Ljava/io/PrintStream;
#3 = String #19 // Hello from Hello.main! 
#4 = Methodref #20.#21 // java/io/PrintStream.println:(Ljava/lang/String;)V 
#5 = Class #22 // Hello
#6 = Class #23 // java/lang/Object 
#7 = Utf8 <init> 
#8 = Utf8 ()V 
#9 = Utf8 Code
#10 = Utf8 LineNumberTable
#11 = Utf8 main
#12 = Utf8 ([Ljava/lang/String;)V 
#13 = Utf8 StackMapTable 
#14 = Utf8 SourceFile 
#15 = Utf8 Hello.java 
#16 = NameAndType #7:#8 // "<init>":()V 
#17 = Class #24 // java/lang/System 
#18 = NameAndType #25:#26 // out:Ljava/io/PrintStream;
#19 = Utf8 Hello from Hello.main! 
#20 = Class #27 // java/io/PrintStream 
#21 = NameAndType #28:#29 // println:(Ljava/lang/String;)V
#22 = Utf8 Hello
#23 = Utf8 java/lang/Object 
#24 = Utf8 java/lang/System 
#25 = Utf8 out 
#26 = Utf8 Ljava/io/PrintStream; 
#27 = Utf8 java/io/PrintStream
#28 = Utf8 println 
#29 = Utf8 (Ljava/lang/String;)V
```


### Runtime Constant Pool

Runtime Constant Pool은 Method Area 내부에 존재하는 영역이다. 앞에서 설명한 Constant Pool(클래스 정보)를 ClassLoader로부터 불러 들여 해당 영역에 저장한다.

## 질문 & 확장

(없음)

## 출처(링크)
- https://deveric.tistory.com/123

## 연결 노트










