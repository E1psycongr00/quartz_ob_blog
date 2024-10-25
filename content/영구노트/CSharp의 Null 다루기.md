---
tags:
  - 완성
  - CSharp
aliases: 
date: 2024-10-02
title: CSharp의 Null 다루기
---
작성 날짜: 2024-10-02
작성 시간: 18:49


----
## 내용(Content)

### NRE

>[!summary]
>객체가 예상치 못하게 null을 반환하는 경우, null을 타입으로 착각해서 Runtime 중 예외가 발생한다. 이 에러를 Null Reference Exception이라 한다.

NRE는 JAVA의 NPE와 같다고 이해하면 된다. JAVA에서는 Reference 객체는 Object로 모두 타입이 Null을 가질 수 있다. 이 때문에 Null 체크 코드가 필요하며 다음과 같다.


```csharp
class Main {

	public static void main() {
		String nullString = null;
		nullString.Equals("hello");
	}
}
```

이 코드는 정적 분석에서는 아무런 문제가 없다. C# 8.0에서는 Nullable 참조 형식 사용을 명시적으로 하면서 정적 분석을 어느 정도 수행한다.

이 코드가 vscode 정적 분석에서 어떻게 보이는지 보자

![[Pasted image 20241002201219.png]]

### Nullable 타입

>[!summary]
>타입이 null 될 수 있음을 명시적으로 표현하는 타입이다. 타입 끝에 ? 마크를 붙이면 된다. 
>

Python에선 이와 비슷하게 union 타입으로 str | None 처럼 타입이 null 될 수 있음을 표현한다. C# 에서도 비슷한 느낌으로 사용된다. 예를 들어

```csharp
String? s; // s는 String 타입 이거나 Null 타입이 될 수 있다.
```

실제로 사용해보자.
![[Pasted image 20241002201558.png]]
nullString에서 발생하는 경고가 사라졌음을 알 수 있다. nullString의 타입을 String 또는 null이 될 수 있는 타입이라고 명시적으로 선언했기 때문이다.

### Null 연산자

>[!summary]
>null 타입인 경우 타입 체킹을 통해 null을 반환한다. 메서드 호출 전에 ? 마크를 붙이면 된다. ??인 경우 null 타입이라면 사용자 정의 값을 반환 가능하다.

![[Pasted image 20241002201805.png]]

이 코드를 살펴보자. nullString 변수가 null reference의 가능성이 있다고 경고하고 있다. 만약 null을 참조하고 있다면, 이 코드는 반드시 NRE를 발생시킨다. Null에서 .Equals 메서드를 호출하고 있기 때문이다.

![[Pasted image 20241002202223.png]]
메서드 앞에 null을 붙여주면 이제 다른 오류가 뜬다. ?의 역할은 null 체크 후에 null이면 메서드 호출이 아닌 null을 반환한다. 그러나 result 변수의 타입은 bool로 null를 리턴이 가능하기 때문에 다음과 같이 nullable 타입을 써주면 해결 가능하다.

![[Pasted image 20241002202320.png]]
문제는 해결됬지만 null 대신 bool 타입을 확실하게 받고 싶은 경우에는 ?? 연산자를 활용할 수 있다.

![[Pasted image 20241002202413.png]]

bool result 라인의 코드를 해석하면 다음과 같다.
1. nullString.equals를 호출하기 전에 null-check를 하고 null인 경우에는 null을 반환하고, null 이 아닌 경우에는 Equals를 호출한다.
2. 만약 null을 반환한 경우 `??` 뒤에 값으로 초기화한다.

이렇게 null에 대해서 안전한 코드를 if 문 없이 그리고 명시적인 Nullable 타입을 통해 쉽게 작성하고, Null에 대한 코드 가독성 또한 챙겨 갈 수 있다.
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










