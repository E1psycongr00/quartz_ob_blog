---
tags:
  - 완성
  - JAVA
  - Spring
  - databinding
aliases: 
title: Spring Converter
date: 2023-11-24
---
작성 날짜: 2023-11-24
작성 시간: 14:03


----
## 내용(Content)

### 사용하는 이유
Spring Converter는 Controller에서 데이터를 받아올 때 데이터간 타입 변환에 주로 사용된다. 
주로 String -> object 또는 Integer -> object로 많이 사용되며, object -> object와 같이도 사용할 수 있다.


### Spring에서 컨버터를 관리하는 방법

Spring에서는 Converter를 MVC ConversionService를 이용해 관리한다. 해당 객체는 여러 conversion을 보관하고 있다가 데이터 바인딩이 필요한 시점에 호출된다.

컨버터를 ConversionService에 주입하는 데는 2가지 방법이 있다.

1. Bean 을 이용해 등록하기
2. WebConfigurer 활용하기

### 사용 방법
**org.springframework.core.convert.converter.Converter** 인터페이스를 구현하면 된다.

```java
@FunctionalInterface  
public interface Converter<S, T> {  
  
    T convert(S source);  
  
     default <U> Converter<S, U> andThen(Converter<? super T, ? extends U> after) {  
       Assert.notNull(after, "'after' Converter must not be null");  
       return (S s) -> {  
          T initialResult = convert(s);  
          return (initialResult != null ? after.convert(initialResult) : null);  
       };  
    }  
}
```

함수형 인터페이스로  andThen을 통해 여러 컨버터를 연결해서 사용 가능하다. FunctionalInterface이기 때문에 람다로 간단하게 작성하는 것도 가능하다.

### MVC ConversionService에 등록하기

#### Bean으로 등록하기

구현한 class에 @Component를 붙여주면 된다.

#### WebConfigurer에 등록하기

```java
@Configuration  
public class AppConfig implements WebMvcConfigurer {  
  
    @Override  
    public void addFormatters(FormatterRegistry registry) {  
       registry.addConverter(new MoneyConverter());  
    }  
}
```


### 테스트 코드 작성하기

Converter는 ConversionService가 보관할 수 있다고 했다. 테스트 시에는 **DefaultFormattingConversionService** 활용해서 여러 컨버터들을 등록하고 꺼내서 테스트할 수 있다.


```java
class FormatterConverterTest {  
  
    private static final DefaultFormattingConversionService conversionService = new DefaultFormattingConversionService();  
  
    @BeforeAll  
    public static void setUp() {  
       conversionService.addConverterFactory(new StringToEnumConverterFactory());  
    }  
  
    @Test  
    void test() {  
       Money money = conversionService.convert("1,000,000", Money.class);  
       assertThat(money).isInstanceOf(Money.class)  
          .hasFieldOrPropertyWithValue("value", 1_000_000);  
    }  
  
}
```


## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트

- [[Spring ConverterFactory]]










