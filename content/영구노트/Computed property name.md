---
tags:
  - 완성
  - JS
aliases:
title: Computed property name
date: 2024-02-08
---
작성 날짜: 2024-02-08
작성 시간: 21:01


----
## 내용(Content)
### Computed property name
>[!summary] Computed property name
>동적으로 객체의 key 할당하는 기법( 변수의 value를 key값으로 활용할 수 있음)

```js
let soldier = 76;

const person = {
    [`soldier${soldier}`] : "Jack Morrison",
}

console.log(person);
```

이 결과를 실행하면

![[Pasted image 20240208210809.png]]

로 key값에 76 으로 soldier 변수의 값이 key값에 들어갔음을 알 수 있다. 위처럼 리터럴로 사용도 가능하고 그냥 변수를 넣는 경우 변수의 값이 key가 된다. 동적으로 key 값 할당할 수 있기 때문에 특수한 상황에 매우 유용하게 사용할 수 있다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://velog.io/@yujuck/object-key%EC%97%90-%EB%B3%80%EC%88%98%EB%A5%BC-%EB%84%A3%EC%9C%BC%EB%A0%A4%EB%A9%B4-Computed-Property-Name

## 연결 노트










