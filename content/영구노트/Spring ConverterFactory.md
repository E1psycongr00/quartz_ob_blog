---
tags:
  - 완성
  - JAVA
  - Spring
  - databinding
aliases: 
title: Spring ConverterFactory
date: 2023-11-24
---
작성 날짜: 2023-11-24
작성 시간: 14:05


----
## 내용(Content)

### 용도

**ConverterFactory는 보통 여러 컨버터를 사용해야 하는 복잡한 로직 또는 범용성을 위해 선택할 수 있다.**

### 사용법

```java
public class StringToEnumConverterFactory implements ConverterFactory<String, Enum> {  
  
    @Override  
    public <T extends Enum> Converter<String, T> getConverter(Class<T> targetType) {  
       return new StringToEnumConverter(targetType);  
    }  
  
    private static class StringToEnumConverter<T extends Enum<T>> implements Converter<String, T> {  
  
       @Override  
       public T convert(String source) {  
          return Enum.valueOf(this.enumType, source.trim().toUpperCase());  
       }  
    }  
}
```

getConverters에서 복잡한 로직을 처리하면 된다. 최종적으로는 Converter를 리턴하도록 해야 한다.

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

	@Override
	public void addFormatters(FormatterRegistry registry) {
		registry.addConverterFactory(new StringToEnumConverterFactory());
	}
}
```

그리고 다음과 같이 WebConversionService에 등록을 해준다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://engkimbs.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-Converter%EC%99%80-Formatter%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B0%94%EC%9D%B8%EB%94%A9Converter-Formatter-Spring-Data-Binding

## 연결 노트
- [[범용 EnumConverter 만들기]]




