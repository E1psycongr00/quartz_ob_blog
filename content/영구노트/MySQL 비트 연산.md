---
tags:
  - 완성
  - SQL
  - MySQL
aliases: 
date: 2024-05-10
title: MySQL 비트 연산
---
작성 날짜: 2024-05-10
작성 시간: 20:55



----
## 내용(Content)

### 비트 연산자 사용은 표준인가?

>[!summary]
> ANSI SQL 기준으로 비트 연산자는 표준이 아니다.

[The SQL Standard - ISO/IEC 9075:2023 (ANSI X3.135) - ANSI Blog](https://blog.ansi.org/sql-standard-iso-iec-9075-2023-ansi-x3-135/)에서 자료를 살펴봐도 비트 연산자에 대한 내용은 나와있지 않다. 고로 비트 연산자의 사용은 표준이라 볼 수 없다.

비트 연산자의 사용은 MySQL 또는 SQL Server와 같은 DBMS에서 지원하고 이는 DBMS의 확장 기능일 뿐, ANSI SQL 표준을 따르지는 않는다. 그러므로 DBMS별로 지원하는지 살펴볼 필요가 있다.

### MYSQL의 비트 연산자

MySQL 8.0 기준으로 사용되는 비트 연산자 종류는 다음과 같다.


| 종류  |   역할    |
| :-: | :-----: |
|  &  | 비트 AND  |
| \|  |  비트 OR  |
|  ^  | 비트 XOR  |
|  ~  | 비트 NOT  |
| <<  | 왼쪽 시프트  |
| >>  | 오른쪽 시프트 |

실제 코딩할 때 사용되는 언어와 같기 때문에 이해하는데 어렵지 않다.

### MYSQL에서 제공하는 비트 관련 함수

MySQL에서 제공하는 비트 함수는 다음과 같다.

- BIT_COUNT(): 비트 수를 반환
- BIT_AND(), BIT_OR(), BIT_XOR() => 집계 함수

예를 들어 BIT_AND()를 쓰면 여러 rows들간의 집계 결과를 알려준다. COUNT(), AVG()와 비슷한 결과를 보여준다.

#### example

```sql
CREATE TABLE user_permissions (
    user_id INT PRIMARY KEY,
    permission_mask INT
);
```

```sql
INSERT INTO user_permissions (user_id, permission_mask)
VALUES
    (1, 7),   -- Binary: 0111
    (2, 3),   -- Binary: 0011
    (3, 5);   -- Binary: 0101
```

이런 식으로  `7, 3, 5` 3개의 permission_mask가 주어졌을 때

```sql
SELECT BIT_AND(permission_mask) AS common_permissions
FROM user_permissions;
```

user_permissions 테이블의 permission_mask 컬럼의 모든 데이터에 대한 비트 AND 연산을 수행한다.

$0111_{(2)}$
$0011_{(2)}$
$0101_{(2)}$

이렇게 3개의 row 데이터에 대해서 AND 연산을 취하기 때문에 결과는 $1_{(2)}$가 나온다.  그래서 쿼리의 최종 결과는 

![[Pasted image 20240611001156.png]]



## 질문 & 확장

(없음)

## 출처(링크)

- [MySQL :: MySQL 8.0 Reference Manual :: 14.12 Bit Functions and Operators](https://dev.mysql.com/doc/refman/8.0/en/bit-functions.html)

## 연결 노트