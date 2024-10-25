---
tags:
  - 완성
  - Python
  - Langchain
  - LangGraph
aliases: []
date: 2024-10-10
title: LangGraph의 Graph 구성 요소
---
작성 날짜: 2024-10-10
작성 시간: 15:57


----
## 내용(Content)

### Graph

>[!summary]
> LangGraph는 , `State`, `Node`, `Edge` 3개의 핵심 요소를 이용해 Agent workflow를 그래프로 구성한다.

#### State

>[!summary]
>어플리케이션의 현재 스냅샷을 나타내는 공유 데이터 구조.

계산을 수행하고 결과를 스냅샷으로 찍어서 공유한다. 
State는 기본적으로 `TypedDict` (python 기본 dict) 또는 `Pydandic`(Validation을 적용 가능한 엄격한 python dict 관련 라이브러리) 으로 정의해서 사용한다.
State는 매번 그래프를 거치면서 변화할 수 있고, 나머지 key,value는 유지되고 변화하는 key의 value만 수정한다.

자세한 State에 대한 내용은 [[LangGraph의 State란]] 을 참고하자.

#### Node

>[!summary]
>노드는 현재 State를 입력으로 받고, 계산 이후 State를 반환하는 논리

Node는 State 를 입출력으로 받는 function을 그래프 노드로 구성한 것이다.

node는 함수로 로직을 정의하고 추가하면 다음과 같은 형태를 가져야한다.

```python
from langchain_core.runnables import RunnableConfig
from langgraph.graph import StateGraph

builder = StateGraph(dict)


def my_node(state: dict, config: RunnableConfig):
    print("In node: ", config["configurable"]["user_id"])
    return {"results": f"Hello, {state['input']}!"}


# The second argument is optional
def my_other_node(state: dict):
    return state


builder.add_node("my_node", my_node)
builder.add_node("other_node", my_other_node)
```

첫번째 인자는 입력받을 state로 받고 결과는 state를 반환한다. 두번째 인자는 RunnableConfig로 사용해도 되고, 안 해도 된다.

Node에는 시작과 끝을 알리는 노드로 START, END가 존재한다.

#### edge

>[!summary]
> 현재 Node에서 어떤 Node를 실행할지 결정하는 Component이다.

edge는 상황에 따라 기본 edge 또는 분기에 따라 처리되는 conditional_edge 등 존재한다.

edge 종류
- Normal Edge:  A -> B 로 정의되는 edge
- Conditional Edge: 어느 노드로 이동할지 결정하는 router 함수 호출
- Entry Point: 사용자 입력이 도착하면, 가장 먼저 호출할 노드
- Conditional Entry Point: 사용자 입력이 도착했을 때 호출할 노드를 결정할 router 함수 호출
>[!caution]
>한 노드에서 여러 바깥으로 뻗어나가는 엣지를 사용가능하다.(multi- outgoing edge). 이 경우 병렬로 동시에 실행이 된다.

##### Normal Edge

```python
graph.add_edge("node_a", "node_b")
```

##### Conditional Edge

```python
graph.add_conditional_edges("node_a", routing_function)
```

라우팅 function의 결과에 따라 다음 노드의 이름을 매핑을 제공할 수 있다.

```python
graph.add_conditional_edges("node_a", routing_function, {True: "node_b", False: "node_c"})
```

##### set_entry_point

내부적으로 다음과 같은 코드이다.

```python
graph.add_edge(START, "node")
```

좀 더 명확한 코드 의미 전달을 위해 해당 메서드가 존재한다고 한다.
## 질문 & 확장

(없음)

## 출처(링크)

- https://langchain-ai.github.io/langgraph/concepts/low_level/

## 연결 노트

- [[LangGraph의 State란|State]]








