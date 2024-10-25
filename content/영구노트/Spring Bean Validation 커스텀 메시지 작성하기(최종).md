---
tags:
  - 완성
  - 솔루션
  - JAVA
  - Spring
  - Validation
aliases: 
title: Spring Bean Validation 커스텀 메시지 작성하기(최종)
date: 2023-11-11
---
작성 날짜: 2023-11-11
작성 시간: 00:13


----

## 문제 & 원인

Spring Bean Validation을 사용하면 유효성 검사 예외 발생시 범용적으로 처리하는 것 외에 세부적인 내용에 대해서도 예외 메시지를 작성할 필요가 있다고 했다.

이 전에 이런 문제를 해결하기 위해서 [[Spring bean validation 상황 별 예외 분석]] 과 [[java bean validation에서 커스텀 메시지 만들기]] 를 사용했다.  그러나 첫번째의 경우 예외 별로 BindResult에서 정보를 가져와 단순히 문자열을 가공해서 처리했을 뿐이고, 두번 째의 경우는 JavaTemplate을 이용한 커스텀 예외처리다. Spring에서는 기본적으로 예외 메시지를 Message Source에 보관해둔다.

우리는 MethodArgumentNotValidException과 ConstraintValidationException에서 자동으로 필요한 코드를 뽑아내서 MessageSource를 이용해 기본적으로는 범용 메시지를 사용하고 세부 메시지는 MessageSource를 활용하는 프로세스를 자동화하고 싶다. 이런 경우에는 어떻게 해야 할까?

## 해결 방안

### 알아야 할 것

우선 이 문제를 해결하기 위해 이해해야 할 객체들이 있다.

- [[MethodArgumentNotValidException|MethodArgumentNotValidException이 무엇인가]]
- ObjectError, FieldError, MessageSourceResolvable
- ConstraintViolation가 무엇이고 어떤 것을 제공하는가
- MessageSource 사용법
- [[Spring bean validation과 MessageCodesResolver|MessageCodesResolver]]

이 5가지는 정확하게 이해하고 있어야 한다.

### 해결 전략

해결 전략은 2가지로 나누어서 해야하는 데 크게는 다음과 같다.

1. BindingResult 또는 ContraintViolation으로 부터 Code와 Argument 정보를 뽑아온다.
2. 예외 처리시 MessageSource 객체를 활용해 MessageSource에 등록된 메시지를 출력한다. 
	1. 우선순위 Code부터 처리한다.
	2. 모든 Code를 다 탐색해서 처리해야 한다.
	3. Code가 없는 경우 DefaultMessage로 처리한다.


![[Custom Message 해결 전략(draw).svg|700]]

위의 그림대로 Validation Error가 발생했을 때 처리해야 한다. 그래서 나는 이러한 에러가 발생했을 때 이를 **ControllerAdvice**에서 처리하고자 한다.

그리고 위 과정을 자동화해야한다.

### MessageSource 설정하기

#### Resources에 생성하기
```properties
# messages.properties
Length.itemRequest.name={0}는 {1} 보다 같거나 짧이야 합니다  
Positive.findItem.id=양수로 좀 넣어라 임마...
```

#### application.yml에서 messagesource 관련 설정해주기

만약 messages.properties가 아닌 다른 곳에 저장하고 이를 MessageSource에서 인식하게 만들려면 application.yml에서 설정해주자

```yml
spring:  
  messages:  
    basename: messages,errors
```
### Code
#### MessageSourceResolvable로 부터 메시지 추출하기
우선 MessageSourceResolvable이냐 아니면 ConstraintViolation이냐에 따라서 다르게 처리해야 한다. 이것을 해결할 수 있는 객체를 하나 설계하려고 한다.

