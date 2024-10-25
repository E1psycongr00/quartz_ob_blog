---
tags:
  - 완성
  - JAVA
  - Validation
aliases: 
title: getter에 java bean validation 적용 및 테스트
date: 2023-11-10
---
작성 날짜: 2023-11-10
작성 시간: 11:38


----
## 내용(Content)

### getter에 적용하기

#### getter에 validation 어노테이션 적용

```java
public class Question {  
  
    private final boolean solved;  
  
    public Question(boolean solved) {  
       this.solved = solved;  
    }  
  
    @AssertTrue  
    public boolean isSolved() {  
       return solved;  
    }  
}
```

getter 또는 setter에 적용 시 장점은 세부적으로 조정 가능하다는 특징이 있다. 
#### 테스트 코드

```java
@Test  
void test() {  
  
    ValidatorFactory validatorFactory = Validation.buildDefaultValidatorFactory();  
    Validator validator = validatorFactory.getValidator();  
  
    Question question = new Question(false);  
  
    validator.validate(question).forEach(System.out::println);  
}
```


## 질문 & 확장

(없음)

## 출처(링크)
- https://kapentaz.github.io/java/Java-Bean-Validation-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90/#
- https://docs.spring.io/spring-framework/reference/core/validation/beanvalidation.html

## 연결 노트










