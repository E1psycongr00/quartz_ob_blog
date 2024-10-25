---
tags:
  - 완성
  - SQL
aliases:
  - groupby
  - group by
date: 2024-04-29
title: group by
---
작성 날짜: 2024-04-29
작성 시간: 20:26


----
## 내용(Content)

### Group by

>[!summary]
>SQL를 이용해 데이터를 그룹화해야 하는 상황이 발생할 수 있다. 이럴 때 Group by를 사용한다.

group by는 보통 집계 함수와 많이 사용된다.

>[!info] 대표적인 집계 함수 종류
> 1. COUNT(): 행의 갯수를 셈
> 2. AVG(): 행 안에 있는 값의 평균
> 3. MIN(): 행 안에 있는 값의 최솟값
> 4. MAX(): 행 안에 있는 값의 최댓값
> 5. SUM(): 행 안에 있는 값의 합

집계함수는 행 안에 있는 값을 토대로 어떤 값들을 집계하는데 사용된다. 이 때 group by를 함께 사용하면 group by 행 별로 집계를 할 수 있기 때문에 둘은 거의 같이 쓴다고 봐야 한다.

### Group by 예시

#### Group by + 1 열

>[!summary]
>```SQL
>SELECT sum(Quantity), ProductID
>FROM OrderDetails
>GROUP BY ProductID
>```

가장 많이 쓰는 기본적인 형태이다. 집계함수를 사용한 열과 GROUP BY 대상이 될 열을 함께 SELECT한다.

https://www.w3schools.com/mysql/trymysql.asp?filename=trysql_select_all 에서 SQL을 테스트해볼 수 있는데 OrderDetails 테이블에 다음과 같은 데이터들을 제공한다.

![[Pasted image 20240430164016.png]]

위의 요약을 호출하면

![[Pasted image 20240430164109.png]]

다음과 같이 ProductID별로 Quantity의 총합을 알 수 있다.

#### Group by와 여러 열

>[!summary]
>```SQL
>SELECT sum(Quantity), ProductID, OrderID
>FROM OrderDetails
>GROUP BY ProductID, OrderID
>```

이전 예제에는 ProductID별로 재고의 총합을 알고 싶다.. 그러나 이번엔 ProductID와 OrderID 별로 재고의 총합을 알고 싶을 수도 있다. 이렇게 여러 열을 출력해야 하면서 집계 결과를 알고 싶다면 GROUP By를 여러 번 사용해야 한다.

Group by의 경우 열의 번호를 이용해서 간단하게 표현 가능하기도 하다.

```SQL
SELECT sum(Quantity), ProductID, OrderID 
FROM OrderDetails 
GROUP BY 2, 3
```

#### Group by + Having

>[!summary]
>Having 절은 집계 함수 열의 조건 비교를 위해 사용한다.

Where 절에서는 집계함수를 사용할 수 없다. 그렇기 때문에 집계함수의 조건을 위해서는 Having을 사용해야 한다.

[[group by#Group by + 1 열|groupby + 1열]] 을 살펴보면 ProductID별로 재고의 총합을 조회하는데, 만약 우리가 조회 500 미만은 관심이 없다고 가정하자. 그러면 다음과 같이 SQL문을 쓸 수 있다.

```SQL
SELECT sum(Quantity), ProductID
FROM OrderDetails
GROUP BY 2
Having sum(Quantity) >= 500
```

만약 where 절로 집계 함수를 호출하면 다음과 같은 문제를 발생시킬 수 있다.

![[Pasted image 20240430165440.png]]

#### Group by + where + having

>[!summary]
>where 절은 집계 이전에 필터링되고 having 절은 집계 이후 필터링된다.

where절은 집계 이전에 필터링되기 때문에 집계 함수를 이용한 필터링을 수행할 수 없다. 예를 들어서 재고가 5 이상인 경우만 Sum으로 집계하고 싶다면 이럴땐 Having이 아닌 where 절을 사용하면 된다.

```SQL
# 재고가 5 이상인 Quntity 중에 ProductId별로 총합을 가져오고
# 이 때 총합이 500 이상인 것만 가져와주세요.
SELECT sum(Quantity), ProductID
FROM OrderDetails
WHERE Quantity >= 5
GROUP BY 2
Having sum(Quantity) >= 500
```



## 질문 & 확장

[SQL 실습 사이트](https://www.w3schools.com/mysql/trymysql.asp?filename=trysql_select_all)

## 출처(링크)

- https://kimsyoung.tistory.com/entry/SQL-GROUP-BY-%E4%B8%8A-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%8B%A4%EC%A0%9C-%EC%A0%81%EC%9A%A9-%EB%B0%A9%EB%B2%95

## 연결 노트










