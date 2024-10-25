---
tags:
  - 완성
  - JAVA
  - Spring
  - Validation
aliases: 
title: Spring bean validation 상황 별 예외 분석
date: 2023-11-10
---
작성 날짜: 2023-11-10
작성 시간: 18:36


----
## 내용(Content)

### @RequestBody에서 Validation 에러가 발생한 경우

#### 원인

@Valid와 @RequestBody에서 Validation이 발생한 경우 400 코드 응답이 뜬다. 그리고 [[MethodArgumentNotValidException이란|MethodArgumentNotValidException]] 이 발생한다. 

기본적으로 validation 예외는 ConstraintViolationException 예외가 발생하지만 Controller에서 @RequestBody를 쓰는 경우에는 **RequestResponseBodyMethodProcessor**가 @Valid와 함께 처리하면서 [[MethodArgumentNotValidException이란|MethodArgumentNotValidException]]을 발생시킨다.


#### 커스텀 예외 처리하기

BindingException을 상속하기 때문에 여기서 BindingResult을 뽑아낼 수 있다. 이 말은 BindingResult 를 통해 예외 상세한 정보를 가져오고 가공할 수 있다는 의미가 된다.

```java
@ExceptionHandler(MethodArgumentNotValidException.class)  
public ResponseEntity<ErrorResponse> handleMethodArgumentNotValidException(MethodArgumentNotValidException e) {  
    BindingResult bindingResult = e.getBindingResult();  
    List<String> errorFieldMessages = new ArrayList<>();  
  
    for (FieldError fieldError : bindingResult.getFieldErrors()) {  
       String errorField = fieldError.getField();  
       String message = fieldError.getDefaultMessage();  
       String rejectedValue = fieldError.getRejectedValue().toString();  
       String errorMessage = String.format("필드 이름: %s, 오류 메시지: %s, 입력된 값: %s", errorField, message, rejectedValue);  
       errorFieldMessages.add(errorMessage);  
    }  
  
    log.error(errorFieldMessages.toString());  
    return ResponseEntity.badRequest().body(new ErrorResponse(400, errorFieldMessages.toString()));  
}
```

다음 코드는 MethodArgumentNotValidException을 핸들링하는 코드이다. bindingResult로 부터 FieldError들을 가져오고 상세정보를 가져와서 가공한 코드임을 볼 수 있다.




### @Validated 클래스에서 발생한 예외

#### 원인

클래스 컴포넌트(Bean)에 @Validated를 붙이면 해당 컴포넌트는 Validation 해야할 Bean으로 인식하고 유효성 검사를 실행한다. 예외 발생시 **ConstraintViolationException**이 발생한다.

Spring에서는 따로 처리하고 있지 않으므로 Internal Error Exception(내부 오류)이 발생한다.

#### 커스텀 예외 처리하기

```java
@ExceptionHandler(ConstraintViolationException.class)  
public ResponseEntity<ErrorResponse> handleConstraintViolationException(ConstraintViolationException e) {  
    List<String> errorFieldMessages = new ArrayList<>();  
  
    e.getConstraintViolations().forEach(violation -> {  
       String errorFieldPath = violation.getPropertyPath().toString();  
       String errorField = errorFieldPath.substring(errorFieldPath.lastIndexOf(".") + 1);  
       String message = violation.getMessage();  
       String errorMessage = String.format("필드 이름: %s, 오류 메시지: %s", errorField, message);  
       errorFieldMessages.add(errorMessage);  
    });  
  
    log.error(errorFieldMessages.toString());  
    return ResponseEntity.badRequest().body(new ErrorResponse(400, errorFieldMessages.toString()));  
}
```

getViolations를 활용해 적절한 내용들을 가져오면 된다


## 질문 & 확장

- 이상하게 MockMvc 테스트가 영어로 출력되네.. 어떻게 해결해야 되지?
## 출처(링크)
- https://kapentaz.github.io/spring/Spring-Boo-Bean-Validation-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90/#
- https://docs.spring.io/spring-framework/reference/core/validation/beanvalidation.html
- https://velog.io/@imcool2551/Spring-%EA%B2%80%EC%A6%9D1-BindingResult-MessageCodesResolver#1-bindingresult-fielderror-objecterror
## 연결 노트
- [[MethodArgumentNotValidException이란]]
- [[MockMvc 영어 출력을 한글 출력으로 바꾸기]]









