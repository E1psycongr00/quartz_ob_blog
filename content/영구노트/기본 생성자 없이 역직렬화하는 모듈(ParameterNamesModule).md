---
tags:
  - 완성
  - IT
  - JAVA
  - Jackson
aliases: 
title: 기본 생성자 없이 역직렬화하는 모듈(ParameterNamesModule)
date: 2023-11-06
---
작성 날짜: 2023-11-06
작성 시간: 18:08


----
## 내용(Content)

### 문제점

```java
public class User {  
  
    private final long userId;  
    private final String username;  
    private final int postCount;  
  
    public User(long userId, String username, int postCount) {  
       this.userId = userId;  
       this.username = username;  
       this.postCount = postCount;  
    }  
  
    public long getUserId() {  
       return userId;  
    }  
  
    public String getUsername() {  
       return username;  
    }  
  
    public int getPostCount() {  
       return postCount;  
    }  
}
```
다음과 같은 dto가 있다고 가정하자. dto는 내부 데이터가 바뀔 수도 있지만 경우에 따라서는  바뀌지 않는 것이 더 안전할 수 있다. dto 또는 VO 또한 마찬가지이다. 그러나 이를 ObjectMapper에서 그대로 호출하면 다음과 같은 에러가 발생한다.

![[Pasted image 20231106224347.png]]

### 기본 생성자로 해결하기

private을 활용해 기본 생성자를 지정해준다. 그러면 문제를 해결할 수 있다.

```java
public class User {  
  
    private long userId;  
    private String username;
    private int postCount;  
  
    private User() {}
      
    public User(long userId, String username, int postCount) {  
       this.userId = userId;  
       this.username = username;  
       this.postCount = postCount;  
    }  
  
    public long getUserId() {  
       return userId;  
    }  
  
    public String getUsername() {  
       return username;  
    }  
  
    public int getPostCount() {  
       return postCount;  
    }  
}
```

**단점**

- 기본 생성자가 private이여서 접근할 수 없겠지만 리플렉션 공격에 취약하다
- Java를 엄격하게 사용하는 사람들은 라이브러리 때문에 굳이 변하지 않는 속성을 final로 지정 못하는 것에 대해 불만이 많을 것이다. 자바를 이용한 프로그래밍 특징 중 하나는 엄격하게 프로그래밍을 해서 개발자의 실수를 줄이는 것을 기준으로 삼기 때문이다. 

### 어노테이션으로 해결하기
```java
public class User {  
  
    private final long userId;  
    private final String username;  
    private final int postCount;  
  
    @JsonCreator  
    public User(@JsonProperty("user_id") long userId,  
       @JsonProperty("username") String username,  
       @JsonProperty("post_count") int postCount) {  
       this.userId = userId;  
       this.username = username;  
       this.postCount = postCount;  
    }  
  
    public long getUserId() {  
       return userId;  
    }  
  
    public String getUsername() {  
       return username;  
    }  
  
    public int getPostCount() {  
       return postCount;  
    }  
}
```

**단점**

- Jackson 어노테이션에 너무 많이 의존해서 불필요하게 코드가 길어진다.
	- 매번 dto나 vo를 만들 때 이렇게 작성하는 것은 비효율적이다.

### ParameterNamesModule 사용하기

#### 의존성 추가하기

기본 jackson 라이브러리는 ParameterNamesModule을 제공하지 않는다. 다음과 같이 의존성을 추가한다.

```groovy
// jackson
implementation 'com.fasterxml.jackson.core:jackson-databind:2.12.7.1'  

// parameter-names-module
implementation "com.fasterxml.jackson.module:jackson-module-parameter-names:2.16.0-rc1"
```

#### gradle 컴파일 옵션 설정하기

해당 모듈은 리플렉션 기반으로 동작하는데 생성자와 파라미터 정보를 얻어오기 위해서는 compiler에 -parameters 옵션을 추가시켜야 한다.

인텔리제이를 사용하는 경우에는 build & run 모두 gradle로 바꾸고 다음과 같은 내용을 추가한다.

```groovy
compileJava {  
    options.compilerArgs << '-parameters'  
}  
  
compileTestJava {  
    options.compilerArgs << '-parameters'  
}
```


>[!tip]
> `<<` 의미:
>  **options.compilerArgs라는 리스트에 '-parameters' 인자를 추가시키라는 의미이다.**

SpringBoot의 경우에는 해당 인자가 자동으로 설정되어 있기 때문에 따로 설정할 필요가 없다고 한다.


#### ObjectMapper에 모듈 등록하기

```java
private static final ObjectMapper SNAKE_MAPPER = new ObjectMapper()  
    .registerModule(new ParameterNamesModule())  
    .setPropertyNamingStrategy(PropertyNamingStrategies.SNAKE_CASE);
```

registerModule을 이용해 ParameterNamesModule()을 등록해준다.

### 유의할 점
이 경우 네이밍만 맞으면 알아서 자동으로 인식해서 만들어주는 편리한 장점이 있지만 생성자 파라미터가 하나인 경우에는 오류가 발생한다. 

**생성자는 항상 @JsonCreator를 붙여주는 것이 좋다.**

```java
public class Sum {  
  
    private final long sum;  
  
    @JsonCreator  
    public Sum(long sum) {  
       this.sum = sum;  
    }  
  
    public long getSum() {  
       return sum;  
    }  
  
}
```
## 질문 & 확장

(없음)

## 출처(링크)

- https://dundung.tistory.com/202
- https://beaniejoy.tistory.com/76
- https://mvnrepository.com/artifact/com.fasterxml.jackson.module/jackson-module-parameter-names/2.16.0-rc1
- https://velog.io/@hgo641/Spring%EC%97%90%EC%84%9C%EC%9D%98-%EC%A7%81%EB%A0%AC%ED%99%94JSON-parse-error-%ED%95%84%EB%93%9C%EA%B0%80-%ED%95%98%EB%82%98%EB%B0%96%EC%97%90-%EC%97%86%EB%8A%94-DTO#%EA%B8%B0%EB%B3%B8%EC%83%9D%EC%84%B1%EC%9E%90%EA%B0%80-%EB%AC%B4%EC%A1%B0%EA%B1%B4-%ED%95%84%EC%9A%94%ED%95%98%EB%8B%A4%EA%B3%A0
## 연결 노트