```java
public String getMessage(FieldError fieldError) {  
    String errorMessage;  
    try {  
       errorMessage = messageSource.getMessage(fieldError, LocaleContextHolder.getLocale());  
    } catch (NoSuchMessageException e) {  
       log.debug("No such message code " + Arrays.toString(fieldError.getCodes()));  
       errorMessage = fieldError.getField() + "는 " + fieldError.getDefaultMessage();  
    }  
    return errorMessage;  
}
```

코드를 간단히 설명하자면 단순히 MessageSourceResolvable 타입의 하위 타입인 FieldError를 활용해 메시지를 생성하는 것이다. MessageSourceResolvable 타입은 Code, Argument등의 정보가 포함되어 있기 때문에 MessageSource로 부터 code, argument를 활용해 message를 읽는 역할을 수행한다.

#### ConstraintViolation로 부터 메시지 추출하기

```java
public String getMessage(ConstraintViolation<?> violation) {  
    ConstraintDescriptor<?> descriptor = violation.getConstraintDescriptor();  
    Map<String, Object> attributes = descriptor.getAttributes();  
  
    String annotationName = descriptor.getAnnotation().annotationType().getSimpleName();  
    String rootBeanName = StringUtils.uncapitalize(violation.getRootBeanClass().getSimpleName());  
    String path = violation.getPropertyPath().toString();  
  
    String[] codes = messageCodesResolver.resolveMessageCodes(annotationName, rootBeanName, path, null);  
    String errorMessage = findErrorMessage(codes, attributes, violation.getInvalidValue().toString());  
  
    return errorMessage != null ? errorMessage : violation.getMessage();  
}
```

ConstraintViolation의 경우 정보가 없기 때문에 직접 Code와 Argument 정보를 추출해야 한다.
Code를 MessageCodesResolver를 활용해 만들고 custom message를 가져온다. Attributes로 부터 argument를 뽑아내서 메시지를 직접 수정한다.

#### 커스텀 메시지를 처리하는 객체 전체 Code

```java
@Component  
@RequiredArgsConstructor  
@Slf4j  
public class FieldValidationMessageSource {  
  
    private final MessageSource messageSource;  
    private final MessageCodesResolver messageCodesResolver = new DefaultMessageCodesResolver();  
  
    public String getMessage(FieldError fieldError) {  
       String errorMessage;  
       try {  
          errorMessage = messageSource.getMessage(fieldError, LocaleContextHolder.getLocale());  
       } catch (NoSuchMessageException e) {  
          log.debug("No such message code " + Arrays.toString(fieldError.getCodes()));  
          errorMessage = fieldError.getField() + "는 " + fieldError.getDefaultMessage();  
       }  
       return errorMessage;  
    }  
  
    public String getMessage(ConstraintViolation<?> violation) {  
       ConstraintDescriptor<?> descriptor = violation.getConstraintDescriptor();  
       Map<String, Object> attributes = descriptor.getAttributes();  
  
       String annotationName = descriptor.getAnnotation().annotationType().getSimpleName();  
       String rootBeanName = StringUtils.uncapitalize(violation.getRootBeanClass().getSimpleName());  
       String path = violation.getPropertyPath().toString();  
  
       String[] codes = messageCodesResolver.resolveMessageCodes(annotationName, rootBeanName, path, null);  
       String errorMessage = findErrorMessage(codes, attributes, violation.getInvalidValue().toString());  
  
       return errorMessage != null ? errorMessage : violation.getMessage();  
    }  
  
    private String findErrorMessage(String[] codes, Map<String, Object> attributes, String invalidValue) {  
       for (String code : codes) {  
          Optional<String> message = getMessage(code, null);  
          if (message.isPresent()) {  
             String temp = message.get();  
             for (Map.Entry<String, Object> entry : attributes.entrySet()) {  
                String key = "{" + entry.getKey() + "}";  
                String value = Objects.toString(entry.getValue());  
                temp = temp.replace(key, value);  
             }  
             return temp.replace("{validatedValue}", invalidValue);  
          }  
       }  
       return null;  
    }  
  
    private Optional<String> getMessage(String code, Object[] args) {  
       try {  
          String result = messageSource.getMessage(code, args, LocaleContextHolder.getLocale());  
          return Optional.of(result);  
       } catch (NoSuchMessageException e) {  
          return Optional.empty();  
       }  
    }  
}
```


