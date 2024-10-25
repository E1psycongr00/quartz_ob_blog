---
tags:
  - 완성
  - JS
aliases:
title: shorthand property names
date: 2024-02-08
---
작성 날짜: 2024-02-08
작성 시간: 20:49


----
## 내용(Content)
### Shorthand Property names
>[!summary] Shorthand Property names
>객체의 키를 정의할 때 key 네이밍과 value에 쓸 변수 네이밍이 같을 때 축약해서 사용하는 기법이다.

요약한 말이 어려운데 예시를 보자

```js
const name = "John";
const age = 25;

const Person = {
	name: name,
	age: age
}
```

위 코드를 보면 Person name과 age에 John와 age를 할당하기 위해서 name 변수와 age 변수를 쓰고 있다. 그리고 코드를 보면 : 기준으로 좌 우 네이밍이 같음을 볼 수 있다.

이런 경우 축약해서 다음과 같이 표현 가능하다.

```js
const name = "John";
const age = 25;

const Person = {
	name,
	age
}
```

어떤 객체에 특정 상태를 할당하는 경우에는 유용하게 쓰일 수 있다. 그러나 개인적인 생각에는 확장성이 떨어지고 문법을 모르면 이해를 못할 수도 있을 것 같아, 팀원들과 잘 협의해서 사용해야 할 부분인 것 같다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://bamtory29.tistory.com/entry/Javascript-shorthand-property-names

## 연결 노트
- [[Computed property name]]









