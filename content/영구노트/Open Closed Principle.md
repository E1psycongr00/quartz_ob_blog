---
tags:
  - 완성
  - 객체지향
aliases: 
date: 2024-10-24
title: Open Closed Principle
---
작성 날짜: 2024-10-24
작성 시간: 12:10


----
## 내용(Content)

### Open Closed Principal

>[!summary]
>소프트웨어 구성 요소는 확장에 대해서 개방(Open)되어야 하지만, 변경에 대해서는 폐쇠(Closed)되어야 한다. 

OCP는 쉽게 의미하면 코드의 변경은 최소화하고 확장(새로운 코드를 추가)하는 방식으로 구성되어야 한다.

코드를 수정하면, 결합되어 있는 모듈 간의 사이드 이펙트를 발생할 가능성이 높아진다. 그렇기 떄문에 버전이 높아질수록 이전 코드와는 호환되지 않을 가능성이 높아진다는 의미가 된다. 확장은 기능을 새로운 파일에 추가하는 식으로 확장하기 떄문에 변경에 닫혀있고, 사이드 이펙트 가능성이 낮아진다.

>[!caution]
>소프트웨어 구성 요소란 Component, Class, Module, Function 등을 의미한다. 

### OCP를 적용하는 방법

OCP를 적용하는 방법은 다양한 방법이 있지만 주요 방법들은 다음과 같다.

1. 추상화와 다형성 적용
2. 의존성 주입(DI)
3. 전략 패턴과 같은 디자인 패턴 사용


### OCP에 위배된 코드 예제

```java
import java.util.HashMap;
import java.util.Map;

public class Animal {
    private final String name;
    private final int age;
    private final String species;
    private static final Map<String, Integer> defaultSpeed;

    static {
        defaultSpeed = new HashMap<>();
        defaultSpeed.put("cheetah", 120);
        defaultSpeed.put("rabbit", 60);
        defaultSpeed.put("turtle", 5);
        defaultSpeed.put("snake", 4);
        defaultSpeed.put("human", 10);
    }
    public Animal(String name, int age, String species) {
        this.name = name;
        this.age = age;
        this.species = species;
    }

    public int run() {
        // 4발 동물은 + 10
        // 2발 동물은 + 5
        // 나머지는 기본 속도
        int speed = defaultSpeed.getOrDefault(species, 0);
        if (species.equals("cheetah") || species.equals("rabbit")) {
            return speed + 10;
        } else if (species.equals("turtle") || species.equals("snake")) {
            return speed + 5;
        }
        return speed;
    }

}
```

위 코드는 Animal이라는 클래스가 있고, Species라는 타입을 만들고 defaultSpeed Map을 활용해 기본 스피드를 지정해주고 있다. 그리고 run() 메서드는 4발 동물 또는 2발 동물에 따라 속도가 다르며 나머지는 기본 속도를 가진다.

그런데 이 코드의 문제점은 무엇일까?

새로운 동물이 추가될 때마다, run 메서드, defaultSpeed를 매번 업데이트해줘야 하고, if 분기문도 매우 복잡해진다. 또한 동물이 4발인지, 2발인지 실수할 가능성도 생긴다.

즉, 매번 기능이 추가될 때마다 코드를 수정해야 하므로, 변경에는 열려있고, 확장에는 닫혀있는 OCP 원칙과는 완전히 반대의 구조로 설계되어 있다.

### 추상 클래스와 상속을 이용해서 해결해보기

#### 분석

동물에 4발과 2발로 달리기 속도가 달라지고 구분된다. 근데 동물은 4발일 수도 있고, 두 발일 수도 있다. 즉 is-a 관계라는 것이다. 그리고 우리는 동물의 역할 책임이 달리기(run)인 것을 알 수 있다.

![[Animal is-a 관계 (draw).svg]]

#### 인터페이스 정의 

```java
public interface Animal {
    int run();
}

```

동물의 역할 책임을 분석해서 추상화하면 결국엔 동물은 달리기를 할 책임이 있다. 다만 인스턴스에 따라 달리기 결과값이 달라지기는 하지만 중요한 것은 동물 객체는 달리는 책임이 있다는 것이다.

#### 상속 클래스 정의

```java
public abstract class AbstractAnimal implements Animal{
    private final String name;
    private final int age;

    protected AbstractAnimal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public int run() {
        int speed = speedLogic();
        return speed + 10;
    }

    protected abstract int speedLogic();

    @Override
    public String toString() {
        return "Name: " + name + ", Age: " + age;
    }
}
```

상속을 보다 편하게 하기 위해서 Abstract 클래스를 사용한다. 여기서 중요한 것은 speedLogic이라는 메서드를 추가했고 중요한 speed에 관련된 로직은 자식에게 위임하고 Animal의 틀을 작성하는 것이다.

이 코드의 좋은 점은 Animal의 종류가 다양해져도 상속해서 다양한 클래스를 만들 수 있고, 해당 클래스가 speed에 대한 책임을 가지지 않기 때문에 동물에 따라 speed가 달라져도 해당 코드를 수정할 일이 없다.

