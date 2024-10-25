---
tags:
  - 완성
  - Python
  - Langchain
  - LCEL
aliases: 
date: 2024-10-01
title: LLM chain 라우팅
---
작성 날짜: 2024-10-01
작성 시간: 09:29

#완성 #Python #Langchain #LCEL 

----
## 내용(Content)

### Chain 라우팅

>[!summary]
> LCEL을 통해서 chain을 분기 별로 구성하는 것을 Chain Routing이라 한다.

chain 라우팅을 구성하는 데는 if 분기 문을 활용한 사용자 정의 로직 또는 RunnableBranch를 이용하는 방법이 있다. 공식 문서에서는 RunnableLambda를 이용해 사용자 정의 함수를 이용하는 것을 추천한다.

### 사용자 정의 Chain 라우팅 예제


```python
prompt = PromptTemplate.from_template(
    """주어진 사용자 질문을 `수학`, `과학`, 또는 `기타` 중 하나로 분류하세요. 한 단어 이상으로 응답하지 마세요.

<question>
{question}
</question>

Classification:"""
)

chain = (
    prompt
    | ChatOpenAI(model="gpt-4o-mini")
    | StrOutputParser()  # 문자열 출력 파서를 사용합니다.
)

```

분류 체인을 형성한다.
수학, 과학, 기타 3개 중 하나로만 대답 가능한 LLM chain 모델을 구성한다.

```python
math_chain = (
    PromptTemplate.from_template(
        """You are an expert in math. \
Always answer questions starting with "깨봉선생님께서 말씀하시기를..". \
Respond to the following question:

Question: {question}
Answer:"""
    )
    | ChatOpenAI(model="gpt-4o-mini")
    | StrOutputParser()
)

science_chain = (
    PromptTemplate.from_template(
        """You are an expert in science. \
Always answer questions starting with "아이작 뉴턴 선생님께서 말씀하시기를..". \
Respond to the following question:

Question: {question}
Answer:"""
    )
    # OpenAI의 LLM을 사용합니다.
    | ChatOpenAI(model="gpt-4o-mini")
)

general_chain = (
    PromptTemplate.from_template(
        """Respond to the following question concisely:

Question: {question}
Answer:"""
    )
    # OpenAI의 LLM을 사용합니다.
    | ChatOpenAI(model="gpt-4o-mini")
)
```

3개의 체인 모델을 만든다. 각각 프롬프트는 수학, 과학, 일반 제너럴 chain 모델을 구성한다.
각각의 chain은 question 키를 받아서 대답을 처리한다. 그리고 각각의 프롬프트는 카테고리에 따라 전문가 역할을 맡은 프롬프트를 구성했다.

```python
def route(info):
    if "수학" in info["topic"].lower():
        return math_chain
    elif "과학" in info["topic"].lower():
        return science_chain
    else:
        return general_chain
    
```

사용자 정의 라우팅 프롬프트로 topic 정보가 들어왔을 때 해당 topic에 따라 다른 체인을 구성하도록 만든다.

```python
full_chain = (
    {"topic": chain, "question": itemgetter("question")}
    | RunnableLambda(route)
    | StrOutputParser()
)
```

![[Pasted image 20241001112415.png]]

full_chain에서는 topic에 처음 카테고리 체인을 구성한다.
![[Pasted image 20241001112516.png]]

topic key에는 AI가 수학이라 응답한 text output을 적용한다. question key에서는 itemgetter를 이용해 처음 입력 `{'question': ~~~}` 이 value를 그대로 가져와서 key에 할당한다.

이제 첫번째 pipeline 단계에서 응답 형태는 다음과 같을 것이다.

```text
{"topic": "수학", "question": "미적분의 개념에 대해서 설명해주세요"}
```

이제 사용자 정의 함수를 체인으로 쓰기 위해 RunnableLamda로 감싸는데 이 때 route에서는 chain에서 리턴한 topic 정보를 가져온다.

![[Pasted image 20241001112814.png]]

![[Pasted image 20241001112911.png]]

route를 이용해서 ChatOpenAI를 분석해보면 question에 그대로 입력이 들어오며, math_chain 이 선택 됬음을 알 수 있다.

### Route 모델 그림

```python
from IPython.display import display, Image

image = full_chain.get_graph().draw_mermaid_png()
display(Image(data=image))
```

![[LLM Chain 라우팅 Flow (draw).svg]]


## 질문 & 확장

(없음)

## 출처(링크)

- https://wikidocs.net/235882

## 연결 노트










