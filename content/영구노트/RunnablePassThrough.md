---
tags:
  - 완성
  - Python
  - Langchain
  - LCEL
aliases: 
date: 2024-09-30
title: RunnablePassThrough
---
작성 날짜: 2024-09-30
작성 시간: 17:53


----
## 내용(Content)

### RunnablePassthrough

>[!summary]
> Runnable을 상속받은 타입으로 데이터를 전달하는 역할을 수행한다. 단독으로 호출하는 경우 데이터를 있는 그대로 전달하며, assign을 이용하면, 데이터를 가공해서 전달할 수 있다.

그대로 전달하거나, 추가 key를 할당해서 데이터 전달이 가능하다.


### 데이터 전달 예제

```python
from langchain_core.runnables import RunnableParallel, RunnablePassthrough


runnable = RunnableParallel(
    passed=RunnablePassthrough(),
    extra=RunnablePassthrough.assign(mult=lambda x: x['num'] * 3),
    modified=lambda x: x['num'] + 1
)

runnable.invoke({'num': 1})
```

- RunnableParallel은 병렬로 실행 가능한 작업 정의
- 두번째 extra에서 .assign을 통해 mult라는 새로운 key를 정의했다. 이 경우 `{'num': 1, 'mult': 3}` 이 할당된다.
- 세번째의 경우 주어진 num에서 + 1을 한다.

**결과**
![[Pasted image 20240930180234.png]]

### Retriever & LLM 전달 예제

```python
from langchain.prompts import ChatPromptTemplate
from langchain.schema import StrOutputParser
from langchain_openai import ChatOpenAI


retriever = vectorstore.as_retriever()
template = """Answer the question based only on the following context:
{context}

Question: {question}
"""
prompt = ChatPromptTemplate.from_template(template)

# ChatOpenAI 모델을 초기화합니다.
model = ChatOpenAI(model="gpt-4o-mini")


# 문서를 포맷팅하는 함수
def format_docs(docs):
    return "\n".join([doc.page_content for doc in docs])

retrieval_chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt
    | model
    | StrOutputParser()
)

retrieval_chain.invoke("테디의 직업은 무엇인가요?")

```

이 코드를 langsmith에서 어떻게 동작하는지 분석해보자.

우리가 구성한 Retrival_chain은 langsmith에서 다음과 같이 구성한다.

![[Pasted image 20240930181147.png]]


RunnableSequence로 input이 들어왔을 때 question은 그대로 전달하고, context는 map으로 VectorStoreRetriever와 format_docs가 나란히 실행된다.

![[Pasted image 20240930181449.png]]

map:key:context 탭을 눌렀을 때 나오는 화면이다. json 형태로 'input' key 값에 질문이 들어갔음을 알 수 있다. 그리고 결과는 각각의 document들이 "\n" 형태로 "output"key에 할당되었음을 알 수 있다.

![[Pasted image 20240930181611.png]]

안에 조금 더 세부적으로 살펴보자.
retriever에는 "query" key 형태로 질의가 들어갔음을 알 수 있다. 그리고 이는 document 형태로 출력이 된다. 

	
format_docs는 docs를 입력으로 받아서 \n 형태의 text 데이터로 만들어준다.

이를 이용해서 "context"와 "question"이 완성된다.

![[Pasted image 20240930183027.png]]

위 그림을 보면 input에 context에 document들이 와서 참조됨을 알 수 있다. 이를 Prompt를 기반으로 AI는 답변을 한다.

### RunnableSequence로 디버깅해보기

사실 해당 체인은 RunnableSequence다. 그래서 이해를 더 쉽게 하기 위해 예제 코드를 보자

```python
sub_chain = RunnableSequence(
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | RunnablePassthrough.assign(preprocessed=preprocess)
)
```

2단계를 거친 이유는 RunnableSequence는 단독으로 사용할 수 없다.

우선 RunnableParallel를 이용해서 단독으로 `{"context": retriever | format_docs, "question": RunnablePassthrough()}`이 코드를 실행하면 다음과 같다.

![[Pasted image 20240930183436.png]]

이렇게 context과 question이 생성됨을 알 수 있다.
이를 받아서 가공하고 preprocess라는 key를 할당한 결과는 다음과 같다.

```python
def preprocess(x):
    return "context: " + x["context"] + "\nquestion: " + x["question"]
```

![[Pasted image 20240930183537.png]]


## 질문 & 확장

(없음)

## 출처(링크)

- https://wikidocs.net/235580

## 연결 노트