#### 자식 클래스 정의 

```java 두 발 클래스 정의
public class TwoLegedAnimal extends AbstractAnimal {

    private static final int TWO_LEGGED_SPEED = 5;

    public TwoLegedAnimal(String name, int age) {
        super(name, age);
    }

    @Override
    protected int speedLogic() {
        return TWO_LEGGED_SPEED;
    }
}

```

TwoLegedAnimal은 속도 5라는 고유한 특성을 가지고 있고, speedLogic에서 이를 활용하고 있다. 이렇게 특성마다 달라지는 속도의 책임을 자식 클래스가 가지기 때문에 새로운 동물 종류가 필요하다면, 상속해서 클래스를 생성해서 로직을 새로 짜기만 하면 된다.

이것은 확장에는 열려 있는 코드가 되는 것이고, 변경에는 닫혀있는 OCP를 만족하는 코드가 되는 것이다. 또한 상속의 효과로 불필요한 구현 코드를 피하고, 책임이 필요한 코드만 작성하면 되기 때문에 깔끔한 코드를 작성할 수 있다.

### 클래스 분리와 주입을 이용해 해결해보기

#### 분석

코드를 보면 is-a 관계에 가깝긴 하지만, 예시를 위해 has-a 관계로 접근해보려 한다.
Animal은 2발 다리를 가질 수 있고, 4발 다리를 가질 수 있다. 그렇다면 다리 상태에 따라 다리는 고유의 속도를 가지고 있고, 이러한 상태에 따라 각기 다른 속도를 리턴하도록 해서 속도 로직을 분리하는 것이다.

#### code

```java
public enum Legs {
    TWO_LEGGED(5),
    FOUR_LEGGED(10);

    private final int speed;

    Legs(int speed) {
        this.speed = speed;
    }

    public int getSpeed() {
        return speed;
    }
}

```

```
```
```java
public class DefaultAnimal implements Animal{
    private final String name;
    private final int age;
    private final Legs legs;

    protected DefaultAnimal(String name, int age, Legs legs) {
        this.name = name;
        this.age = age;
        this.legs = legs;
    }

    @Override
    public int run() {
        int speed = legs.getSpeed();
        return speed + 10;
    }

    @Override
    public String toString() {
        return "Name: " + name + ", Age: " + age;
    }
}
```

이렇게 사용하면 좋은 점은 Legs를 독립적으로 관리할 수 있고, DefaultAnimal과 긴밀한 관계가 아닌 하나의 부품처럼 다룰 수 있기 때문에 큰 수정이나 로직 변경이 발생하더라도 Legs를 제거만 하면 된다. 다만 Legs가 Animal을 위한 로직이지만 너무 독립적이면, Legs의 용도가 헷갈릴 수 있다. 

### 정리


|            | **상속**                                      | **Composite or Aggregate**                                                          |
| ---------- | ------------------------------------------- | ----------------------------------------------------------------------------------- |
| **설계 난이도** | **높음**                                      | **낮음**                                                                              |
| **구현 난이도** | **높음**                                      | **낮음**                                                                              |
| **변경 난이도** | **부모 클래스의 경우 변경이 힘듬**                       | **독립적인 모듈로 주입하기 때문에 쉬움**                                                            |
| **의존도**    | **높음** (부모와 긴밀한 연결 관계)                      | **낮음**                                                                              |
| **코드 길이**  | **적음** (Abstract를 이용하면 중복 코드를 엄청나게 줄일 수 있음) | **중간** (단순한 독립적인 관계이기 때문에 코드 수가 엄청 줄어들지는 않음. 물론 여기에도 주입하는 클래스에 Abstract 적용할 수도 있음.) |


**상속으로 구현**
- 장점: 간편한 구현, 긴밀한 연결 관계, 중복 코드 X, 깔끔한 코드
- 단점: 부모 클래스의 변경은 유지 보수가 힘들고 사실상 힘듬(긴밀한 연계)

**Composite 또는 Aggregate 구현**
- 장점: 간편한 구현, 독립적인 연결 관계, 수평 관계임으로 따로 관리가 쉬움
- 단점: 독립적인 관계라 로직을 다른 도메인에 쓸 위험이 존재, 관리해야 할 변수가 늘어남.(주입 or 매개변수)
## 질문 & 확장


is-a 관계가 명확하고 계층관계를 명확하게 설계하고 추상화 가능하다면 상속관계를 추천한다. 그러한 관계가 아니라면 has-a 관계로 인터페이스를 추상화하고, 논리를 다른 곳에서 구현 및 주입하는 형태가 OCP 입장에서 바람직하다. 새로운 논리가 추가되면, 추가로 구현하면 되기 떄문이다. 그리고 대대적인 변경이 있을 때 그냥 떼어내기만 하면 된다.

## 출처(링크)

- https://dublin-java.tistory.com/48
- https://velog.io/@harinnnnn/OOP-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-5%EB%8C%80-%EC%9B%90%EC%B9%99SOLID-%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84-%EC%9B%90%EC%B9%99-OCP

## 연결 노트






