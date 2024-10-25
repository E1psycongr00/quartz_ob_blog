---
tags:
  - 완성
  - JAVA
  - SQL
aliases: 
title: ResultSetExtractor와 RowMapper 차이
date: 2023-11-15
---
작성 날짜: 2023-11-15
작성 시간: 19:58


----
## 내용(Content)

둘 다 DB로부터 데이터를 추출하고 java 객체에 데이터를 전달하는데 사용된다. 차이를 분석해보자.

### RowMapper

RowMapper는 하나의 타입에 1:1 매핑할 때 사용한다. 예를 들어 Member라는 회원 테이블이 있다고 가정하자. 각각의 column들이 자바 객체의 속성과 1:1 매핑이 된다면 그대로 매핑하면 된다. 그래서 RowMapper이라 붙인듯 하다.

```java
public class Member {
	private final Long id;
	private final String name;
	private final int age;
}
```

![[RowMapper(draw).svg|500]]


만약 Member가 다른 외래키와의 관계가 조금 더 복잡하다면 다른 방법을 사용해야 한다.

```java
public class MemberMapper implements RowMapper<Member>  {  
  
    @Override  
    public Member mapRow(ResultSet rs, int rowNum) throws SQLException {  
       return new Member(  
          rs.getLong("id"),  
          rs.getString("name"),  
          rs.getInt("age")  
       );  
    }  
}
```

### ResultSetExtractor

한개의 객체와 1:1 비교를 하는 것이 아니라 여러 객체를 비교해서 DAO를 구성해야 하는 경우에는 ResultSetExtractor가 효과적이다. ResultSetExtractor는 바로 resultset를 매핑하는 것이 아니라, rs.next() 이터레이터를 통해 여러 resultset을 순회하면서 객체를 매핑 가능하다. 

그래서 보통 1:N 관계의 매핑의 경우 ResultSetExtractor를 활용 할 수 있다.

```java
public class MemberWithItemMapper implements ResultSetExtractor<List<MemberWithItems>> {  
    @Override  
    public List<MemberWithItems> extractData(ResultSet rs) throws SQLException, DataAccessException {  
       Map<Long, MemberWithItems> memberMap = new HashMap<>();  
  
       while (rs.next()) {  
          long memberId = rs.getLong("member_id");  
          String memberName = rs.getString("member_name");  
          int memberAge = rs.getInt("member_age");  
          memberMap.putIfAbsent(memberId, new MemberWithItems(memberId, memberName, memberAge, new ArrayList<>()));  
  
          long itemId = rs.getLong("item_id");  
          String itemName = rs.getString("item_name");  
          int itemPrice = rs.getInt("item_price");  
          if (itemId > 0) {  
             Item item = new Item(  
                itemId,  
                itemName,  
                itemPrice,  
                null             );  
             memberMap.get(memberId).getItems().add(item);  
          }  
       }  
       return new ArrayList<>(memberMap.values());  
    }  
}
```

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










