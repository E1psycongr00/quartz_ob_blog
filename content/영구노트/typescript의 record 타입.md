---
tags:
  - 완성
  - Typescript
aliases: 
date: 2024-04-03
title: typescript의 record 타입
---
작성 날짜: 2024-04-02
작성 시간: 23:31


----
## 내용(Content)

### Record

>[!summary]
>키가 key 타입이고, 값이 Value인 객체 타입

#### 공통점

>[!summary]
>index signature와 비슷한 역할을 수행한다. key, value 타입을 정의하는 object 타입이다.

```ts
// 인덱스 시그니처
type Score = {
	[name: string]: number
}

//  Record
type ScoreRecord = Record<string, number>;

let scoreRecord = {
	'age': 10,
	'salary': 1000
}
```

#### 차이점

>[!summary]
> index signature는 문자열 리터럴 타입을 지정할 수 없지만 Record는 가능하다.

```ts

// Record
type Score = Record<'치즈볼' | '초코볼', number>

// index signature
// 인덱스 시그니처는 문자열 리터럴 타입을 직접 지정 불가능하다.
const Names = '치즈볼' | '초코볼';
type Score = {
	[name in Names]: number;
}
```

## 질문 & 확장

(없음)

## 출처(링크)
- https://cheeseb.github.io/typescript/typescript-record/

## 연결 노트










