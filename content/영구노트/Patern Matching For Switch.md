---
tags:
  - 완성
  - JAVA
aliases: 
date: 2024-10-07
title: Patern Matching For Switch
---
작성 날짜: 2024-10-07
작성 시간: 13:54


----
## 내용(Content)

### Switch와 Instanceof

기존에는 instanceof를 switch-case 문에 적용할 수 없었기 때문에 if-else를 이용해서 작성해야 했다.

```java
static String formatter(Object obj) {
    String formatted = "unknown";
    if (obj instanceof Integer i) {
        formatted = String.format("int %d", i);
    } else if (obj instanceof Long l) {
        formatted = String.format("long %d", l);
    } else if (obj instanceof Double d) {
        formatted = String.format("double %f", d);
    } else if (obj instanceof String s) {
        formatted = String.format("String %s", s);
    }
    return formatted;
}
// 출처: https://mangkyu.tistory.com/308 [MangKyu's Diary:티스토리]
```

그러나 21에서는 다음과 같이 사용 가능하다.

```java
static String formatterPatternSwitch(Object obj) {
    return switch (obj) {
        case Integer i -> String.format("int %d", i);
        case Long l    -> String.format("long %d", l);
        case Double d  -> String.format("double %f", d);
        case String s  -> String.format("String %s", s);
        default        -> obj.toString();
    };
}
// 출처: https://mangkyu.tistory.com/308 [MangKyu's Diary:티스토리]
```

### Null 검사

Null 타입은 21 이전 java에서는 switch-case문에서 오류가 발생한다. 그래서 null에 대한 처리를 해줘야했다.

그러나 java21에서는 null에 대한 처리를 switch-case문에서 해줄 수 있다.

```java
public static void testString(String s) {
	switch (s) {
		case null -> System.out.println("Null!!!");
		case "Foo", "bar" -> System.out.println("Great!!!");
		default -> System.out.println("OK BRO!!!!");
	}
}
```

### Switch 문과 When

Switch문을 쓰다 보면, Switch 문 내부에서 분기 처리와 같은 복잡한 작업이 있을 수 있다. 그러나 중첩 조건문은 가독성을 떨어뜨리는 문제점이 있다.

```java
public static String testString(String response) {
	return switch (response) {
		case null -> {throw new NullPointerException();}
		case String s -> {
			if (s.equalsIgnoreCase("LEFT")) {
				yield "L";
			}
			if (s.equalsIgnoreCase("RIGHT")) {
				yield "R";
			}
			yield "NONE";
		}
	};
}
```

매우 심플한 코드지만 String 타입이라면 s로 할당해서 그 안에 if문을 이용해 다시 분기를 처리하고 있다. 이런 중첩된 패턴은 로직이 복잡해지면 코드 가독성을 해칠 수 있고, 유지보수하기 쉽지 않다.

```java
public static String testString(String response) {
	return switch (response) {
		case null -> {throw new NullPointerException();}
		case String s
		when s.equalsIgnoreCase("LEFT") -> "L";
		case String s
		when s.equalsIgnoreCase("RIGHT") -> "R";
		default -> "NONE";
	};
}
```

java 21에서는 when 을 활용해 중첩 분기문을 중첩된 문처럼 보이지 않고 가독성 편하게 코드를 제공하고 있다.

### enum 인터페이스와 switch 문

interface를 상속받은 enum을 다음과 같이 쉽게 처리할 수 있다.

```java

public sealed interface Food permits Pizza {}

public enum Pizza implements Food {
    RICE_PIZZA, PEPE_PIZZA
}

public static void test(Food f) {
	switch (f) {
		case Pizza.RICE_PIZZA -> System.out.println("RICE!!");
		case Pizza.PEPE_PIZZA -> System.out.println("PEPE!!");
		default -> System.out.println("NO!!!");
	}
}
```


## 질문 & 확장

(없음)

## 출처(링크)

- https://mangkyu.tistory.com/308
- https://openjdk.org/jeps/441
## 연결 노트



