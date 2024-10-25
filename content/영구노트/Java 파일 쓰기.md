---
tags: 
  - 완성
  - JAVA
aliases:
title: Java 파일 쓰기
date: 2023-11-07
---
작성 날짜: 2023-11-07
작성 시간: 13:08


----
## 내용(Content)

File에 쓰기 위해서 2가지 타입을 사용한다.


| 클래스          | 공통점    | 차이점               | 용도                      |
| --------------- | --------- | -------------------- | ------------------------- |
| FileWriter      | 파일 쓰기 | 인코딩 처리가 가능   | 문자 파일                 |
| FileOutputStream | 파일 쓰기 | 바이너리 형태로 씀 | 이미지 같은 바이너리 파일 |

### FileWriter 사용하기

#### BuffedWriter

```java
public class Main {  
    private static final String SYSTEM_PATH = System.getProperty("user.dir");  
    private static final String DIRECTORY = "src/backjoon";  
  
    private static final String CURRENT_DIRECTORY = SYSTEM_PATH + File.separator + DIRECTORY;  
  
    public static void main(String[] args) throws IOException {  
       String currentPath = CURRENT_DIRECTORY + File.separator + "hello.txt";  
       try (BufferedWriter buffer = new BufferedWriter(new FileWriter(currentPath, StandardCharsets.UTF_8, true))) {  
          buffer.append("Hello asaaad!!!!");  
       }  
    }  
}
```

**FileWriter를 사용해서 BuffedWriter를 사용하는 경우 write, append가 제대로 동작하지 않는다.** FileWriter를 기반으로 동작하는데 FileWriter 생성시 append이냐 append가 아니냐에 따라 다르게 동작하기 때문이다. 

if FileWriter의 append ==  true : 추가로 붙임 else 새로 만듬

#### PrintWriter
```java
import java.io.File;  
import java.io.FileWriter;  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.nio.charset.StandardCharsets;  
  
public class Main {  
    private static final String SYSTEM_PATH = System.getProperty("user.dir");  
    private static final String DIRECTORY = "src/backjoon";  
  
    private static final String CURRENT_DIRECTORY = SYSTEM_PATH + File.separator + DIRECTORY;  
  
    public static void main(String[] args) throws IOException {  
       String currentPath = CURRENT_DIRECTORY + File.separator + "hello.txt";  
       try (PrintWriter printer = new PrintWriter(new FileWriter(currentPath, StandardCharsets.UTF_8, true))) {  
          printer.println("hello world!!");  
          printer.println(String.format("hello world %d", 50));  
       }  
    }  
}
```

BuffedWriter와 달리 println, printf 와 같은 print에 친숙한 메서드를 제공해서 쓰는 것을 더 쉽게 만들어준다. 공통점은 FileWriter를 기반으로 동작하기 때문에 append 동작이 FileWriter의 생성 인자에 기반에 동작한다.

### FileOutputStream 사용하기

```java
import java.io.BufferedOutputStream;  
import java.io.File;  
import java.io.FileOutputStream;  
import java.io.IOException;  
import java.nio.charset.StandardCharsets;  
  
public class Main {  
    private static final String SYSTEM_PATH = System.getProperty("user.dir");  
    private static final String DIRECTORY = "src/backjoon";  
  
    private static final String CURRENT_DIRECTORY = SYSTEM_PATH + File.separator + DIRECTORY;  
  
    public static void main(String[] args) throws IOException {  
       String currentPath = CURRENT_DIRECTORY + File.separator + "hello.txt";  
       try (BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(currentPath, true))) {  
          bos.write("Hello World!".getBytes(StandardCharsets.UTF_8));  
       }  
    }  
}
```

바이너리를 입력할 때 쓰이며 필요한 경우 인코딩해서 문자열을 넣을 수도 있다. 위의 코드는 UTF-8로 인코딩해서 집어넣는 코드이다.

bos.write() 동작시에 byte 데이터를 넣어주어야 한다.


## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트

- [[Java 파일 읽기]]
- [[java.io.File 파일 생성 및 가져오기]]