#### ControllerAdvice에 적용하기

```java
@RestControllerAdvice  
@RequiredArgsConstructor  
@Slf4j  
public class GlobalExceptionHandler {  
  
    private final FieldValidationMessageSource fieldValidationMessageSource;  
  
    @ExceptionHandler(ConstraintViolationException.class)  
    public ResponseEntity<ErrorResponse> handleConstraintViolationException(ConstraintViolationException e) {  
       List<String> errorFieldMessages = new ArrayList<>();  
  
       e.getConstraintViolations().forEach(violation -> {  
          String errorMessage = fieldValidationMessageSource.getMessage(violation);  
          errorFieldMessages.add(errorMessage);  
       });  
  
       log.error(errorFieldMessages.toString());  
       return ResponseEntity.badRequest().body(new ErrorResponse(400, errorFieldMessages.toString()));  
    }  
  
    @ExceptionHandler(MethodArgumentNotValidException.class)  
    public ResponseEntity<ErrorResponse> handleMethodArgumentNotValidException(MethodArgumentNotValidException e,  
       BindingResult bindingResult) {  
       List<String> errorFieldMessages = new ArrayList<>();  
  
       for (FieldError fieldError : bindingResult.getFieldErrors()) {  
          String errorMessage = fieldValidationMessageSource.getMessage(fieldError);  
          errorFieldMessages.add(errorMessage);  
       }  
  
       log.error(errorFieldMessages.toString());  
       return ResponseEntity.badRequest().body(new ErrorResponse(400, errorFieldMessages.toString()));  
    }  
  
    @ExceptionHandler(RuntimeException.class)  
    public ResponseEntity<ErrorResponse> handleInternalException(RuntimeException e) {  
       log.error(e.getClass() + " " + e.getMessage());  
       return ResponseEntity.internalServerError().body(new ErrorResponse(500, "Internal Server Error"));  
    }  
}
```

이제 해당 코드를 이용해서 설계만 해주면 문제를 쉽게 해결할 수 있다.


### 결과 테스트 해보기

code의 우선순위대로 테스트를 하는데 여러 코드가 생성되고 가장 앞에 있는 코드가 가장 긴 경로를 가지고 있어 긴 경로를 설정해주면 그 경로만 커스텀 메시지가 작성되고 그 다음부터는 점점 범용성이 증가한다. 맨 앞에 Length와 같은 어노테이션 이름으로 지정하면 모든 어노테이션에 커스텀 메시지가 적용된다.

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


이런 dto가 있고 
```properties
Length.itemRequest.name={0}는 {1} 보다 같거나 짧이야 합니다
```

이렇게 messageSource에 등록해보자

그리고 컨트롤러 테스트 코드를 짜서 일부로 validation error를 발생시키면 다음과 같은 에러가 발생한다.

**name는 10 보다 같거나 짧이야 합니다, 0보다 커야 합니다**


>[!attention] 주의점
>ControllerAdvice에서 예외처리할때 메시지를 생성하므로 다른 곳에서 ConstraintViolation 예외가 발생했을때 Message 예외처리가 안된다고 당황하지 말자


## 질문 & 확장

(없음)

## 출처(링크)

- https://kapentaz.github.io/spring/Spring-Boo-Bean-Validation-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90/#
- https://catsbi.oopy.io/c61342dd-26c5-4cd8-a462-957cd5787525
- https://velog.io/@imcool2551/Spring-%EA%B2%80%EC%A6%9D1-BindingResult-MessageCodesResolver
- https://www.baeldung.com/spring-custom-validation-message-source
## 연결 노트

- [[Spring bean validation과 MessageCodesResolver]]
- [[MethodArgumentNotValidException]]







