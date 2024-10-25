---
tags:
  - 완성
  - Spring
  - Validation
  - Error
aliases: 
title: MethodArgumentNotValidException
date: 2023-12-07
---
작성 날짜: 2023-12-07
작성 시간: 14:25


----
## 내용(Content)
### 예외 발생 시점
>Exception to be thrown when validation on an argument annotated with @Valid fails. Extends BindException as of 5.3.

MethodArgumentNotValidException은 Controller에서 @Valid 가 붙은 dto 클래스의 유효성 검사 결과가 옳바르지 못한 경우 예외를 발생한다.

BindException을 확장한 클래스이기 때문에 BindingResult를 가지고 있으며, MessageSource와 관련된 간편한 메서드를 제공한다.


### 예외로부터 얻을 수 있는 것들

BindingResult 타입과 정보를 얻을 수 있고, ControllerAdvice에서 다음과 같이 예외 처리를 할 수 있다.

```java
@ExceptionHandler(MethodArgumentNotValidException.class)  
public ResponseEntity<ErrorResponse> handleMethodArgumentNotValidException(MethodArgumentNotValidException e,  
    BindingResult bindingResult) {
    // ...
}
```


BindingResult는 DefaultMessageSourceResolvable를 상속한 ObjectError와 FieldError를 얻을 수 있는데 이를 통해 MessageSource와 함께 쉽게 커스텀 Validation Message를 작성할 수 있다.
## 질문 & 확장

(없음)

## 출처(링크)
- javadoc

## 연결 노트










