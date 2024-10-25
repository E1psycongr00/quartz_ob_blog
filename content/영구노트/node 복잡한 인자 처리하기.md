---
tags:
  - 완성
  - 솔루션
  - NodeJS
  - JS
aliases: 
date: 2024-04-04
title: node 복잡한 인자 처리하기
---
작성 날짜: 2024-04-04
작성 시간: 13:43


----

## 문제 & 원인
이전에 [[node 인자 처리하기]]에서 인자를 처리할 수 있지만 복잡한 인자처리는 문제가 있다.

예를 들면 linux command shell에서 docker 같은 것을 사용해보면 이런 식으로 인자를 받고 싶을 수도 있다.

```shell
node hello.js --name hyeonja --name minsu
```

이것을 출력하면 

```text
[ '--name', 'h', '--name', 'l' ]
```

복잡한 인자의 경우 어떻게 처리해줄 수 있을까?
## 해결 방안
### Minimist
>[!summary]
>process.argv 의 args list 를 파싱해서 key, value 형태의 object로 인자로 받을 수 있도록 처리해주는 라이브러리

인자를 key,value 형태로 받아서 처리하려면 이 정보를 받아서 가공해야 한다. 이런 귀찮은 작업을 nodejs에서는 npm 라이브러리에 minimist를 통해 쉽게 해결할 수 있다.

```js
import minimist from "minimist";

console.log(minimist(process.argv.slice(2)));
```

```text
{ _: [], name: [ 'h', 'l' ] }
```

`_`는 어떤 key도 입력하지 않은 인자고 name 인자가 중복되서 입력이 들어갔기 때문에 리스트 형태로 보관된다. 단일 입력이면 문자열도 반환된다.

```text
{ _: [], name: 'h' }
```

### Minimist Option을 활용해 -와 -- 같도록 맵핑

```js
import minimist from "minimist";

console.log(
	minimist(process.argv.slice(2), { alias: { name: "n", age: "a" } })
);

```

alias 옵션을 활용하면 같은 인자가 되도록 만들 수 있다.

```text
{ _: [], name: [ 'h', 'l' ], n: [ 'h', 'l' ] }
```
## 질문 & 확장


(없음)

## 출처(링크)
- [미니미스트 - npm (npmjs.com)](https://www.npmjs.com/package/minimist)

## 연결 노트
- [[node 인자 처리하기]]