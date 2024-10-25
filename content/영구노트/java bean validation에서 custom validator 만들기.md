---
tags:
  - 완성
  - JAVA
  - Validation
aliases: 
title: java bean validation에서 custom validator 만들기
date: 2023-11-10
---
작성 날짜: 2023-11-10
작성 시간: 13:39


----
## 내용(Content)

### 기존의 어노테이션 문제점

기존의 어노테이션만으로는 충분하지 않을 수 있을 때가 있다. 바로 어노테이션을 많이 사용하거나 또는 Pattern이 복잡하고 자주 중복해서 사용하는 경우에는 커스텀 어노테이션을 만드는게 적절할 수 있다.



### 커스텀 Validator 만들기

#### 어노테이션 정의하기

```java
@Documented  
@Constraint(validatedBy = {PhoneConstraintValidator.class})  
@Target({ElementType.METHOD, ElementType.FIELD})  
@Retention(RetentionPolicy.RUNTIME)  
public @interface Phone {  
    String message() default "Invalid phone number";  
    Class<?>[] groups() default {};  
    Class<? extends Payload>[] payload() default {};  
}
```

위에 3개 속성은 필수적으로 지정해줘야한다.

- message -> MessageTemplage
- groups -> interface group
- payload -> 심각도

메시지 이외에는 잘 안쓰임. 보통 구현된 부분 보면 message는 MessageTemplate 경로를 많이 지정해둠

#### Validator 작성하기

```java
public class PhoneConstraintValidator implements ConstraintValidator<Phone, String> {  
  
    private static final Pattern PHONE_PATTERN = Pattern.compile("^010-(\\d{4})-(\\d{4})$");  
  
    @Override  
    public void initialize(Phone constraintAnnotation) {  
       // 어노테이션 정보를 가져와서 초기화 가능  
    }  
  
    @Override  
    public boolean isValid(String value, ConstraintValidatorContext context) {  
       return PHONE_PATTERN.matcher(value).matches();  
    }  
}
```

어노테이션을 통해 필요한 속성 정보를 가져오고 싶다면 initialize를 구현한다.

핵심 valid 코드는 isValid 메서드이다.

#### 테스트

```java
@Test  
void test() {  
    Validator validator = Validation.buildDefaultValidatorFactory().getValidator();  
    Man man = new Man("1234");  
    validator.validate(man).forEach(System.out::println);  
}
```


```text
 ConstraintViolationImpl{interpolatedMessage='Invalid phone number', propertyPath=phoneNumber, rootBeanClass=class backjoon.Man, messageTemplate='Invalid phone number'}
```


## 질문 & 확장


## 출처(링크)
- https://kapentaz.github.io/java/Java-Bean-Validation-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90/#
- https://docs.spring.io/spring-framework/reference/core/validation/beanvalidation.html


## 연결 노트
- [[어노테이션 @Target]]









