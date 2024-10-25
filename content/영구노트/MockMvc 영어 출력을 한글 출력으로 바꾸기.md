---
tags:
  - 완성
  - 솔루션
  - JAVA
  - 테스트
  - JUnit
aliases: 
title: MockMvc 영어 출력을 한글 출력으로 바꾸기
date: 2023-11-10
---
작성 날짜: 2023-11-10
작성 시간: 23:10


----

## 문제 & 원인

실제 실행 환경과 다르게 영어로 출력되는 경우가 있다. 이상하게 Validation 처리할 때 다른 데서는 한글로 잘 나온다.

Locale이 한글로 설정되도록 하는 방법이 있을까?

## 해결 방안

```java
@SpringBootTest  
class HelloControllerTest {  
  
    @Autowired  
    private WebApplicationContext wac;  
  
    private MockMvc mockMvc;  
  
    @BeforeEach  
    void setUp() {  
       this.mockMvc = MockMvcBuilders.webAppContextSetup(wac)  
          .defaultRequest(get("/").locale(Locale.KOREAN))  
          .build();  
    }
}
```

defaultRequest와 locale을 설정해서  build해주면 된다.
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
