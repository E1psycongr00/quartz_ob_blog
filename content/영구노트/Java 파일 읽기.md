---
tags:
  - 완성
  - JAVA
aliases: 
title: Java 파일 읽기
date: 2023-11-07
---
작성 날짜: 2023-11-07
작성 시간: 12:41


----
## 내용(Content)

java.io.File을 읽기 위해서 FileReader와 FileInputStream을 사용한다. 두 개의 공통점과 차이점은 다음과 같다.

| 클래스          | 공통점    | 차이점               | 용도                      |
| --------------- | --------- | -------------------- | ------------------------- |
| FileReader      | 파일 읽기 | 인코딩 처리가 가능   | 문자 파일                 |
| FileInputStream | 파일 읽기 | 바이너리 형태로 읽음 | 이미지 같은 바이너리 파일 |

### FileReader 사용하기

```java
import java.io.BufferedReader;  
import java.io.File;  
import java.io.FileReader;  
import java.io.IOException;  
import java.nio.charset.StandardCharsets;  
  
public class Main {  
    private static final String SYSTEM_PATH = System.getProperty("user.dir");  
    private static final String DIRECTORY = "src/backjoon";  
  
    public static void main(String[] args) throws IOException {  
       String currentPath = SYSTEM_PATH + File.separator + DIRECTORY + File.separator + "hello.txt";  
       
       try (BufferedReader bufferedReader = new BufferedReader(new FileReader(currentPath, StandardCharsets.UTF_8))) {  
          String line;  
          while ((line = bufferedReader.readLine()) != null) {  
             System.out.println(line);  
          }  
       }  
    }  
}
```

try resouce를 이용해 구현했다. 이 경우 auto close를 해준다.

BufferedReader와 FileReader를 사용했는데 인자로 **String 타입** 또는 **파일 타입** 모두 가능하다

또 Reader의 경우 텍스트 파일을 읽는데 사용하기 때문에 인코딩을 명시적으로 설정할 수 있다.
**StandardCharsets.UTF_8** 인자를 활용해 UTF-8로 명시적으로 등록했다.
### FileInputStream 사용하기
```java
import java.io.BufferedInputStream;  
import java.io.File;  
import java.io.FileInputStream;  
import java.io.IOException;  
  
public class Main {  
    private static final String SYSTEM_PATH = System.getProperty("user.dir");  
    private static final String DIRECTORY = "src/backjoon";  
  
    public static void main(String[] args) throws IOException {  
       String currentPath = SYSTEM_PATH + File.separator + DIRECTORY + File.separator + "hello.txt";  
       
       try (BufferedInputStream buffer = new BufferedInputStream(new FileInputStream(currentPath))) {  
          byte[] readBytes = buffer.readAllBytes();  
          System.out.println(new String(readBytes));  
       }  
    }  
}
```

성능을 위해 BufferedInputStream으로 감싸서 사용하고 사용 방법은 FileReader와 동일하다. String 타입, File 타입 모두 인자로 받을 수 있다. 

차이점은 byte를 가져오느냐, 아니면 인코딩된 데이터를 가져오느냐 차이가 될 것이다.
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트

- [[java.io.File 파일 생성 및 가져오기]]
- [[Java 파일 쓰기]]









