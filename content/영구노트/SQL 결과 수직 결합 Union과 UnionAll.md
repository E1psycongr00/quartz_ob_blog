---
tags:
  - 완성
  - SQL
aliases: 
date: 2024-04-15
title: SQL 결과 수직 결합 Union과 UnionAll
---
작성 날짜: 2024-04-15
작성 시간: 17:20


----
## 내용(Content)

### 결과를 합쳐야 한다.

자주 사용하지는 않지만 SQL를 사용해서 얻은 여러 쿼리 결과를 합치고 싶을 때가 있다.
이런 경우 Union, UnionALL을 사용할 수 있다.

### Union, UnionALL

>[!summary]
>Union은 중복을 제거하고 결과들을 합치고 UnionALL 중복을 고려하지 않고 결과를 합친다.

Union, UnionALL을 이해하기 위해서는 Join과 비교할 필요가 있다.


|       | Union & UnionALL                              | Join                                                   |
| ----- | --------------------------------------------- | ------------------------------------------------------ |
| 결합    | 수직                                            | 수평                                                     |
| 제한 조건 | 수직으로 결합하기 위해 합칠 select 쿼리의 alias 및 타입이 동일해야 함 | 특별한 제한 조건이 없다면 카르테시안 곱을 사용하며 보통은 외래키들을 join조건으로 많이 활용함 |

Join의 경우에는 워낙 유명하고 많이 사용하니 수평 결합이 이해가 될 것있다. Union의
수직 결합과 수평 결합이 이해가 안될 수 있으니 그림으로 살펴보자.

![[union & unionAll(draw).svg]]

그림을 살펴보면 바로 이해가 될 듯 싶긴 한데, 수직으로 결합하다보니 사용 조건이 있다.

- 컬럼명 동일
- 컬럼별로 데이터 타입 동일
- 출력할 컬럼 갯수 동일


### Example

월 별로 잡은 물고기 수를 출력하려고 한다. 이 때 잡은 물고기가 없더라도 month를 출력한다.

![[Pasted image 20240429175553.png]]

위와 같은 테이블 정보를 담고 있다. 



```sql
with months as (
    select 1 as month
    union all select 2
    union all select 3
    union all select 4
    union all select 5
    union all select 6
    union all select 7
    union all select 8
    union all select 9
    union all select 10
    union all select 11
    union all select 12
)

select count(month(f.time)), m.month as month
from fish_info f
right join months m
on (month(f.time) = m.month)
group by m.month
order by month;
```

위와 같이 짜면 모든 month를 1월부터 12월까지 출력하고 count 할 수 있다.

with문을 이용해 1월부터 12월까지의 month라는 컬럼으로 가지고 있는 데이터를 만든다. 그리고 이를 right join을 이용해서 테이블을 합치고 count해주면 된다.

여기서 count를 month(f.time)으로 한 이유는 월별로 물고기를 count해야하기 때문이다.
나오는 결과는 다음과 같다.

![[Pasted image 20240429202418.png]]
## 질문 & 확장

(없음)

## 출처(링크)

- https://silverji.tistory.com/49

## 연결 노트

- [[SQL With (Common Table Expressions)]]








