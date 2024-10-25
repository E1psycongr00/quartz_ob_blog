---
tags: 
  - 완성
  - Spring
  - Validation
aliases: 
title: index 기반으로 ConstraintViolation을 MessageSource로 활용하기
date: 2023-12-08
---
작성 날짜: 2023-12-08
작성 시간: 20:37


----
## 내용(Content)

### ConstraintViolation에서 FieldError 추출하기

ConsraintViolation은 여러 정보들을 가지고 있지만,  code가 없기 떄문에 MessageCodesResolver를 활용해 Code를 생성해줘야 한다.

### code

```java
@Component  
@RequiredArgsConstructor  
@Slf4j  
public class IndexBasedViolationMessageResolver {  
  
    private static final String[] EXCLUDED_ATTRIBUTES = {"groups", "payload", "message"};  
    private static final int FIRST_ARGUMENT_INDEX = 0;  
  
    private final MessageSource messageSource;  
    private final MessageCodesResolver messageCodesResolver = new DefaultMessageCodesResolver();  
  
    public String getMessage(ConstraintViolation<?> violation) {  
       ConstraintDescriptor<?> descriptor = violation.getConstraintDescriptor();  
       Map<String, Object> attributes = descriptor.getAttributes();  
  
       String annotationName = descriptor.getAnnotation().annotationType().getSimpleName();  
       String rootBeanName = StringUtils.uncapitalize(violation.getRootBeanClass().getSimpleName());  
       String path = violation.getPropertyPath().toString();  
       String lastName = getLastName(path);  
       String invalidValue = violation.getInvalidValue().toString();  
       String[] codes = messageCodesResolver.resolveMessageCodes(annotationName, rootBeanName, path, null);  
  
       List<Object> args = buildArguments(attributes, lastName);  
       FieldError fieldError = createFieldError(rootBeanName, path, invalidValue, codes, args, violation.getMessage());  
       return messageSource.getMessage(fieldError, LocaleContextHolder.getLocale());  
    }  
  
    private List<Object> buildArguments(Map<String, Object> attributes, String lastName) {  
       List<Object> args = new ArrayList<>(attributes.entrySet().stream()  
          .filter(entry -> !excludedAttribute(entry.getKey()))  
          .sorted(Map.Entry.comparingByKey())  
          .map(Map.Entry::getValue)  
          .toList());  
       args.add(FIRST_ARGUMENT_INDEX, lastName);  
       return args;  
    }  
  
    private boolean excludedAttribute(String key) {  
       return Arrays.asList(EXCLUDED_ATTRIBUTES).contains(key);  
    }  
  
    private FieldError createFieldError(String rootBeanName, String path, String invalidValue, String[] codes, List<Object> args, String defaultMessage) {  
       return new FieldError(rootBeanName, path, invalidValue, false, codes, args.toArray(new Object[0]), defaultMessage);  
    }  
  
    private String getLastName(String path) {  
       int index = path.lastIndexOf('.');  
       return index == -1 ? path : path.substring(index + 1);  
    }  
}
```


## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










