---
tags:
  - 완성
  - JAVA
  - Validation
aliases: 
title: field에 java bean validation 적용 및 테스트
date: 2023-11-10
---
작성 날짜: 2023-11-10
작성 시간: 09:49


----
## 내용(Content)

클래스 필드에 Validation 어노테이션을 적용해서 유효성 검사를 진행할 수 있다.

일반 코드와 테스트 코드를 통해 비교하면서 어떻게 사용하는지 살펴보자

### 필드를 통해 유효성 정의하기

#### 어노테이션을 활용해 Validation 정의하기

```java
public class Product {  
  
    @Positive  
    private final int productNo;  
  
    @Size  
    private final String name;  
  
    @Range(min = 1000, max = 1000000)  
    private final int price;  
  
    @DecimalMin("0.0")  
    @DecimalMax("100.0")  
    private final BigDecimal discount;  
  
    @PastOrPresent  
    private final LocalDateTime salStartAt;  
  
    @Future  
    private final LocalDateTime salEndAt;  
  
    public Product(int productNo, String name, int price, BigDecimal discount, LocalDateTime salStartAt,  
       LocalDateTime salEndAt) {  
       this.productNo = productNo;  
       this.name = name;  
       this.price = price;  
       this.discount = discount;  
       this.salStartAt = salStartAt;  
       this.salEndAt = salEndAt;  
    }  
}
```

#### field validation 테스트

```java
class ProductTest {  
  
    @Test  
    void test() {  
       Product product = new Product(-1, "화장품", 13000,  
          BigDecimal.valueOf(10.0),  
          LocalDateTime.now().plusDays(5),  
          LocalDateTime.now().plusDays(10));  
  
       // Validator 생성  
       ValidatorFactory validatorFactory = Validation.buildDefaultValidatorFactory();  
       Validator validator = validatorFactory.getValidator();  
  
       // Validation 출력  
       Set<ConstraintViolation<Product>> validate = validator.validate(product);  
       validate.forEach(System.out::println);  
    }  
  
}
```


Bean Validation은 Validator 객체를 활용해서 인스턴스의 내부 속성을 외부에서 유효성 검사를 진행한다.

이 때 ValidatorFactory 를 호출하고 팩토리에서 Validator를 가져온다.

그 이후 valdate 메서드를 통해 ConstraintViolation 을 가져온다. 테스트를 실행하면 다음과 같은 결과를 얻을 수 있다.

```text
 ConstraintViolationImpl{interpolatedMessage='과거 또는 현재의 날짜여야 합니다', propertyPath=salStartAt, rootBeanClass=class backjoon.Product, messageTemplate='{javax.validation.constraints.PastOrPresent.message}'}
 ConstraintViolationImpl{interpolatedMessage='0보다 커야 합니다', propertyPath=productNo, rootBeanClass=class backjoon.Product, messageTemplate='{javax.validation.constraints.Positive.message}'}
```



### Nested Field 유효성 정의하기

#### Nested 클래스 Validation 정의하기

코드를 짜다 보면 Nest 클래스의 유효성을 체크해야 할 때가 있다. 다음의 예제를 살펴보자

```java
public class Person {  
  
    @NotBlank  
    private final String name;  
  
    @Range(min = 1, max = 100)  
    private final int age;  
  
    @NotNull  
    @Valid    
    private final Address address;  
  
    public Person(String name, int age, Address address) {  
       this.name = name;  
       this.age = age;  
       this.address = address;  
    }  
  
    public static class Address {  
  
       @NotBlank  
       private final String city;  
  
       @Range(min = 1, max = 1000)  
       private final int rodeNumber;  
  
       public Address(String city, int rodeNumber) {  
          this.city = city;  
          this.rodeNumber = rodeNumber;  
       }  
    }  
}
```


코드를 보면 Address라는 객체가 있고 address 객체는 내부적으로 NotBlank과 Range validation이 존재한다. 이를 그냥 사용하면 Person에 적용된 어노테이션은 검사를 진행하지만 Address 타입은 진행하지 않는다. 이를 수행하기 위해서는 **@Valid** 어노테이션을 필드에 붙여주면 된다. 그러면 Vallidator는 해당 타입을 인식하고 생성된 속성의 타입 인스턴스의 유효성을 검사한다.

>**@Valid**
>Valid는 타입 내부에 Jakarta Bean Validation 어노테이션으로 정의된 속성을 명시적으로 유효성 검사를 하라고 하는 것이다.  이를 적용하지 않으면 타입 내부에 어떤 Validation 어노테이션을 적용하더라고 검사를 하지 않는다.


#### Nested Field를 Validation하는 테스트

```java
@Test  
void test() {  
    Person person = new Person("홍길동", -2, new Person.Address("서울", -1));  
  
    // Validator 생성  
    ValidatorFactory validatorFactory = Validation.buildDefaultValidatorFactory();  
    Validator validator = validatorFactory.getValidator();  
  
    // Validation 출력  
    Set<ConstraintViolation<Person>> validate = validator.validate(person);  
    validate.forEach(System.out::println);  
}
```

결과는 다음과 같다.

```text
 ConstraintViolationImpl{interpolatedMessage='1에서 100 사이여야 합니다', propertyPath=age, rootBeanClass=class backjoon.Person, messageTemplate='{org.hibernate.validator.constraints.Range.message}'}
 ConstraintViolationImpl{interpolatedMessage='1에서 1000 사이여야 합니다', propertyPath=address.rodeNumber, rootBeanClass=class backjoon.Person, messageTemplate='{org.hibernate.validator.constraints.Range.message}'}
```




## 질문 & 확장

(없음)

## 출처(링크)
- https://kapentaz.github.io/java/Java-Bean-Validation-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90/#
- https://docs.spring.io/spring-framework/reference/core/validation/beanvalidation.html

## 연결 노트










