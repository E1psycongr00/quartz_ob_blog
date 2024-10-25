---
tags:
  - 완성
  - JAVA
  - Spring
  - Validation
aliases: 
title: Spring bean validation과 MessageCodesResolver
date: 2023-11-10
---
작성 날짜: 2023-11-10
작성 시간: 18:37


----
## 내용(Content)

### MessageCodesResolver가 필요한 이유

매번 Validation Message 를 작성하는것은 굉장히 피곤한 일이고, 코드를 길게 만든다. 그러나 범용성이 높은 Validation Message를 작성하는 것은 코드를 짧게 만들고, 유지 보수하기 좋게 만들지만, 때로는 세부적으로 에러 메시지를 전달해야 할 때가 있다.

MessageCodesResolver는 단계적으로 코드를 생성하며, 세부적인 내용에서 범용적인 내용 순으로 우선 순위를 가지고 관리한다. 이 때문에 코드를 이용하여 평소에는 범용적으로 사용하다가 필요할 때 세부적인 예외를 작성할 수 있게 된다.

![[MessageCodesResolver code 우선순위(draw).svg]]

### MessageCodesResolver 구성
Spring에서는 오류 메시지를 관리하기 위해서 `MessageCodesResolver` 를 사용한다. 기본 구현체는 DefaultMessageCodesResolver이다.

```java
public interface MessageCodesResolver {  

	String[] resolveMessageCodes(String errorCode, String objectName);  
  
	String[] resolveMessageCodes(String errorCode, String objectName, String field, @Nullable Class<?> fieldType);   
}
```



### 동작 원리 파악하기

```java
@Getter  
@RequiredArgsConstructor  
public class ItemRequest {  
  
    @Positive  
    private final Long serialNumber;  
  
    @NotBlank  
    @Length(max = 10)  
    private final String name;  
  
}
```

다음과 같은 ItemRequest가 있다고 가정하자. 우리는 여기서 코드를 추출해볼 것이다.

```java
public class MessageCodesResolverTest {  
  
    private final MessageCodesResolver resolver = new DefaultMessageCodesResolver();  
  
    @Test  
    void objectTest() {  
       String[] codes = resolver.resolveMessageCodes("NotBlank", "itemRequest", "name", String.class);  
       for (String code : codes) {  
          System.out.println(code);  
       }  
    }  
}
```

테스트를 간단히 설명하자면 errorCode가 NotEmpty(**어노테이션 이름**)인 ItemRequest 객체에서 필드 이름이 name이고 타입이 String인 것을 단계별 메시지 코드를 생성해주세요란 의미가 된다.

그러면 다음과 같은 메시지가 생성된다.

```text
NotBlank.itemRequest.name
NotBlank.name
NotBlank.java.lang.String
NotBlank
```


이는 내부적으로 BindingResult 와 같은 데서 사용되는데 이것과 MessageSource를 이용하면 커스텀 메시지를 작성할 수 있다.


>[!summary]
>Bean Validation에서는 다음과 같은 규칙을 따른다.
>- errorCode -> 어노테이션 이름
>- objectName -> path
>- fieldName -> target

>[!example] 예제 1: 클래스
>ItemRequest의 name 필드에 @NotBlank가 적용된 경우
>- errorCode -> NotBlank
>- objecName -> itemRequest
>- fieldName -> name
>
>최종 생성되는 code는 NotBlank.itemRequest.name

>[!example] 예제2: 메서드 파라미터
> WordFinder 클래스의 findWord 메서드 파라미터 word라는 것에 @NotBlank가 적용된 경우
>- errorCode -> NotBlank
>- objectName -> wordFinder
>- fieldName -> findWord.word
>
>최종 생성되는 code는 NotBlank.wordFinder.findWord.word








## 질문 & 확장

- MessageSource를 활용해 Custom 메시지를 작성하는 방법을 알아봐야겠군

## 출처(링크)
- https://docs.spring.io/spring-framework/reference/core/validation/conversion.html
- https://kapentaz.github.io/spring/Spring-Boo-Bean-Validation-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90/#
- https://catsbi.oopy.io/f6bc86a1-d19d-4647-bd12-b2d1d7db1b4b
## 연결 노트










