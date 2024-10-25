---
tags:
  - 완성
  - Spring
  - databinding
aliases: 
title: 범용 EnumConverter 만들기
date: 2023-11-24
---
작성 날짜: 2023-11-24
작성 시간: 14:31


----

## 문제 & 원인

여러 Enum 타입에 대해서 대소문자 구분 없이 String  -> 특정 enum 타입으로 바꾸는 방법이 필요한데 기존의 converter로는 쉽지 않다.
## 해결 방안

### ConverterFactory 작성

```java
@SuppressWarnings("rawtypes")  
public class StringToEnumConverterFactory implements ConverterFactory<String, Enum> {  
  
    @SuppressWarnings("unchecked")  
    @Override  
    public <T extends Enum> Converter<String, T> getConverter(Class<T> targetType) {  
       return new StringToEnumConverter(targetType);  
    }  
  
    private static class StringToEnumConverter<T extends Enum<T>> implements Converter<String, T> {  
  
       private final Class<T> enumType;  
  
       public StringToEnumConverter(Class<T> enumType) {  
          this.enumType = enumType;  
       }  
  
       @Override  
       public T convert(String source) {  
          return Enum.valueOf(this.enumType, source.trim().toUpperCase());  
       }  
    }  
}
```

### 등록
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

	@Override
	public void addFormatters(FormatterRegistry registry) {
		registry.addConverterFactory(new StringToEnumConverterFactory());
	}
}
```

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
- [[Spring ConverterFactory]]