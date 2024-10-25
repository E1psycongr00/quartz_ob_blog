---
tags:
  - 완성
  - JAVA
aliases: 
title: Java 파일 다루기
date: 2023-11-07
---
작성 날짜: 2023-11-07
작성 시간: 10:47


----
## 내용(Content)

### Java.io.File

```java
public static void main(String[] args) throws IOException {  
    String path = System.getProperty("user.dir");  
    
    File file = new File(path + "/src/backjoon.txt");  
    boolean newFile = file.createNewFile();  
    if (newFile) {  
       System.out.println("파일 생성 성공");  
    } else {  
       System.out.println("파일 생성 실패");  
    }  
}
```

[[java.io.File 파일 생성 및 가져오기]]

File 인스턴스를 생성하고 createNewFile()을 이용해 파일을 생성할 수 있다.


### java.io.FileOutputStream & FileInputStream

```java
public static void main(String[] args) throws IOException {  
    String path = System.getProperty("user.dir");  
  
    try (FileOutputStream fileOutputStream = new FileOutputStream(path + "/src/backjoon.txt", true)) {  
       String str = "Hello World!";  
       byte[] bytes = str.getBytes();  
       fileOutputStream.write(bytes);  
    } catch (FileNotFoundException e) {  
       e.printStackTrace();  
    }  
}
```

읽기 성능을 끌어 올리기 위해 BufferedOutputStream과 같이 쓰기도 한다.

FileOutputStream의 append가 true인 경우 기존 파일에 데이터를 추가하고, false인 경우에는 새롭게 생성한다.

### java.nio.file.Files
```java
public static void main(String[] args) throws IOException {  
    String path = System.getProperty("user.dir");  
    Path filePath = Paths.get(path, "input.txt");  
    Path path1 = Files.createFile(filePath);  
    System.out.println(path1);  
}
```

[[java.nio.file.Files 사용하기]]
[[java.nio.file.Path 사용하기]]

Files는 파일, 디렉토리 생성부터 복제, 삭제, 쓰기, 읽기 등을 굉장히 편하게 활용할 수 있는 유틸 클래스이다. Path 기반으로 동작한다
## 질문 & 확장

(없음)

## 출처(링크)
- https://hianna.tistory.com/588
- https://mimah.tistory.com/entry/Java-%EC%9E%90%EB%B0%94%EB%A1%9C-%ED%8F%B4%EB%8D%94%EC%99%80-%ED%8C%8C%EC%9D%BC-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0
## 연결 노트

- [[java.io.File 파일 생성 및 가져오기]]
- [[Java 파일 쓰기]]
- [[Java 파일 읽기]]
- [[java.nio.file.Path 사용하기]]
- [[java.nio.file.Files 사용하기]]







