---
tags:
  - 완성
  - JS
aliases: 
title: Lookup Table
date: 2024-02-08
---
작성 날짜: 2024-02-08
작성 시간: 21:03


----
## 내용(Content)
### Lookup Table
>[!summary] Lookup Table
>특정 입력 값이 여러 분기에 따라 어떤 처리를 하도록 할 때 객체를 활용해 심플하게 사용하는 기법

```js
function getType(type) {
    if (type === "ADMIN") {
        return "관리자";
    } else if (type === "USER") {
        return "사용자";
    } else if (type === "GUEST") {
        return "손님";
    } else if (type === "MANAGER") {
        return "매니저";
    } else {
        return "알수없음";
    }
}
```

이 코드의 문제점은 요구사항이 많아질수록 매번 else if 를 추가해야 한다는 점이다. 코드가 복잡하고 읽기도 쉽지 않은 단점이 있다. 요즘 클린 코드의 트렌드는 else if나 else를 사용하지 않는 것이다. 이를 조금 더 개선해보겠다.

```js
function getType(type) {
    switch (type) {
        case "ADMIN":
            return "관리자";
        case "USER":
            return "사용자";
        case "GUEST":
            return "손님";
        case "MANAGER":
            return "매니저";
        default:
            return "알수없음";
    }
}
```

switch case문으로 조금 더 개선해 보았다. 매우 깔끔해 졌음을 알 수 있지만 다른 방법으로도 개선할 수 있다.


```js
function getType(type) {
    const types = {
        ADMIN: "관리자",
        USER: "사용자",
        GUSET: "손님",
        MANAGER: "매니저",
    };
    return types[type] || "알수없음";
}
```

내부에 객체를 선언하고 객체에 값이 없다면 단축 평가를 활용해서 default 값을 호출한다. 이 방법은 분기가 많아져도 관리가 편하고 숏코딩도 가능하므로 매우 효과적이다. JS 스럽다고도 할 수 있다. 여기서 types를 외부로 빼서 관리할 수 있다. 그러면 최소한의 수정으로 여러 요구사항을 쉽게 충족할 수 있다.
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
- [[단축 평가 계산|short circuit evaluation]]









