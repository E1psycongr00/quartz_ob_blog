---
tags:
  - 완성
  - 솔루션
  - NodeJS
  - JS
aliases:
  - node args 처리하기
  - 콘솔에서 node 인자 처리
  - js args 처리하기
date: 2024-04-04
title: node 인자 처리하기
---
작성 날짜: 2024-04-04
작성 시간: 13:12


----

## 문제 & 원인
node에서 스크립트를 실행할 때 콘솔로 인자를 받아서 처리해야 할 상황이 올 수 있다.

```js
function greeting(name) {
	console.log(`hello ${name}`);
}

 greeting(????);
```

인자를 어떻게 받아서 처리할 수 있을까?
## 해결 방안
### process.argv
#### 분석
>[!summary]
> js script의 인자를 받아서 처리하려면 process.argv.slice(2)를 사용하면 된다.

process.argv에는 console에서 실행할 때 노드 정보를 함께 전달한다. 예시를 살펴보자

```js
console.log(process.argv);
```

![[Pasted image 20240404132430.png]]

실행 결과를 살펴보면 0번 인덱스에는 node 실행 경로와 1번 인덱스에는 hello.js 실행 위치를 출력한다.

그러면 이번엔 인자를 넣어서 사용해보자

```shell
node hello.js arg1 20
```

![[Pasted image 20240404132726.png]]

![[Pasted image 20240404132755.png]]

node와 스크립트 실행 경로는 불필요하기 때문에 .slice(2)로 2번 인덱스부터 받도록 하면 된다.

>[!info] process
> node에는 [process](https://nodejs.org/api/process.html#process)라는 객체가 있다. Process 객체는 현재에 대한 정보 및 위치를 제공한다.

#### greeting 문제 해결하기
```js
function greeting(name) {
	console.log(`hello ${name}`);
}
const args = process.argv.slice(2).join(",");
const names = args.join(",");
greeting(names);
```


## 질문 & 확장

(없음)

## 출처(링크)
- [프로세스 | Node.js v21.7.2 설명서 (nodejs.org)](https://nodejs.org/api/process.html#process)

## 연결 노트
- [[node 복잡한 인자 처리하기]]