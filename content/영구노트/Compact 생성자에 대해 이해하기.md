---
tags:
  - 완성
  - JAVA
aliases: 
date: 2024-10-06
title: Compact 생성자에 대해 이해하기
---
작성 날짜: 2024-10-06
작성 시간: 19:42


----
## 내용(Content)

### Compact 생성자 동작 원리

![[Compact 생성자 동작 원리 (draw).svg]]

```java
final class Record {
	private final String variable;

	public Record(String name) {
		validate(name);  // compact
		transform(name); // compact
		this.name = name; // field initialize
	}
}
```

1. Record 인스턴스 생성 단계에서는 말 그대로 Record 인스턴스를 생성한다. 그러나 아직 필드는 초기화되지 않았고, 접근이 불가능하다.
2. Compact 생성자 실행 단계에서는 여전히 필드가 초기화되지 않았기 때문에 this를 이용해 필드 값에 접근하는 것은 불가능하다. 그러나 vaidation이나 인자로 받은 값을 변형 가능하다.
3. 최종적으로 Compact 생성자에서 변형된 인자를 이용해 필드를 초기화한다.

Compact 생성자는 실제 필드에 접근할 수 없지만 Validation과 정규화가 가능하다. 이 말은 필드에 초기화되기 전에 무결성 검사와 안전하게 데이터를 변형 가능하다는 것이다.

### 일급 객체 예시

일급 객체는 Value Object에 가깝다고 볼 수 있다. 그리고 내부에는 배열 또는 list와 같은 멀티 데이터 객체가 있으며, 이러한 내부 객체 데이터들을 이용해 HashCode와 Equals를 구성한다.
Record는 매우 이것을 쉽게 할 수 있다.

```java
public record Datas<T>(List<T> list) {
    
    public Datas {
        validate(list);
        list = List.copyOf(list);
    }

    public static <K> void validate(List<K> list) {
        Objects.requireNonNull(list);
    }
}

```

static으로 validate 메서드를 구성했다. 인스턴스가 생성 직 후 동작하기 때문에 정적 메서드가 아닌 일반 메서드를 사용할 수 있지만, 내부 필드가 초기화되지 않았고, 필드에 접근할 수 없기 때문에 코드의 안정성을 얻기 위해 정적 메서드로 선언한다.

그 이후 list 변수를 copyOf를 이용해 재 정의하고 있다. 그러면 최종적으로 변경된 list가 필드에 초기화된다.

컴파일러는 해당 코드를 어떻게 바꾸는지 살펴보자.

```java
// Datas.class
public record Datas<T>(List<T> list) {
   public Datas(List<T> list) {
      validate(list);
      list = List.copyOf(list);
      this.list = list;
   }

   public static <K> void validate(List<K> list) {
      Objects.requireNonNull(list);
   }

   public List<T> list() {
      return this.list;
   }
}
```

생성된 .class 코드를 보면 Datas 생성자를 구성하는데 compact 생성자 코드가 처음에 생성되고, 그 이후 필드에 데이터를 할당하는 코드가 추가됨을 알 수 있다. 

## 질문 & 확장

콤팩트 생성자는 Value Object를 만들 떄 매우 효과적이며, POJO(Plain Old Java Object)를 만족한다. validation과 정규화를 Compact 생성자의 분리하고, 불필요한 보일러 플레이트 코드를 작성할 필요 없기 떄문에 앞으로 유용하게 사용될 문법인 것 같다. 다만 final로 엄격하고 JPA의 Entity를 만들 때는 쓰기 힘들 것이다.

## 출처(링크)


## 연결 노트










