---
tags:
  - 완성
  - JS
aliases: 
title: Array.from
date: 2024-02-20
---


작성 날짜: 2024-02-20
작성 시간: 20:50


----
## 내용(Content)
### Array.from
>[!summary]
>Array.from은 유사배열객체나, 혹은 arguments와 같은 객체들을 배열로 만들 때 사용한다.
>

인자로 배열이 될 공간과 내부 배열을 어떻게 채울지 mapping 함수가 들어간다.

경우에 따라 다음과 같이 사용한다.

- mapping
- 초기화
- 순차적인 데이터 생성 ex) \[1,2,3,4,5,6,7,8,9,10]
- 외부 객체 메서드 콜백(bind)


>[!tip] 유사배열 객체
>[[배열은 객체다(JS)|배열은 객체다]]
>객체이지만 `{0:1, 1:2, 2:3, length:3}`과 같이 이루어져 있다. 차이점은 배열과 동일하게 활용되나 배열로 인식되진 않는다. 배열 메서드를 제공하지 않는다.


#### mapping

```js
Array.from({0:1, 1:2, 3:10, length: 4}, v => v*2); //  [ 2, 4, NaN, 20 ]
```


mapping 용도로는 잘 사용하지 않는다. 배열 객체의 경우, 중간에 정의하지 않은 경우에는 NaN과 같은 데이터가 나오기 떄문이다. 

#### 초기화

```js
Array.from({ length: 10 }).fill(0);
```

이렇게 배열을 0으로 초기화할 수 있따.

#### 순차적인 데이터 생성
```js
Array.from({length:3}, (_, i) => i); // [0, 1, 2]
```

mappingFunction은 value와 index를 인자로 가지는 mappingFunction을 인자로 받아서 작업할 수 있다. 위 표현은 java의 Intstream.range(0, 3).toArray(); 와 비슷하다.

#### 외부 메서드 콜백

외부에서 메서드를 가져올 때 this는 가져오지 못한다. 그래서 bind를 사용해야 하는데 이것을 인자 3개를 통해 간편하게 받을 수 있다.

```js
const Utils = {
    val: 10,
  
    add(x) {
        return x + this.val;
    },
};

// == Array.from([1,2,3], Utils.add.bind(Utils));
const result = Array.from([1,2,3], Utils.add, Utils); 

```
## 질문 & 확장

(없음)

## 출처(링크)
- https://5takoo.tistory.com/220

## 연결 노트
- [[배열은 객체다(JS)|배열은 객체다]]








