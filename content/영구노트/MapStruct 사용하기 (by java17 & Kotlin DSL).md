---
tags:
  - 완성
  - JAVA
aliases: 
title: MapStruct 사용하기 (by java17 & Kotlin DSL)
date: 2023-10-26
---

작성 날짜: 2023-10-26
작성 시간: 22:29


----
## 내용(Content)

MapStruct 1.5.5 Final을 기준으로 사용 방법을 알아보자


### MapStruct를 사용하는 이유

비즈니스 로직을 짜다 보면 특정 data class가 필요한 경우가 생길 수 있다. 예를 들면 MVC 패턴으로 코드를 짤 때 컨트롤러에서는 DTO를 사용한다. 단순 데이터를 전달하는 목적으로 사용하므로, 만약 domain 영역에서 사용하려면 dto가 아닌 엔티티도 변환해야 할 일이 생긴다. dto는 보통 프로젝션 목적으로 많이 사용하는 데 매 번 이렇게 변환하고 검증하는 것은 피곤한 일이다. 이런 문제를 Mapstruct를 활용해 해결할 수 있다.

### build.gradle
```kotlin
dependencies {
	implementation("org.mapstruct:mapstruct:1.5.5.Final")
	annotationProcessor("org.mapstruct:mapstruct-processor:1.5.5.Final")
}
```


💡 **롬복을 사용하는 경우 롬복 의존성을 mapstruct보다 위에 배치해줘야한다.**
### 가장 기본적인 Mapper 활용하기

가장 기본적인 사용법은 다음과 같다.

```java
@Builder  
public record HelloDto(@NotNull String name, @PositiveOrZero int age, @Size(min = 5, max = 10) String message) {  
}
```

```java
@Entity  
@Table(name = "hello")  
@Getter  
@AllArgsConstructor(staticName = "of")  
@NoArgsConstructor(access = AccessLevel.PRIVATE)  
public class Hello {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.UUID)  
    @Column(name = "id", nullable = false)  
    private UUID id;  
  
    @Column(name = "name", nullable = false)  
    private String name;  
  
    @Column(name = "age", nullable = false)  
    private int age;  
  
    @Column(name = "message", nullable = false)  
    private String message;  
  
    @Builder()  
    public Hello(String name, int age, String message) {  
       this.name = name;  
       this.age = age;  
       this.message = message;  
    }  
}
```

```java
@Mapper  
public interface HelloMapper {  
  
    HelloMapper INSTANCE = Mappers.getMapper(HelloMapper.class);  
  
    HelloDto helloToHelloDto(Hello hello);  
  
    Hello helloDtoToHello(HelloDto helloDto);  
}
```

id, name, age, message 속성이 있고, dto로 프로젝션할때는 id 없이 name, age, message 3개를 프로젝션하고 싶다. 이럴 때 Mapstruct를 이용하면 간단하게 mapper를 작성할 수 있다.

사용 방법은 매우 심플하다.

자동 생성된 구현체에 접근하기 위해 INSTANCE라는 변수를 만들어주고 Mappers.getMapper를 활용해 HelloMapper 클래스의 구현체를 연결해준다.

그 이후 메서드를 만들어주기만 하면 알아서 메서드를 생성한다.


### mapping 속성 이름이 일치하지 않는 경우

mapping 속성이 일치하지 않는 경우에는 직접 매핑을 해줘야한다. 이 때 @Mapping 어노테이션을 사용한다.

기존의 Hello 엔티티는 그대로 사용하고 HelloRequestDto라는 것을 만든다고 가정하자. 코드는 다음과 같다.

```java
public record HelloRequestDto(@NotNull String userName, @PositiveOrZero int userAge) {  
}
```

name이 아닌 userName, age가 아닌 userAge이다. 이런 경우에는 직접 mapping 처리를 해줘야 한다.

```java
@Mapping(target = "userName", source = "name")  
@Mapping(target = "userAge", source = "age")  
HelloRequestDto helloToHelloRequestDto(Hello hello);
```

이 코드는 hello -> helloRequsetDto로 mapping 해주는 코드이다. target은 리턴할 타입의 속성이다.
source는 변환할 속성이다. 

![[Mapping mapper(draw)|700]]
## 질문 & 확장

(없음)

## 출처(링크)
- https://mapstruct.org/documentation/installation/
- https://mapstruct.org/documentation/stable/reference/html/#basic-mappings

## 연결 노트










