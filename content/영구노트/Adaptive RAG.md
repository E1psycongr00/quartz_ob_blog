---
tags:
  - 완성
  - RAG
  - LLM
  - Python
  - LangGraph
aliases:
  - 적응형 RAG
date: 2024-10-14
title: Adaptive RAG
---
작성 날짜: 2024-10-14
작성 시간: 17:53


----
## 내용(Content)

### Adaptive RAG

>[!summary]
> 적응형 RAG는 [query analysis](https://blog.langchain.dev/query-construction/) 과 [active / self corrective RAG](https://blog.langchain.dev/agentic-rag-with-langgraph/)가 합쳐진 전략이다.

![[Pasted image 20241014180649.png]]

Langgraph 문서에 들어가면 Adaptive RAG에 대해 위와 같은 동작 과정을 보여준다.

우선, Question이 들어오면 Query Analysis 단계를 거친다. 이 단계에서는 질문의 의도를 파악하고, 키워드 또는 문맥의 의미를 유추한다. 관련된 데이터가 embedding 된 데이터 인덱스가 있다면 RAG + self-reflection 단계로 가고, 그렇지 않다면 web 검색 단계로 간다. 상황에 따라 또 다른 Route 옵션을 줄 수 있다. 

그 이후에 RAG + self-reflection 단계에서는 여러 단계들이 있지만 요약해서 얘기하면 중간에 평가 노드를 두어 대답이 적절한지 평가하고, 대답을 재구성하는 단계라 할 수 있다.

### 쿼리 분석 단계 이해하기

#### Router 단계

코드로 쉽게 쿼리 분석 단계 중 Router 단계에 대해 이해해보자.

```python
def route_question(state: GraphState):
    question = state["question"]
    source = question_router.invoke({"question": question})
    if source.datasource == "web_search":
        return "web_search"
    elif source.datasource == "vectorstore":
        return "vectorstore"
```

GraphState에서 question 속성을 받아온다. 가져온 question을 이용해서 question_router로 정의된 LLM에게 질문을 요청한다.

```python
# Data model
class RouteQuery(BaseModel):
    """Route a user query to the most relevant datasource."""

    datasource: Literal["vectorstore", "web_search"] = Field(
        ...,
        description="Given a user question choose to route it to web search or a vectorstore.",
    )


# LLM with function call
llm = ChatOpenAI(model="gpt-3.5-turbo-0125", temperature=0)
structured_llm_router = llm.with_structured_output(RouteQuery)

# Prompt
system = """You are an expert at routing a user question to a vectorstore or web search.
The vectorstore contains documents related to agents, prompt engineering, and adversarial attacks.
Use the vectorstore for questions on these topics. Otherwise, use web-search."""
route_prompt = ChatPromptTemplate.from_messages(
    [
        ("system", system),
        ("human", "{question}"),
    ]
)

question_router = route_prompt | structured_llm_router
```

코드를 분석하면 해당 LLM은 question을 질문받았을 때 해당 question에 따라 question을 web-search해야 할 지 아니면 Local에 임베딩된 데이터가 저장된 vectorstore에 접근해야 할지 알려주는 LLM model이다. 그리고 그 결과를 RouteQuery("vectorstore" 또는 "web-search") 형태로 반환한다.

```python
# Build graph
workflow.add_conditional_edges(
    START,
    route_question,
    {
        "web_search": "web_search",
        "vectorstore": "retrieve",
    },
)
```

이렇게 정의된 route_question 함수는 conditional_edges의 조건부 간선에 연결될 노드로 쓰인다.

![[Conditional edge (draw).svg]]

route_question에서는 web_search와 retrieve 두 개만 답변하도록 구성했기 때문에 router_question 답변에 따라 web_search 또는 retrieve 노드로 갈라질 것이다.


>[!info]
>GraphState는 그래프에서 상태를 담당한다. TypedDict나 Pydantic의 BaseModel로 key -value 형태의 Object 형태로 구성된다. 그리고 이러한 상태는 key 중 변경된 점만 변경되고, 나머지 key-value는 유지되는 특징을 가진다.

#### Generate 노드

```python
### Generate
from langchain import hub
from langchain_core.output_parsers import StrOutputParser

# Prompt
prompt = hub.pull("rlm/rag-prompt")

# LLM
llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0)


# Post-processing
def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)


# Chain
rag_chain = prompt | llm | StrOutputParser()

# Run
generation = rag_chain.invoke({"context": docs, "question": question})
```

generate 단계에서는 단계를 거쳐서 결국 context 정보와 question 정보를 모아 사용자에게 필요한 대답을 생성하는 역할을 맡고 있는 LLM이다. 

이제 해당 chain을 사용하는 generate 노드를 생성해보자.

```python
def generate(state):
    question = state["question"]
    documents = state["documents"]

    # RAG generation
    generation = rag_chain.invoke({"context": documents, "question": question})
    return {"documents": documents, "question": question, "generation": generation}
```

generate 단계에서는 참고한 documents와 question을 통해 answer를 생성하고 생성한 answer를 generation이라는 key와 함께 상태를 업데이트 시키는 노드이다.

아까 쿼리 분석에서 web_node로 라우터가 정해줬다면 바로 generate node를 연결 시켜서 사용자 응답을 제공한다.

### Self RAG / C RAG 단계

#### 자기 반성 RAG

>[!summary]
>LLM을 사용하여 품질이 좋지 않은 검색 및 세대를 스스로 수정한다는 아이디어에 기반된 RAG

![[Pasted image 20241015115156.png]]

자기 반성 RAG는 가벼운 평가자를 두어서 대답이 옳바른지 평가하고, 잘못된 경우 대답을 다시 쓴다.


#### 교정 RAG (CRAG)

- 가벼운 검색 평가기를 사용하여 질의에 대해 검색된 문서의 전반적인 품질을 평가해서 신뢰도 점수를 반환한다.
- 벡터 스토어의 검색이 모호하거나, 질의와 관련없다고 판단한 경우 컨텍스트를 보완하기 위해 웹 기반 문서 검색을 수행한다.
- 관련 없는 스트립을 필터링한다.

![[Pasted image 20241015120910.png]]

논문의 CRAG에 대한 다이어그램이다. 이 다이어그램을 보면 Retriever를 통해 문서를 가져오고 그 이후 `Retrieval Evaluator` 에서 문서가 적합한지 평가하는 과정을 거친다. 평가에 따라 외부에 지식을 검색할 지, 아니면 가져온 문서의 데이터를 정제할 지만 판단하고, 경우에 따라 질문과 함께 정제된 지식 또는 외부 지식들을 전달해서 대답을 형성한다.
#### Self RAG

![[Pasted image 20241015121345.png]]

Self RAG는 중간에 여럿 평가자를 두어 정보가 적합한지 평가한다. 


## 질문 & 확장

(없음)

## 출처(링크)

- https://langchain-ai.github.io/langgraph/tutorials/rag/langgraph_adaptive_rag/
- https://blog.langchain.dev/agentic-rag-with-langgraph/
## 연결 노트


