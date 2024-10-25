---
tags:
  - 완성
  - JS
aliases: 
title: 함수와 this
date: 2024-02-09
---
작성 날짜: 2024-02-09
작성 시간: 14:07


----
## 내용(Content)
### 함수

>[!summary] 함수
>JS의 함수는 [[일급 객체]] 이다.

JS는 일급 객체이기 떄문에 일급 함수라 하며 일급 객체에 특징을 가진다.

1. 함수는 변수나 데이터로 할당 될 수 있다.
2. 함수를 매개 변수로 전달 가능하다.(콜백 함수)
3. 함수가 함수를 반환 가능하다.(고차 함수, 클로저)
### 함수의 호출 패턴
>[!summary] 함수 호출 패턴
>- 일반 함수 실행
>- 함수가 object의 method일 때 실행
>- bind method 호출했을 때
>- 함수가 생성자 함수로 사용될 때


### 일반 함수 호출(Single Function Invocation)
>[!summary] 일반 함수 호출
>함수를 ()를 이용해 일반적으로 호출하는 경우 this는 global(브라우저에서는 window) 객체로 동작, strict mode에서는 undefined로 동작한다.

```js
function hello(name) {
    return `${this}: Hello, ${name}!`;
}

let result = hello("world");
console.log(result);
```

**node 에서 결과**
![[Pasted image 20240209144443.png]]

**브라우저에서 결과**
![[Pasted image 20240209144206.png]]


>[!note] Function.prototype.call(global, argument)
>위에 ()를 호출하는 동작은 Function.prototype.call을 호출한 것이다. 이 때 global을 임의의 값으로 수정하면 특정 객체를 this로 활용할 수 있다.

>[!caution] window is not defined
>window 객체는 브라우저 환경에서 정의된 최상위(전역) 객체이기 떄문에 node에서 실행되는 경우 not defined를 리턴할 수 있다.
### 메서드
>[!summary] 메서드
>객체에 의존성이 있는 함수이다. OOP의 핵심이다. Function call의 경우 this가 메소드를 포함하는 Object이다.

```js
const obj = {
    name: "John",
    age: 30,
    hello(word) {
        console.log(`${this} says hello ${word}!`);
    }
}
  
obj.hello("world");
```

![[Pasted image 20240209145334.png]]


### Bind 메소드
>[!summary] Function.prototype.bind(obj)
>콜백 함수와 같이 가져와서 함수를 결합하는 경우 this가 소멸되는데 this를 온전히 가져오고 싶을 때 사용하는 함수

**소멸되는 this**
```js
const person = {
    name: "Jack",
    hello(thing) {
        console.log(`${this.name} says hello ${thing}`);
    },
};

const boundedHello = person.hello;
boundedHello("world");
```
![[Pasted image 20240209152724.png]]

boundedHello를 person.Hello 메서드로 할당했다. 그러나 이렇게 할당하면 완전히 결속되지 못하고 this는 undefined가 된다.

이를 해결하기 위해 function의 this 정보를 가져오기 위해서  bind를 사용한다.

```js
const person = {
    name: "Jack",
    hello(thing) {
        console.log(`${this.name} says hello ${thing}`);
    },
};

const boundedHello = person.hello.bind(person);
boundedHello("world");
```

![[Pasted image 20240209152924.png]]

### 생성자 함수
>[!summary] 생성자 함수
>생성자 함수에서는 new 키워드를 이용해 함수를 호출하면 인스턴스가 생성되고 생성된 인스턴스가 this를 가르킨다.

```js
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.hello = function () {
        console.log(
            `Hello, my name is ${this.name} and I am ${this.age} years old.`
        );
    };
}
  
const person1 = new Person("John", 20);
person1.hello();
```

![[Pasted image 20240209153737.png]]
## 질문 & 확장

(없음)

## 출처(링크)
- https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B8%89_%EA%B0%9D%EC%B2%B4
- https://velog.io/@reveloper-1311/%EC%9D%BC%EA%B8%89-%EA%B0%9D%EC%B2%B4First-Class-Object%EB%9E%80
- https://velog.io/@imacoolgirlyo/JS-JavaScript-Function-Invocation%EC%99%80-this#-functionprototypecallthis-arglist%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%ED%95%A8%EC%88%98-%ED%98%B8%EC%B6%9C
- https://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/
## 연결 노트










