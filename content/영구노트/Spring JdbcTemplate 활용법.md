---
tags:
  - 완성
  - JAVA
  - SQL
aliases: 
title: Spring JdbcTemplate 활용법
date: 2023-11-13
---
작성 날짜: 2023-11-13
작성 시간: 16:07


----
## 내용(Content)

Spring Jdbc를 사용하면 JdbcTemplate를 활용해 sql 쿼리를 실행 할 수 있다. 이 때 Java 객체 (DAO)와 매핑 할 수 있는 SqlMapper를 작성해야 하며, SQL문과 함께 요청하면 DB에 있는 데이터를 자바 객체에 담아 올 수 있다.


### Mapper 작성하기

```java
@Data  
public class Member {  
  
    private final Long id;  
    private final String name;  
    private final int age;  
  
}
```

다음과 같은 Member 객체가 있다고 가정하자. 

이 때 id, name, age에 DB 데이터를 전달하기 위해 다음과 같이 Mapper를 작성한다.

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

### select 문

JdbcTemplate의 **query** 메서드를 활용한다.  여러 객체를 반환할 때는 query를 사용하고, 단일 객체를 반환하고 싶다면 **queryForObject** 메서드를 활용한다.

#### 단일 객체 (queryForObject)
```java
@Override  
public Optional<Member> findById(Long id) {  
    Member member;  
  
    try {  
       member = jdbcTemplate.queryForObject(  
          MemberQuery.FIND_BY_ID.getSql(),  
          new Object[] {id},  
          ITEM_MAPPER);  
    } catch (DataAccessException e) {  
       member = null;  
    }  
  
    return Optional.ofNullable(member);  
}
```

>[!info] try ... catch를 한 이유
>만약 데이터를 찾지 못한 경우 **EmptyResultDataAccessException이** 발생한다.  예외를 null로 처리하고 Optional로 묶기 위해서 처리해주었다.

MemberQuery.FIND_BY_ID 는 `SELECT * FROM members WHERE id = ?` 형태의 동적 쿼리이다.
동적 쿼리를 입력하는 경우 `new Object[] {...args}`를 인자로 넣어주면 된다. 

#### 여러 객체(query)
```java
@Override  
public List<Member> findAllByAge(int age) {  
    return jdbcTemplate.query(  
       MemberQuery.FIND_ALL_BY_AGE.getSql(),  
       new Object[] {age},  
       ITEM_MAPPER  
    );  
}
```

여러 객체를 호출하는 경우에는 따로 예외 처리할 필요가 없다. 빈 리스트가 반환되기 때문이다.

### update 문

```java
@Override  
public boolean update(Member member) {  
    int update = jdbcTemplate.update(  
       MemberQuery.UPDATE.getSql(),  
       member.getName(),  
       member.getAge()  
    );  
    return update == 1;  
}
```

update문은 jdbcTemplate.update를 호출하며 역시 동적으로 사용가능하다. argument를 넣어주면 된다.

위 코드의 update 쿼리는 `UPDATE members SET name = ?, age = ? WHERE id = ?` 이다.

### insert 문

```java
@Override  
public boolean save(Member member) {  
    int update = jdbcTemplate.update(  
       MemberQuery.INSERT.getSql(),  
       member.getName(),  
       member.getAge()  
    );  
    return update == 1;  
}
```

jdbcTemplate.update 문으로 작성한다.

insert문: `INSERT INTO members (name, age) VALUES (?, ?)`


### delete 문

```java
@Override  
public boolean delete(Long id) {  
    int update = jdbcTemplate.update(  
       MemberQuery.DELETE_BY_ID.getSql(),  
       id  
    );  
    return update == 1;  
}
```

역시 update 문으로 작성하낟.

delete 문: DELETE FROM members WHERE id = ?
## 질문 & 확장

(없음)

## 출처(링크)
- https://gmlwjd9405.github.io/2018/12/19/jdbctemplate-usage.html

## 연결 노트










