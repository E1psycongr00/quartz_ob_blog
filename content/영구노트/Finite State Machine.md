---
tags:
  - 완성
  - 객체지향
  - 디자인패턴
  - JAVA
aliases:
  - 유한 상태 머신
  - FSM
  - State Machine
  - 유한 상태 오토마타
date: 2024-06-19
title: Finite State Machine
---
작성 날짜: 2024-06-19
작성 시간: 16:21


----
## 내용(Content)

### FSM

>[!summary]
>상태를 기반으로 동작하는 형태로 상태와 행동 간의 노드 그래프로 볼 수 있다. 크게 3가지 요소를 가지고 있다. `상태`, `행동`, `전이`

FSM이 특징은 다음과 같다.
- 유한 상태 기계는 자신이 취할 수 있는 유한한 갯수의 상태를 가짐.
- 특정 조건이 되면 다른 상태로 변할 수 있다.(전이 조건)
- FSM은 상태들의 집합과 각 상태들의 전이 조건으로 정의될 수 있다.
- 각각의 상태들은 노드와 그 노드를 연결하는 그래프로 표현 가능하다.

>[!tip]
> `특정 조건이 다른 상태로 변할 수 있다`는 것은 상태의 전이 조건을 의미한다. 이 때문에 상태들 간의 그래프가 형성이 가능하다.

>[!caution]
>상태에는 행동이 정의되어야 한다.

### 예시

FSM의 경우 게임 AI 또는 AI를 위한 시뮬레이션이나 Animator등에 많이 사용된다. 예를 들면 우리가 어떤 게임을 만들고 몬스터 AI를 설계해야 한다고 가정하자.

몬스터의 3가지 행동 패턴은 다음과 같다. 우리는 몬스터에게 다음과 같은 임무를 주려고 한다.
몬스터에 대기, 이동, 공격 3가지 Action을 수행하는 상태를 제공하며, 각각의 상태는 특정 조건에 따라 전이 될 수 있다. 

1. Idle
	- 아무런 행동없이 대기 상태
	- 플레이어가 인지 가능 범위 안에 있다면 Move로 상태 전환
	- 플레이어가 공격 가능 범위 안에 있다면 Attack 상태 전환
2. Move
	- 이동
	- 인지 가능 범위를 벗어나면 Idle 상태 전환
	- 공격 가능 범위 안에 있다면 Attack 상태 전환
3. Attack
	- 공격 상태
	- 인지 가능 범위를 벗어나면 Idle 상태 전환
	- 공격 범위를 벗어나면 인지 가능 범위에 있다면 Move로 전환

3가지 특징을 이용해서 그래프를 그려보자


![[몬스터 AI 상태 오토 마타(draw).svg]]

위 그림을 통해 상태들이 어떤 액션을 가지고 어떤 전이(Transition)를 가지는 지 쉽게 파악 가능하다. 위의 각 상태들은 2가지 전이 방식을 가진다.(밖으로 나가는 Edge) 그리고 Idle은 대기 상태, Move는 이동 상태, Attack은 공격 상태임을 쉽게 알 수 있다.

### 구현

구현 방법은 Switch, if-else 구문, State Pattern등으로 다양하다. 객체들 간의 상태가 간단하다면 Switch, if-else 구문으로 구현해도 상관없지만, 그림이 없이 상태와 전이들을 파악하는 것이 복잡하다면 State Pattern을 고려해볼만 하다. 여기선 2 방식 모두 보여주기로 한다.

#### switch, if-else

