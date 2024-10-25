---
tags:
  - 완성
  - JAVA
aliases: 
title: java.io.File 파일 생성 및 가져오기
date: 2023-11-07
---
작성 날짜: 2023-11-07
작성 시간: 12:03


----
## 내용(Content)

**java.io.File은 String 타입의 path와 File 인스턴스를 생성**해서 동작한다. 이를 이용해서 Diretory와 File 생성이 가능하다.

### 디렉토리  & 파일 생성하기

디렉토리를 생성하는데는 mkdir() 메서드와 mkdirs() 메서드 2개가 존재한다.

| 메서드   | 용도                                                 |
| -------- | ---------------------------------------------------- |
| mkdir()  | 단일 디렉토리 생성(상위 디렉토리가 없을 경우 생성 X) |
| mkdirs() | 재귀적으로 디렉토리 생성(상위 디렉토리가 없는 경우 직접 생성) |


```java
import java.io.File;  
import java.io.IOException;  
  
public class Main {  
  
    public static void main(String[] args) throws IOException {  
       String path = System.getProperty("user.dir");  
       makeDirectory(path + "/src/backjoon");  
       makeFile(path + "/src/backjoon/hello.txt");  
    }  
  
    private static boolean makeDirectory(String directory) {  
       File file = new File(directory);  
       return file.mkdirs();  
    }  
  
    private static boolean makeFile(String path) {  
       File file = new File(path);  
       try {  
          return file.createNewFile();  
       } catch (IOException e) {  
          e.printStackTrace();  
       }  
       return false;  
    }  
}
```

### 하위 디렉토리 파일 가져오기

하위 디렉토리의 모든 파일을 읽을 때는 listFiles()라는 메서드를 사용한다.

**listFiles()는 File이 디렉토리가 아닌경우 null을 리턴한다.**

```java
public class Main {  
    private static final String SYSTEM_PATH = System.getProperty("user.dir");  
    private static final String DIRECTORY = "src/backjoon";  
  
    public static void main(String[] args) throws IOException {  
       String currentPath = SYSTEM_PATH + File.separator + DIRECTORY;  
       File dir = new File(currentPath);  
       File[] files = dir.listFiles();  
       System.out.println(Arrays.toString(files));  
    }  
  
}
```


#### 재귀를 이용해서 모든 하위 디렉토리 파일 가져오기

depth가 1차가 아닌 n차인 경우에 재귀적으로 탐색하는 방법을 알아보자.
재귀적으로 코드를 짜주기만 하면 된다.

```java
private static void recursiveSearchFile(File dir, List<File> temp) {  
	if (dir.isFile()) {  
		temp.add(dir);  
		return; 
	}  
    for (File file : Objects.requireNonNull(dir.listFiles())) {  
		if (file.isDirectory()) {  
			recursiveSearchFile(file, temp);  
		} else {  
			temp.add(file);  
		}  
    }  
}
```


## 질문 & 확장

(없음)

## 출처(링크)
- https://codingdog.tistory.com/entry/java-file-%EA%B0%9D%EC%B2%B4%EB%A1%9C-%ED%95%98%EC%9C%84-%ED%8F%B4%EB%8D%94-%ED%8C%8C%EC%9D%BC%EB%93%A4%EC%9D%84-%EB%AA%A8%EB%91%90-%EC%B0%BE%EC%95%84-%EB%B4%85%EC%8B%9C%EB%8B%A4
- https://sgpassion.tistory.com/331

## 연결 노트










