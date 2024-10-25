---
tags:
  - 완성
  - JS
aliases:
  - 배열은 객체다
  - 유사배열
title: 배열은 객체다(JS)
date: 2024-02-08
---
작성 날짜: 2024-02-08
작성 시간: 20:18


----
## 내용(Content)
### JS에서 배열은 객체다
>[!summary] 배열 == 객체
>[!list like object by MDN](https://developer.mozilla.org/ko/docs/Learn/JavaScript/First_steps/Arrays#%EB%B0%B0%EC%97%B4%EC%9D%B4%EB%9E%80)
>리스트 형태로 다수의 값들은 포함한 객체라는 의미

```js
let x = [1,2,3];
```
### 유사배열
>[!summary] 유사배열
>유사 배열은 배열처럼 인식되는 객체이나 배열 매서드를 사용할 수 없음
>


```js
let x = {
	0: 1,
	1: 2,
	length: 2
}
```

해당 배열에서 배열 메서드를 사용하고 싶으면 배열로 변환시켜주면 된다.

```js
Array.from(x);
```

배열인지 확인하고 싶다면
```js
Array.isArray(x);
```

>[!caution] Array.from 주의점
>- 유사배열 형식을 갖춰야한다.(0, 1, ... ,length)
>- length가 적게 쓴경우 끝 인덱스부터 짤리고, length가 key 수보다 많은 경우 undefined가 배열에 들어간다.


## 질문 & 확장

(없음)

## 출처(링크)
- https://pozafly.github.io/javascript/array-is-object/
- https://developer.mozilla.org/ko/docs/Learn/JavaScript/First_steps/Arrays#%EB%B0%B0%EC%97%B4%EC%9D%B4%EB%9E%80
## 연결 노트










