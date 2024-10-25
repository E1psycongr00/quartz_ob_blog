---
tags:
  - 완성
  - Python
  - Langchain
  - LCEL
aliases: 
date: 2024-10-01
title: "@chain과 Runnable"
---
작성 날짜: 2024-10-01
작성 시간: 15:08


----
## 내용(Content)

### @Chain 데코레이터

>[!summary]
>사용자 정의 함수를 @Chain을 이용하여 RunnableLambda 객체로 래핑해서 쉽게 chain을 만들 수 있게 해준다.

@Chain 데코레이터는 기능적으로는 RunnableLambda 객체로 만들어 편하게 사용할 수 있게 하기 위해 사용한다. @Chain을 사용하면 PipeLine에 커스텀 함수로 길게 RunnableLambda로 래핑할 필요 없이 바로 사용 가능해진다.


### 예제

```python
from langchain.prompts import ChatPromptTemplate


prompt1= ChatPromptTemplate.from_template(
    "{topic}에 대해 짧게 한글로 설명해주세요" 
)

prompt2 = ChatPromptTemplate.from_template(
    "{sentence}를 emoji를 활용한 인스타그램 게시글로 만들어주세요."
)
```

프롬프트를 정의해준다.

```python
from langchain.schema import StrOutputParser
from langchain_core.runnables import chain
from langchain_openai import ChatOpenAI


@chain
def custom_chain(text):
    chain1 = prompt1 | ChatOpenAI(model="gpt-3.5-turbo") | StrOutputParser()
    output1 = chain1.invoke({"topic": text})

    chain2 = prompt2 | ChatOpenAI(model="gpt-3.5-turbo") | StrOutputParser()
    return chain2.invoke({"sentence": output1})


```

@chain을 이용해 입력과 응답이 Runnable하게 나오도록 사용자 정의 함수를 랩핑해 줄 수 있다.

```python
	print(custom_chain.invoke("양자역학"))
```

```text
🔬✨ 양자역학으로 알 수 없는 세계를 탐험해보세요! 입자들의 물결과 확률의 세계에서 빛과 원자가 어떻게 상호작용하는지 알아보는 재미있는 여행 🌌🌀 #양자역학 #물리이론 #빛과입자들 #물결의세계 #이해하기어렵지만흥미진진 🌟
```

위와 같이 결과가 나옴을 알 수 있다.

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트

