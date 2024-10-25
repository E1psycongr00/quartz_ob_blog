---
tags:
  - 완성
  - JAVA
aliases: 
title: java.nio.file.Path 사용하기
date: 2023-11-07
---
작성 날짜: 2023-11-07
작성 시간: 14:17


----
## 내용(Content)

### Path 란
파일 시스템에서 파일을 찾는 데 사용할 수 있는 개체다. 이는 일반적으로 시스템 종속 파일 경로를 나타낸다.

Files 클래스와 함께 사용한다.

**Path 특징**
- Path는 계층적이고 특수 구분 기호로 구분된 일련의 디렉터리 및 파일 이름 요소로 구성된 경로를 나낼 수 있다.
- 파일 시스템 계층 구조를 식별하는 루트 구성 요소 도 존재할 수 있다.
- Path 루트, 루트 및 이름 시퀀스 또는 단순히 하나 이상의 이름 요소를 나타낼 수 있습니다.
- Path가 하나의 비어 있는 이름 요소로만 구성된 경우 Path 빈 경로로 간주된다.
	- 빈 경로를 사용하여 파일에 액세스하는 것은 파일 시스템의 기본 디렉터리에 액세스하는 것과 같다.

### Path 생성하기

```java
String currentPath = "/home/project/src";
Path path = Paths.get(currentPath);
```


### 경로 관련 정보 가져오기

#### getRoot()

```java
Path path = Paths.get("/home/project/file.txt");
path.getRoot() // "/home"
```

이 경우에는 home을 Path로 가진다.

#### getParent()

```java
Path path = Paths.get("/home/project/file.txt");
path.getParent() // "/home/project"
```
바로 이전 경로를 리턴한다. 


#### getFileName()

```java
Path path = Paths.get("/home/project/file.txt");
path.getFileName(); // file.txt
```

Root 와 가장 먼 경로를 출력한다. 최종 디렉토리 이름 또는 파일 이름이 된다.


#### getNameCount()

```java
Path path = Paths.get("/home/project/file.txt"); // 요소는 3개
for (int i = 0; i < path.getNameCount(); i++) {
	System.out.println(path.getName(i));
}
// home
// project
// file.txt

```

구분자로 구분된 각 요소들의 갯수를 반환한다. getName(index) 메서드와 함께 사용해서 각각의 요소를 Path로 반환이 가능하다.

#### subpath()
```java
Path path = Paths.get("/home/project/file.txt");
path.subpath(1, 3); // "project/file.txt"
```

일부 Path를 분리해서 상대 경로를 추출할 수 있다.

**유의점**
endIndex는 항상 +1로 열린 공간으로 생가해준다.
위의 예제의 경우 0: home, 1: project, 2: file.txt이지만 위의 결과를 위해선 2 + 1을 해준다.


### Path 변환하기

#### toUri()

```java
String currentPath = "/home/project/hello.txt";  
Path path = Paths.get(currentPath);  
path.toUri();
// file:///C:/home/project/hello.txt
```

Path를 웹 브라우저에서 사용하는 형식의 문자열로 변환할 수 있다.

#### toAbsolutePath()

```java
String currentPath = "/home/project/hello.txt";  
Path path = Paths.get(currentPath);  
path.toAbsolutePath();
// C:\home\project\hello.txt
```

절대 경로로 변환한다.

Root에 /를 붙이지 않은 경우에는 현재 작업 디렉토리를 기준으로 절대 경로를 반환한다

```java
String currentPath = "home/project/hello.txt";  
Path path = Paths.get(currentPath);  
path.toAbsolutePath();
// C:\java_project\algorithm\home\project\hello.txt
```

#### toFile()

```java
String currentPath = "home/project/hello.txt";  
Path path = Paths.get(currentPath);  
path.toFile();
```

파일 타입으로 변환한다. 파일의 경우에도 역으로 Path로 변환이 가능하다.


### Path 조합하기

#### resolve(other path)

```java
Path base = Paths.get("src/backjoon");  
Path path1 = base.resolve("hello.txt");
// src/backjoon/hello.txt
```

다른 path 또는 String 타입을 인자로 받을수 있다. 

이 메서드는 두 개의 Path를 합치는 역할을 수행한다.


#### resolveSibling(other path)

```java
Path base = Paths.get("src/backjoon/hello.txt");  
Path path1 = base.resolveSibling("hello2.txt");
// src\backjoon\hello2.txt
```

resolve와 비슷하나 차이점은 부모 디렉토리 기준으로 추가된다는 점이다. 이 의미는 현재 최종 경로에 해당 경로로 바꿔치기한다는 것이다. 그래서 Sibling이라는 이름이 메서드에 붙은듯 하다


### 경로 비교하기

#### equals()

단순이 Path들의 경로가 같은지 비교할 때 쓰인다.

#### startsWith(), endsWith()

Path의 접두사 혹은 접미사가 같은지 테스트한다.


### Iterator 지원

for-each문을 지원한다.

```java
Path base = Paths.get("src/backjoon/hello.txt");  
for (Path path : base) {  
    System.out.println(path);  
}
// src
// backjoon
// hello.txt
```



## 질문 & 확장

(없음)

## 출처(링크)
-  https://m.blog.naver.com/horajjan/220484659082\
-  https://www.baeldung.com/java-path-vs-file

## 연결 노트
- [[java.nio.file.Files 사용하기]]








