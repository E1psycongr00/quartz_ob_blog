---
tags:
  - 완성
  - JAVA
  - Validation
aliases: 
title: parameter에 java bean validation 적용 및 테스트
date: 2023-11-10
---
작성 날짜: 2023-11-10
작성 시간: 11:47


----
## 내용(Content)

### 파라미터에 Validation 적용하기

#### 파라미터 validaiton 코드

```java
public class Calculator {  
  
    @Min(100000)  
    public int calculate(@Min(1_000) int price,  
       @Max(3000) int discountPrice) {  
       return price - discountPrice;  
    }  
}
```

파라미터에도 validation을 적용할 수 있다. 이 경우에는 해당 인자를 메서드 실행 이전에 적합한 인자가 입력됬는데 Validation을 수행한다


#### 테스트 코드

```java
@Test  
void parameterTest() throws NoSuchMethodException {  
    // ExecutableValidator 생성  
    ValidatorFactory factory = Validation.buildDefaultValidatorFactory();  
    ExecutableValidator validator = factory.getValidator().forExecutables();  
  
    // Calculator 객체 생성  
    Calculator calculator = new Calculator();  
    Method method = calculator.getClass().getMethod("calculate", int.class, int.class);  
  
    Set<ConstraintViolation<Calculator>> violations = validator.validateParameters(  
       new Calculator(), method, new Object[] {9000, 4001}  
    );  
  
    // validation 결과 출력  
    violations.forEach(System.out::println);  
}
```

필드나 getter를 검사할 때와는 다르게 ExecutableValidator를 이용해 테스트를 한다.

이 때 메서드 정보는 리플렉션을 통해 가져오며 실제 실행하지는 않고 reflection 정보만을 활용해 해당 인자가 적절한 인자인지 validation을 수행한다.



## 질문 & 확장

(없음)

## 출처(링크)

- https://kapentaz.github.io/java/Java-Bean-Validation-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90/#
- https://docs.spring.io/spring-framework/reference/core/validation/beanvalidation.html

## 연결 노트