```java
import java.util.Random;  
  
public class Monster {  
    private static final int RECOGNITION_DISTANCE = 10;  
    private static final int ATTACK_DISTANCE = 2;  
    enum ActionState {  
       IDLE, MOVE, ATTACK  
    }  
    private ActionState actionState;  
    public void play() {  
       int distance = findPlayer();  
       switch (actionState) {  
          case IDLE -> {  
             System.out.println("IDLE");  
             if (distance < RECOGNITION_DISTANCE && distance > ATTACK_DISTANCE) {  
                actionState = ActionState.MOVE;  
             } else {  
                actionState = ActionState.ATTACK;  
             }  
          }  
          case MOVE -> {  
             System.out.println("MOVE");  
             if (distance < ATTACK_DISTANCE) {  
                actionState = ActionState.ATTACK;  
             } else {  
                actionState = ActionState.IDLE;  
             }  
          }  
          case ATTACK -> {  
             System.out.println("ATTACK");  
             if (distance > RECOGNITION_DISTANCE) {  
                actionState = ActionState.IDLE;  
             } else {  
                actionState = ActionState.MOVE;  
             }  
          }  
       }  
    }  
    private int findPlayer() {  
       Random r = new Random();  
       return r.nextInt(10) + 1;  
    }  
}
```


switch case문과 if-else를 사용하면 장점은 쉽게 구현이 가능하다는 점이다. 단점은 한 클래스에 너무 세부적인 구현 요소가 모두 들어가 있다는 것이다.

장점은 구체적인 부분을 생략하고 단점을 살펴보겠다.

Monster라는 클래스는 play를 통해 상태가 변하는 클래스이고, 이를 관리한다. 위 코드에서는 Monster가 상태를 관리하는 Context처럼 사용된다. 그러나 이런 코드의 문제는 간단한 경우에는 쉬울지 몰라도 상태 간의 관계가 복잡해지면 사용하기가 힘들다.

#### 상태 패턴

이번 글에서 상태 패턴에 대해서 자세히 다루지는 않지만 상태 패턴을 이용하면 상태 변환과 액션을 다른 객체에 위임해서 로직을 분리해낼 수 있다.

![[Pasted image 20240619181910.png]]

구현 방식은 위의 다이어그램을 참고해서 상태패턴을 구현할 것이다.

```java
public class Monster {  
  
    private ActionState actionState = new IdleActionState();  
  
    public void play() {  
       int distance = findPlayer();  
       actionState = actionState.next(distance);  
       actionState.action();  
    }  
  
    private int findPlayer() {  
       Random r = new Random();  
       return r.nextInt(10) + 1;  
    }  
}
```

Monster는 단순히 play를 실행하고 세부 로직은 사라지면서 간단해졌다.
action 및 전이는 ActionState라는 interface에게 위임한다.

```java
/**  
 * 행동 상태  
 * 1. action 정의  
 * 2. next(transition) 정의  
 */  
public interface ActionState {  
    void action();  
  
    ActionState next(int distance);  
}
```

그리고 각 ActionState를 편리하게 구현하기 위해 템플릿 패턴을 적용

```java
public abstract class AbstractActionState implements ActionState {  
  
    private static final int RECOGNITION_DISTANCE = 10;  
    private static final int ATTACK_DISTANCE = 2;  
    
    protected boolean canAttack(int distance) {  
       return distance <= ATTACK_DISTANCE;  
    }  
  
    protected boolean canRecognize(int distance) {  
       return distance <= RECOGNITION_DISTANCE;  
    }  
}
```

```java
public class IdleActionState extends AbstractActionState implements ActionState {  
  
    @Override  
    public void action() {  
       System.out.println("Idle");  
    }  
  
    @Override  
    public ActionState next(int distance) {  
       if (canAttack(distance)) {  
          return new AttackActionState();  
       }  
       if (canRecognize(distance)) {  
          return new MoveActionState();  
       }  
       return this;  
    }  
}
```

나머지 ActionState도 비슷한 방식으로 구현한다.



## 질문 & 확장

위 코드에서 아쉬운 점은 매번 transition 마다 본인 State 이외에 다른 ActionState 인스턴스를 생성하는 것이다. 만약 상태가 자주 바뀌고, 인스턴스의 생성 비용이 크다면, 이를 해결하기 위해 인스턴스를 미리 생성하고 관리하는 Context를 따로 생성해 둘 수도 있다. 좋은 방법은 싱글턴 패턴으로 인스턴스를 미리 생성해두는 방법인데 이를 위해 enum을 활용할 수 있다. 

## 출처(링크)

- https://welcomeheesuk.tistory.com/45
- https://seoksii.tistory.com/73
- https://daru-daru.tistory.com/70
## 연결 노트










