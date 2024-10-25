---
tags:
  - 완성
  - 클린코드
  - JS
aliases:
  - 배열 불변 복사
title: 배열 불변성 복사(JS)
date: 2024-02-08
---
작성 날짜: 2024-02-08
작성 시간: 10:21


----
## 내용(Content)
### 불변성이 필요한 이유
![[Pasted image 20240208102207.png]]

위 그림을 보면 originArray는 1234인데 123456으로 copyArray가 영향을 받는다. 너무 당연한 이야기지만 copyArray는 originArray 객체 자체를 복사했기 떄문에 단순히 객체 주소를 복사해온 것이다. 이를 해결하려면 새로운 객체에 복사하면 된다.


```js
Array.from(originalArray)
[...originalArray]
[].concat(originalArray)
originalArray.slice();
```
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










