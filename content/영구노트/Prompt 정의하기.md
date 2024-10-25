---
tags:
  - 완성
  - LLM
  - Langchain
aliases: 
date: 2024-09-24
title: Prompt 정의하기
---
작성 날짜: 2024-09-24
작성 시간: 16:03


----
## 내용(Content)

### LLM 모델 종류

우선 Prompt를 정의하기 전에 LLM 모델을 이해할 필요가 있다.

LLM의 생성 스타일은 2가지로 나눠지는데 `Chat` 모델과 `Completion` 모델이다.


>[!info] info1: Completion
>- Completion 스타일은 중간에 글을 써놓고 추가적으로 글을 완성할 때 쓰는 모델이다. 자동 완성 기능이 여기에 속한다.
>

>[!info] info2: Chat
>- Chat 스타일은 ChatGPT, Claude, Gemini등 대화형을 지원하는 모델이다. Completion도 물론 지원한다.

### Prompt 정의하기

언어 모델이 동작하기 이전에 사전에 Role을 정해서 LLM이 좀 더 나은 응답을 위해 설정하는 것이 Prompt이다. 보통 Prompt 모델은 Chat모델에서 정의하며 Langchain에서는 다음과 같이 3개의 역할을 정의함으로써 prompt를 정의한다.

**System:**
System은 AI 모델에 대한 전반적인 지시 사항이나 컨텍스트를 제공한다. 이는 AI의 행동 방식, 대화의 목적, 또는 특정 규칙을 설정하는 데 사용된다. System 프롬프트는 대화의 전체적인 틀을 잡아주는 역할을 한다.

**Human**:  
Human은 사용자의 입력이나 질문을 나타낸다. 이는 AI 모델이 응답해야 할 실제 쿼리나 명령을 포함한다. Human 프롬프트는 대화에서 사용자의 역할을 시뮬레이션한다. 예를 들면 "{input}" 이렇게 텍스트를 만들면 input을 동적으로 prompt를 구성해서 적절한 대답을 LLM에게 제공해줄 수 있다.

**AI**:  
AI는 모델이 생성해야 할 응답의 시작을 나타낸다. 보통은 응답의 형식이나 스타일을 지정하는 데 활용된다.

### PromptTemplate

>[!summary]
> 변수를 넣어서 동적인 템플릿을 간단하게 생성하는 클래스로 from_template을 통해 템플릿 형식을 만들고 .format을 통해 prompt를 완성한다.

```python
from langchain.prompts import PromptTemplate

template = "{task}를 수행하는 로직을 {language}으로 작성해줘"

prompt_template = PromptTemplate.from_template(template)
print(prompt_template)
prompt = prompt_template.format(task="0부터 10까지 계산", language="파이썬")
print(prompt)
```


```
# prompt_template 출력값
input_variables=['language', 'task'] template='{task}을 수행하는 로직을 {language}으로 작성해 줘~'

# prompt 출력값
0부터 10까지 계산을 수행하는 로직을 파이썬으로 작성해 줘~
```

위 예제에서는 task, language 2개의 변수를 지정했고, format을 통해 변수 값을 할당한다.

json파일을 .load_prompt 메서드를 통해 template형식을 불러올 수도 있다.


### ChatPromptTemplate

ChatPromptTemplate은 대화형으로 응답하는 LLM에 효과적으로 사용 가능하다.

```python
model = ChatOpenAI()
prompt = ChatPromptTemplate.from_messages(
    [
        (
            "system",
            "당신은 {ability}에 능숙한 어시스턴트입니다. 20자 내외로 응답해주세요"
        ),
        # 대화 기록을 변수로 사용, history가 messageHistory의 key가 된다
        MessagesPlaceholder(variable_name="history"),
        ("human", "{input}"),
    ]
)

chain = prompt | model
```

이 코드를 보면 system 프롬프트에 ability 변수를 주었고, human 프롬프트에 input 변수를 주었다.

여기서 바로 .format_messages를 통해 prompt를 완성시켜줄 수도 있고, invoke를 통해서 요청시에 변수를 할당할 수도 있다.

```python
prompt = prompt.format_messages(ability="math", input="What is Cosine")
```

invoke를 통해 chain에 요청 시에 변수를 할당하는 방법

```python
chain.invoke({
			  "ability": "math",
			  "input": "What is Cosine"
})
```

### Langchain Hub

Langchain Hub를 통해서 쉽게 prompt를 정의할 수 있다.

https://smith.langchain.com/hub

```shell
pip install langchainhub
```

이걸 입력 후에

```python
prompt = hub.pull("prompt name", api_url="")
```

이걸 이용하면 빠르고 쉽게 prompt를 정의할 수 있게 된다.

## 질문 & 확장

(없음)

## 출처(링크)

- https://rudaks.tistory.com/entry/langchain-Prompt-Template-%EC%82%AC%EC%9A%A9%EB%B2%95
- https://www.pinecone.io/learn/series/langchain/langchain-prompt-templates/
## 연결 노트


