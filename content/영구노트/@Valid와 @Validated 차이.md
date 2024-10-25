---
tags:
  - 완성
  - JAVA
  - Spring
  - Validation
aliases: 
title: "@Valid와 @Validated 차이"
date: 2023-11-12
---
D작성 날짜: 2023-11-12
작성 시간: 13:21

----
## 내용(Content)
### @Valid
valid는 객체의 내부에 정의된 어노테이션의 유효성을 검증할 떄 사용한다.

예를 들어 다음과 같은 객체를 만들었다고 가정하자

```java
public class Person {
	@NotBlank
	private final String name;
	
	@Positive
	private final int age;
	// ....
}
```

다른 곳에서는 위에서 정의해둔 Person을 사용하고 이 객체 안의 내용을 검증하고 싶을 것이다. 이럴 떄 @Valid를 사용한다.

```java
class Store {
	private final List<@NotNull @Valid Person> people;
}
```

Store는 Person을 List 타입으로 가지는 people 속성을 가지고 있다고 정의하자. 이럴 때 Person 내부적으로는 bean validation이 적용되어 있을지 몰라도 Store 클래스 단계에서는 Person이 bean validation이 적용되어 있는지 알 수 없다. 그래서 @Valid 어노테이션을 달아서 Person 객체의 유효성 검사를 하도록 마킹해준다.

@Valid를 활용하면 중첩 클래스에 대해서도 테스트를 수행할 수 있다.

주로 컨트롤러에서 dto를 만들고 이를 검증할 때 많이 붙이는 어노테이션이다.

주요 용도는 여기를 참고 [JSR303](https://beanvalidation.org/1.0/spec/#constraintdeclarationvalidationprocess)


### @Validated

Spring Bean Validation에서 추가된 어노테이션이다. 두 가지 기능을 제공한다.

- @Valid는 그룹별 Validation이 불가능하지만 Spring Bean Validation은 가능하다.
- 클래스에 @Validated를 붙이면 해당 컴포넌트(빈)은 유효성 검사를 진행하고 예외 발생시 ConstraintViolationException을 발생한다.



## 질문 & 확장

(없음)

## 출처(링크)
- https://beanvalidation.org/1.0/spec/#constraintdeclarationvalidationprocess
- https://beanvalidation.org/1.0/spec/#d0e991
## 연결 노트










