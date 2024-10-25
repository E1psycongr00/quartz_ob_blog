---
tags:
  - 완성
  - SQL
  - CTE
aliases:
  - Common Table Expressions
  - CTE
date: 2024-04-29
title: SQL With 문
---
작성 날짜: 2024-04-29
작성 시간: 18:06


----
## 내용(Content)


### CTE (Common Table Expressions)

>[!summary]
>CTE는 일시적인 결과 집합을 생성하며, 마치 생성된 테이블처럼 참조해서 사용가능하다.

```sql
WITH cte_name AS (
	-- 여기에 서브쿼리를 작성한다.
)
-- 이후 일반적인 쿼리 작성
-- cte.name을 마치 실제 테이블인것 처럼 사용 가능

SELECT * FROM cte_name;
```

>[!example]
>우리가 employees 테이블에서 급여가 평균 이상인 직원들을 찾고 싶다고 가정하자. 이를 위해서 cte문을 사용해볼 수 있다.
>```SQL
>WITH AvgSalary AS (
>	SELECT AVG(salary) AS average FROM emplyees
>)
>SELECT e.name, e.salary
>FROM emplyees e, AvgSalary a
>WHERE e.salary > a.average;
>```

위에 With 문을 AvgSalary 이름으로 테이블을 정의하고, employees 테이블에서 salary 평균을 가져오는 서브 쿼리문을 작성하고 있다. 

그리고 이를 가져와서 사용하면 된다. join ,from 등등 많은 부분에 응용할 수 있다.

### 서브 쿼리의 장단점

#### 장점

- 복잡한 쿼리를 WIth문으로 분리할 수 있고, 이를 통해 가독성을 향상시킬 수 있다.
- with문을 사용하면 동일한 서브쿼리를 여러번 사용할 수 있어 재활용하기 쉽다.
- with 문을 사용하면 각 부분을 개별적으로 테스트하고, 디버깅이 쉬워진다.

#### 단점

- with문은 일시적인 집합을 생성하기 때문에 매우 큰 데이터 세트에 대해 작업할 때 메모리 사용량이 증가해 성능 이슈가 발생할 수 있다.
- WITH문은 SQL 고급 기능이기 때문에, 초보자가 이해하기는 쉽지 않을 수 있다.

>[!tip]
>- 복잡한 쿼리를 여러 개의 간단한 부분으로 나누고 싶을때 사용하자
>- 동일한 서브쿼리를 재활용해야 할 때 사용해보자
>- 큰 데이터를 with문으로 생성하지 말자

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










