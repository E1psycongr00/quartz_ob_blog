---
tags:
  - 완성
  - Python
  - Langchain
  - LangGraph
aliases:
  - State of LangGraph
  - State
date: 2024-10-10
title: LangGraph의 State란
---
작성 날짜: 2024-10-10
작성 시간: 16:09


----
## 내용(Content)

### State

>[!summary]
>어플리케이션의 현재 스냅샷을 나타내는 공유 데이터 구조.

일반적으로 LangGraph는 StateGraph 기반으로 동작한다.

State는 일반적으로 TypedDict로 구성되며, Validation으로 상태를 엄격하게 구성하고 싶다면, Pydantic으로 정의하기도 한다.

### 다중 스키마

보통 State는 단일 스키마로 사용하지만, 경우에 따라 세밀하게 제어하고 싶은 경우 다중 스키마를 사용할 수 있다.

```python
class InputState(TypedDict):
    user_input: str

class OutputState(TypedDict):
    graph_output: str

class OverallState(TypedDict):
    foo: str
    user_input: str
    graph_output: str

class PrivateState(TypedDict):
    bar: str

def node_1(state: InputState) -> OverallState:
    # Write to OverallState
    return {"foo": state["user_input"] + " name"}

def node_2(state: OverallState) -> PrivateState:
    # Read from OverallState, write to PrivateState
    return {"bar": state["foo"] + " is"}

def node_3(state: PrivateState) -> OutputState:
    # Read from PrivateState, write to OutputState
    return {"graph_output": state["bar"] + " Lance"}

builder = StateGraph(OverallState,input=InputState,output=OutputState)
builder.add_node("node_1", node_1)
builder.add_node("node_2", node_2)
builder.add_node("node_3", node_3)
builder.add_edge(START, "node_1")
builder.add_edge("node_1", "node_2")
builder.add_edge("node_2", "node_3")
builder.add_edge("node_3", END)

graph = builder.compile()
graph.invoke({"user_input":"My"})
{'graph_output': 'My name is Lance'}
```

상당히 복잡한 예시기 때문에 따로 공부하는 경우 정리하고, 지금은 단일 스키마로도 충분하는 것만 알아두자.
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










