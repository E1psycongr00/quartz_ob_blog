---
tags:
  - 완성
  - JAVA
  - SQL
aliases: 
title: JDBC와 JOIN
date: 2023-11-15
---
작성 날짜: 2023-11-15
작성 시간: 15:10


----
## 내용(Content)

### 기존과 다른 문제점

RowSqlMapper 사용시  단순 데이터 맵핑이 아니라 Nested class가 있다고 가정하자

```java
@Getter  
@RequiredArgsConstructor  
@ToString  
public class Item {  
    private final Long id;  
    private final String name;  
    private final int price;  
    private final Member member;  
}
```

이 코드에는 Member 클래스가 있고 이 정보를 매핑하고 싶다. 이런 경우에 어떻게 짜야 할까?


### SQL 코드 짜기
```sql
SELECT i.id as id,
	i.name as name,
	i.price as price,
	m.id as member_id,
	m.name as member_name,
	m.age as member_age
FROM items i
LEFT JOIN members as m on m.id = i.member_id
```

```java
public class ItemMapper implements RowMapper<Item> {  
    @Override  
    public Item mapRow(ResultSet rs, int rowNum) throws SQLException {  
       Member member = new Member(  
          rs.getLong("member_id"),  
          rs.getString("member_name"),  
          rs.getInt("member_age")  
       );  
       return new Item(  
          rs.getLong("id"),  
          rs.getString("name"),  
          rs.getInt("price"),  
          member  
       );  
    }  
}
```

>[!warning] SQL 작성시 주의해야 할 점
>SQL 작성시 column 네임을 명확히 해야 ResultSet으로 부터 가져오는데 오류를 줄일 수 있다. Join의 경우에는 가져올때 name이 column name이랑 똑같이 가져오는 경우가 생길 수 있기 때문이다. 그래서 as로 column name을 반드시 명시해주자





## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










