---
tags:
  - 완성
  - AI
  - LLM
  - Langchain
aliases: 
date: 2024-09-24
title: StrOutputParser
---
작성 날짜: 2024-09-24
작성 시간: 11:23


----
## 내용(Content)

### StrOutputParser

>[!summary]
>StrOutputParser의 주된 역할은 LLM이나 ChatModel에서 출력된 결과를 문자열 형식으로 변환하여 리턴한다. 체인 마지막에 정의한다.

- LLM의 경우 문자열로 리턴하면 그대로 출력한다.
- ChatModel의 경우 .content 속성을 추출하여 리턴한다.

### 예제

```python
model = ChatOpenAI("gpt-3.5-turbo")
prompt = PromptTemplate(template="{query}")
chain = prompt | model

response = chain.invoke({"query": "한국의 수도는?"})
print(response)
```

이에 대한 결과는 여러 메타데이터와 함께 invoke의 응답이 출력된다.

![[Pasted image 20240924112907.png]]

여기에 StrOutputParser를 넣으면 다음과 같다.

```python
model = ChatOpenAI("gpt-3.5-turbo")
prompt = PromptTemplate(template="{query}")
chain = prompt | model | StrOutputParser()

response = chain.invoke({"query": "한국의 수도는?"})
print(response)
```

![[Pasted image 20240924113038.png]]
이렇게 사용하기 쉽고 간단하게 응답을 문자열 형태로 출력하기 때문에 잘 활용하면 좋다.

## 질문 & 확장

(없음)

## 출처(링크)

- https://rudaks.tistory.com/entry/langchain-StrOutputParser-JsonOutputParser-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0
-
## 연결 노트











