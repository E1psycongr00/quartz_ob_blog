---
tags:
  - 완성
  - Spring
  - databinding
  - JAVA
aliases: 
title: Spring Formatter
date: 2023-11-24
---
작성 날짜: 2023-11-24
작성 시간: 16:00


----
## 내용(Content)

### 사용 용도

Spring Formatter는 Converter와 마찬가지로 데이터 바인딩에 사용되나, 차이점은 쌍방 관계로 구현된다는 점이다. Converter와 비교해서 설명해보자

Converter => object -> object

Formatter => String -> Object, Object -> String

문자열을 바인딩하는데 주로 사용되고, 대부분이 데이터 바인딩은 간단한 Object 변환이 아니면 Formatter를 사용한다. 응답에 형식을 줄 수 있기 때문이다.

### 사용 방법

```java
public class MoneyFormatter implements Formatter<Money> {  
  
    @Override  
    public Money parse(String text, Locale locale) throws ParseException {  
       return Money.of(text);  
    }  
  
    @Override  
    public String print(Money object, Locale locale) {  
       return object.toString();  
    }  
}
```

### 등록하기

WebMvcConversionService에 등록해야 정상적으로 데이터 바인딩이 가능해진다. 

#### bean으로 등록하기
@Component를 이용해 bean으로 등록해주면 된다.


#### WebMvcConfigurer에 등록하기

```java
@Configuration  
public class AppConfig implements WebMvcConfigurer {  
  
    @Override  
    public void addFormatters(FormatterRegistry registry) {  
       registry.addFormatter(new MoneyFormatter());  
    }  
}
```

### 테스트 코드 작성하기

```java
class FormatterConverterTest {  
  
    private static final DefaultFormattingConversionService conversionService = new DefaultFormattingConversionService();  
  
    @BeforeAll  
    public static void setUp() {  
       conversionService.addFormatter(new MoneyFormatter());  
    }  
  
    @Test  
    void test() {  
       String convert = conversionService.convert(new Money(1_000_000), String.class);  
assertThat(convert).isEqualTo("1,000,000 원입니당!!!");
    }  
  
```

## 질문 & 확장

(없음)

## 출처(링크)
- https://engkimbs.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-Converter%EC%99%80-Formatter%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B0%94%EC%9D%B8%EB%94%A9Converter-Formatter-Spring-Data-Binding
- https://ojt90902.tistory.com/679

## 연결 노트
- [[Spring Converter]]









