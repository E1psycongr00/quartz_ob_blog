---
tags:
  - 완성
  - JAVA
  - Spring
  - SQL
aliases: 
title: schema.sql & data.sql 활용해 더미 데이터 생성하기
date: 2023-11-13
---
작성 날짜: 2023-11-13
작성 시간: 16:23


----
## 내용(Content)

### Schema.sql과 data.sql 용도

Spring에서 Resource에 Schema.sql과 data.sql을 정의해두면  Spring Boot에서는 이를 자동으로 인식하고 db에 실행한다. Schema.sql의 경우는 데이터 스키마 즉 테이블 구조에 대해서 정의해고 data.sql은 초기 데이터를 보관하거나 생성하는데 쓰인다.

### 실행 순서

Schema.sql -> data.sql


### 실행 원리

DataSourceInitializer 클래스는 db의 데이터를 초기화하고 삭제를 담당하는 클래스이다.
DatabasePopulator를 통해 해당 경로에 있는 .sql 파일을 가져오고 초기화를 진행한다.


### application.yml 설정하기

resource 하위 디렉토리가 아닌 다른 곳에 설정해주었다면 경로를 명시해주어야 한다.

```yml
spring:  
  h2:  
    console:  
      enabled: true  
      path: /h2-console  
  datasource:  
    driver-class-name: org.h2.Driver  
    url: jdbc:h2:mem:testdb  
    username: sa  
```

## 질문 & 확장

(없음)

## 출처(링크)
- https://engineerinsight.tistory.com/77
- https://woodadada16.tistory.com/30

## 연결 노트










