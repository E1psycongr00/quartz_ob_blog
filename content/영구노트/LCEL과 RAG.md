---
tags:
  - 완성
  - LLM
  - RAG
  - Langchain
aliases: 
date: 2024-09-24
title: LCEL과 RAG
---
작성 날짜: 2024-09-24
작성 시간: 20:03


----
## 내용(Content)

### LECL

>[!summary]
>langchain에서는 pipline으로 langchain 모듈간의 연결을 "|"로 쉽게 표현가능하다.

### RAG와 함께 LECL 예제

#### 1. 문서 임베딩과 Retrieval 질의 검색

```python
from dotenv import load_dotenv
from langchain_core.documents import Document
from langchain_community.vectorstores import FAISS
from langchain_openai import OpenAIEmbeddings

load_dotenv()

docs = [
    Document(page_content="the dog loves to eat pizza", metadata={"source": "animal.txt"}),
    Document(page_content="the cat loves to eat lasagna", metadata={"source": "animal.txt"})
]

db = FAISS.from_documents(docs, OpenAIEmbeddings())

retriever = db.as_retriever()
result = retriever.invoke("What does the dog want to eat?")
print(result)
```

그러면 invoke 결과로 "the dog loves to eat pizza" Document가 출력된다.

#### 2. Prompt와 모델 설정

```python
prompt = ChatPromptTemplate.from_template(
    """Answer the question based only on the following context:
{context}
Question: {question}
"""
)

model = ChatOpenAI()

```

#### 3. chain 형성하기

##### 질문 가공 후 검색

```python
retrieval_chain = (
    {
        "context": (lambda x : x["question"]) | retriever,
        "question": itemgetter("question")
    }
    | prompt
    | model
    | StrOutputParser()
)

result = retrieval_chain.invoke({"question": "what does the dog want to eat?"})
print(result)
```

이 코드는 invoke시 입력된 딕셔너리에서 "question" 키의 값을 추출해서 이를 retriever의 쿼리로 활용하라는 뜻이다.
question에는 itemgetter를 사용해 딕셔너리의 "question" 키의 값을 "question"에 전달하라는 뜻이다.

여기서는 단순하게 Question 키의 값을 뽑아서 그대로 전달했지만, 상황에 따라 전처리를 해서 Retriever에 넘길 수도 있다.

##### 질문 그대로 검색

```python
retrieval_chain = (
    {
        "context": retriever,
        "question": RunnablePassthrough()
    }
    | prompt
    | model
    | StrOutputParser()
)

result = retrieval_chain.invoke({"what does the dog want to eat?"})
print(result)
```

"context"에 retriever만 전달되므로, 질문 자체를 그대로 retriever로 전달한다.
질문에 변형 없이 RunnablePassthrough()를 사용하면, 질문을 가공하지 않고 그대로 전달한다.

다만 위와 같이 사용한 경우에는 dictionary가 아닌 단순 문자열 형태로 질의를 해야한다.


## 질문 & 확장

(없음)

## 출처(링크)

- https://rudaks.tistory.com/entry/langchain-LCEL%EB%A1%9C-RAG%EB%A5%BC-%EA%B5%AC%ED%98%84%ED%95%98%EB%8A%94-%EA%B8%B0%EB%B3%B8-%EC%98%88%EC%A0%9C

## 연결 노트




