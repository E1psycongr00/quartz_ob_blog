---
tags: 
  - 완성
  - IT
  - JAVA
  - Annotation
aliases:
title: 어노테이션 @Target
date: 2023-11-10

---
작성 날짜: 2023-11-10
작성 시간: 14:57


----
## 내용(Content)

@Target 어노테이션은 적용할 대상을 지정하는데 사용하는 메타 어노테이션이다. 

ElementType enum 클래스에 작성되어 있다.

### 종류


| 종류             | 적용할 타겟                    |
| ---------------- | ------------------------------ |
| TYPE             | class, interface, enum, record |
| FIELD            | Field, enum constant           |
| METHOD           | Method                         |
| PARAMETER        | Paramter                       |
| CONSTRUCTOR      | Constructor                    |
| TYPE_PARAMETER   | 제네릭 매개변수                |
| TYPE_USE         | 타입이 사용되는 모든 대상      |
| MODULE           | Module                         |
| RECORD_COMPONENT | RecordMember                   |

TYPE_USE의 경우 제네릭 매개변수에도 어노테이션이 지정 가능하다는 이야기이다.

ex)
```java

private final List<@Length(max=3) String> names;
```


## 질문 & 확장

(없음)

## 출처(링크)
- https://ittrue.tistory.com/160

## 연결 노트










