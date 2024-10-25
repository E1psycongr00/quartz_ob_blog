---
tags:
  - 완성
  - JAVA
  - SQL
aliases: 
title: JDBC와 1대 N 조인
date: 2023-11-15
---
작성 날짜: 2023-11-15
작성 시간: 19:49


----
## 내용(Content)

### 문제점

Jdbc를 활용해 1:N join을 해야하는 경우를 살펴보자

예를 들어  Member : Item = 1 : N 관계를 가진다고 가정하자. 이런 경우 Member에 List\<Item> 을 가질 수 있다.  그러나 이런 경우 매핑은 쉽지 않은데 어떤 방식으로 매핑할 지 살펴보자.

우선 Member과 다음과 같다고 가정해보자

```java
public class Member {

	private final Long id;
	private final String name;
	private final int age;
	private final List<Item> items;
}
```

속성들을 보면 id, name, age는 모두 1:1 매핑이 가능하지만 items의 경우에는 Item 테이블과 join해서 데이터를 가져와서 매핑해줘야 한다. 문제는 join할 때는 해당 컬럼의 여러 데이터를 가져오는데 이것은 Member객체 내부의 컬렉션으로 구성된 객체와 매핑이 되지 않는다는 것이다.

이런 문제를 해결하기 위해 ResultSetExecutor를 사용해서 해결한다.

### 해결 전략

1. 여러 데이터와 List\<Items> 을 업데이트하기 위해서 HashMap을 활용한다. 
	- key는 ID, value는 Member 객체를 활용한다. 
2. ID를 통해 Member 객체를 찾아나가고, items 데이터를 업데이트한다.
3. HashMap을 List 형태로 펼쳐서 반환한다.


### code
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


코드를 분석해보면 우선 Member를 식별해서 저장하기 위해 memberId를 기준으로 HashMap을 만든다.

그 이후 rs.next() 이터레이터를 통해 가져온 데이터를 순회하면서 Member를 생성하고 hashMap 안에 데이터를 초기화한다. 만약 itemId가 존재한다면 Item 객체를 만들어주고 List 컬렉션 형태로 되어있는 items 에 집어넣는다. 그 이후 펼쳐서 반환한다.

>[!info] itemId가 0인 경우
>getLong("item_id") 메서드의 경우 찾지 못하면 0을 반환한다. 그렇기 때문에 0보다 큰 경우로 분기 조건을 처리한 것이다.



## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트

- [[ResultSetExtractor와 RowMapper 차이]]









