---
tags:
  - 완성
  - Redux
aliases: 
date: 2024-03-06
title: Redux
---
작성 날짜: 2024-03-06
작성 시간: 19:12


----
## 내용(Content)
### Redux
>[!summary]
>Redux란 "액션"이란 이벤트를 사용하여 애플리케이션의 상태를 전역적으로 관리하고 업데이트하기 위한 라이브러리다.

Redux는 상태를 중앙 집중적인 상태(전역 상태)로 애플리케이션을 관리한다. 상태를 무분별하게 전역으로 관리하는 것은 좋지 않을 수도 있다. 그래서 공식 홈페이지도 Redux를 무분별하게 사용하는 것은 추천하지 않으며 Redux가 꼭 필요한 상황에 활용하라고 한다.

>[!tip] Redux가 좋아하는 상황
>1. **앱의 여러 위치에서 상태가 사용된다**
>2. 앱 상태가 자주 업데이트된다.
>3. **상태에 대한 업데이트 논리가 복잡하다.**
>4. 앱에 중간 또는 대규모 코드베이스가 있으며 많은 사람들이 작업하는 상황
> 
> 1번과 3번이 핵심이라 생각한다. 간단한 상태에 대한 로직인 경우 컴포넌트 내에서 처리하나 props로 받으면 되지만 여러 위치에서 참조하거나 업데이트하는 경우 상태 관리가 쉽지 않고, 자료에 대한 업데이트 구조가 복잡해서 상태를 별도로 관리하기 위한 util 모듈이 필요한가? 의문을 가질 때 이를 redux를 이용하면 쉽게 해결할 수 있기 떄문이다. (컴포넌트로부터 상태 로직도 분리되기 때문에 코드 가독성이 올라갈 수도 있다?)

### Redux 동작 원리
![[Pasted image 20240306195042.png]]

리덕스는 위와 같은 데이터 흐름을 가진다. 위의 3 요소는 리액트와 함께 리덕스를 사용하는 경우 다음과 같은 곳에서 활용된다.

- View => Component
- Actions => Reducer
- State => Store

위 그림은 Redux를 왜 사용하는 건지 모든 것을 담은 그림이라 할 수 있다.

View의 UI는 상태 기반에 따라 렌더링된다. 그리고 어떤 이벤트가 발생하면 Actions를 호출하게 되고 전역으로 상태를 관리하는 Store에서 State의 변경이 일어난다. View는 해당 변경된 State를 가져오고 리렌더링한다.

![[ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif]]

지금까지 설명한 것을 잘 표현한 그림
## 질문 & 확장

(없음)

## 출처(링크)
- https://changelog.com/posts/when-and-when-not-to-reach-for-redux
- https://ko.redux.js.org/tutorials/essentials/part-1-overview-concepts
- https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367
## 연결 노트









