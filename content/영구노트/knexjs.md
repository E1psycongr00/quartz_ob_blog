---
tags:
  - 완성
  - knex
  - JS
  - Typescript
aliases:
  - knex
  - Knex
date: 2024-03-18
title: knexjs
---
작성 날짜: 2024-03-18
작성 시간: 23:49


----
## 내용(Content)
![[Pasted image 20240319000336.png]]

### Knex
>[!summary]
>노드 스타일의 콜백, promise 인터페이스 기반으로 동작하는 SQL query builder 이다.스키마 빌더 기능 제공, 트랜잭션 지원, 커넥션 풀링과 표준화된 응답 지원한다.

Knex는 **PostgreSQL**, **CockroachDB**, **MSSQL**, **MySQL**, **MariaDB**, **SQLite3**, **Better-SQLite3**, **Oracle** 등 db를 지원하며 이들을 손쉽게 활용할 수 있다. typescript를 통해 정적으로 query와 스키마를 작성할 수 있다. java의 querydsl과 유사한 라이브러리라 할 수 있다.


### why Knex
>[!summary]
가벼운 프로젝트 또는 쿼리를 빠르게 재사용하거나 IDE 도움을 받아 빠르게 개발하고 싶다면  SQL Query Builder를 사용하는 것이 적합하다.

Knex를 사용하는 이유는 Knex는 SQL Query Builder 라이브러리이기 때문이다. 그러면 SQL Query Builder가 ORM이나 raw Query에 비해 어떤 이점이 있는지 알아보자.

**SQL Query Builder vs raw query vs ORM**

|        | Raw Query | Query Builder | ORM |
| ------ | --------- | ------------- | --- |
| 러닝 커브  | 중간        | 중간            | 높음  |
| 쿼리 자유도 | 높음        | 중간            | 낮음  |
| 생산성    | 낮음        | 높음            | 높음  |
| 범용성    | 낮음        | 높음            | 높음  |

orm의 경우 객체 자체를 sql에 맵핑하면서 동작 원리나 구조들을 잘 알아야 사용할 수 있는 반면 querybuilder는 기본적이 SQL만 알아도 쉽게 사용 가능하다. 또한 ORM에 비해 쿼리 제약조건이 보다 자유롭고 IDE의 도움을 받을 수 있기 때문에 생산성이 높다.


## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










