---
tags:
  - 완성
  - JAVA
aliases: 
title: java.nio.file.Files 사용하기
date: 2023-11-07
---
작성 날짜: 2023-11-07
작성 시간: 14:07


----
## 내용(Content)

Files는 파일, 디렉토리 생성부터 복제, 삭제, 쓰기, 읽기 등을 굉장히 편하게 활용할 수 있는 유틸 클래스이다. **Path 기반으로 동작**한다.

Files의 장점은 코드의 실수를 줄여주고 읽기 쓰기 등 File 관련해서 선언해야 할 긴 코드들을 줄여주기 때문에 가독성이 높은 특징이 있다.


### 파일 및 디렉토리 생성하기

#### Files.createFile(path)

```java
Path base = Paths.get(DIRECTORY);  
Files.createFile(base.resolve("hello5.txt"));
```

createNewFile은 true false 인 반면 해당 메서드는 해당 경로에 파일이 존재한다면 
**java.nio.file.FileAlreadyExistsException** 예외가 발생한다.

#### Files.createDirectory(path)

```java
Path base = Paths.get(DIRECTORY);  
Files.createDirectory(base.resolve("backjoon"));
```


디렉토리 생성시 실제 부모 디렉토리가 존재하지 않는다면 **java.nio.file.NoSuchFileException** 예외가 발생한다.

#### Files.createDirectories(path)

```java
Path base = Paths.get(DIRECTORY);  
Files.createDirectories(base.resolve("backjoon").resolve("input"));
```

중간에 디렉토리가 존재하지 않아도 계속해서 디렉토리를 생성한다.

### 속성 조회하기

#### Files.readAttributes(path, attribute)

```java
Path base = Paths.get(DIRECTORY);  
BasicFileAttributes attributes = Files.readAttributes(base.resolve("hello.txt"),  
    BasicFileAttributes.class);
```


파일 접근 시간, 생성 시간 등등에 대해서 접근할 수 있다.


### 파일 복제하기

```java
Path base = Paths.get(DIRECTORY);  
Files.copy(base.resolve("hello.txt"), base.resolve("hello2.txt"));
```

이미 target 경로에 파일이 존재한다면 **java.nio.file.FileAlreadyExistsException** 예외가 발생한다

3번째 인자로 CopyOption을 줄 수도 있다.

**CopyOption 종류**
- StandardCopyOption.REPLACE_EXISTING => 기존에 파일이 존재한다면 덮어 씌움
- StandardCopyOption.COPY_ATTRIBUTES => 기존 파일이 존재하면 예외 발생
- StandardCopyOption.ATOMIC_MOVE

디렉토리를 재귀적으로 복사하고 싶은 경우에는 SpringFramework를 활용해야 한다.
**org.springframework.util.FileSystemUtils.copyRecursively**


### 파일 내용 읽기

#### Files.readAllLines(path)

```java
Path base = Paths.get(DIRECTORY);  
List<String> list = Files.readAllLines(base.resolve("hello.txt"), StandardCharsets.UTF_8);
```

줄바꿈을 기준으로 List 형태로 글자를 가져온다.  인코딩 타입을 명시적으로 정해줄 수 있다.

존재하지 않는 경우 **java.nio.file.NoSuchFileException** 예외 발생
#### Files.readAllBytes(path)

```java
Path base = Paths.get(DIRECTORY);  
byte[] bytes = Files.readAllBytes(base.resolve("hello.txt"));
```

binary 파일을 가져올 때 쓰인다

존재하지 않는 경우 **java.nio.file.NoSuchFileException** 예외 발생


#### Files.readString(path)

```java
Path base = Paths.get(DIRECTORY);  
String s = Files.readString(base.resolve("hello.txt"));
```

readAllLines은 줄바꿈을 List로 구분해서 담아두는 반면 readString은 모든 라인을 String 타입으로 반환한다.

존재하지 않는 경우 **java.nio.file.NoSuchFileException** 예외 발생


### 파일에 쓰기

#### Files.write(path, byte, OpenOption)

바이너리 파일을 쓸 때 사용한다. OpenOption은 생략 가능하다


#### Files.write(path, Iterable 문자열, charset, OpenOption)

```java
Path base = Paths.get(DIRECTORY);  
List<String> lines = Arrays.asList("hello", "world");  
Files.write(base.resolve("hello2.txt"), lines, StandardCharsets.UTF_8, StandardOpenOption.CREATE);
```

인코딩 옵션을 설정할 수 있고 이터러블한 문자열을 입력 받는다. 분리된 문자열은 줄바꿈으로 분리된다. List 마지막 문자열에도 자동으로 "\n"이 들어간다.

그래서 글자가 길어지고 println 느낌으로 여러 문자열을 넣고 싶다면 해당 메서드를 추천한다

StandardOpenOption.APPEND 

#### Files.write(path, Iterable 문자열)

단순 문자열 String 타입을 추가할 떄 사용한다. 위와 사용방법은 거의 같다. List로 받느냐 String으로 받느냐 차이일뿐

### 디렉토리 내 파일 조회하기

#### Files.walk(path)

```java
Path base = Paths.get(DIRECTORY);  
List<Path> files;  
  
try (Stream<Path> walk = Files.walk(base)) {  
    files = walk.filter(Files::isRegularFile)  
       .toList();  
} catch (IOException e) {  
    throw new IOException("파일 목록을 가져오는데 실패했습니다.");  
}  
files.forEach(System.out::println);
```

**해당 path 디렉토리에 파일을  포맷을 재귀적으로 찾는다**. 

Stream\<Path> 형태로 제공하기 때문에
filter를 사용해서 편하게 필터링 가능하다.

depth를 제한하고 싶다면 path 인자에 depth 인자를 추가만 해주면 된다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://www.baeldung.com/java-nio-2-file-api
- https://www.baeldung.com/java-path-vs-file
- https://velog.io/@dailylifecoding/Java-nio-package-Files-usage#%F0%9F%A5%9D-%ED%8A%B9%EC%A0%95-%ED%99%95%EC%9E%A5%EC%9E%90%EC%9D%98-%ED%8C%8C%EC%9D%BC%EB%93%A4%EB%A7%8C-%EC%B0%BE%EC%95%84%EB%82%B4%EA%B8%B0
## 연결 노트

- [[java.nio.file.Path 사용하기]]








