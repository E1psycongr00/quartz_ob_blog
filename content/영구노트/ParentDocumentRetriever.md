---
tags:
  - 완성
  - Python
  - Langchain
  - RAG
  - Retriever
aliases:
  - 상위 문서 검색기
date: 2024-09-27
title: ParentDocumentRetriever
---
작성 날짜: 2024-09-27
작성 시간: 14:41


----
## 내용(Content)

### ParentDocumentRetriever

>[!summary]
> 각 Chunk 간의 맥락이 유지되도록 긴 Context를 원하는 경우와 작은 문서 정보 모두 원하는 경우 사용하는 검색기

ParentDocumentRetriever는 문서 간의 계층 구조를 이용하여, 보다 긴 맥락의 문서를 효율적으로 탐색할 수 있도록 한다.

### 동작 원리

![[ParentDocumentRetriever 동작 원리 (draw).svg]]


Question이 들어오면 쿼리를 만들어서 VectorStore에서 검색한다. 이 때 chunk 1-1이 검색된 유사 청크라면 chunk 1-1의 상위 문서인 Page 1을 모두 포함해서 LLM에게 전달한다. 그러면 LLM은 선택한 유사 청크가 문맥이 짤려서 제대로 답변이 안된 경우라도, 문서 하나를 LLM에게 전달했기 떄문에 문맥에 따라서 chunk 1-1을 이해하고 제대로 된 답변을 기대할 수 있다.

### 코드

##### PDF Load

```python
from langchain_community.document_loaders import PyPDFLoader


loaders = [
    PyPDFLoader("./contents/diva.pdf"),
    PyPDFLoader("./contents/doom.pdf")
]
docs = []
for loader in loaders:
    docs.extend(loader.load_and_split())
```

load_and_split은 기본적으로 RecursiveCharacterTextSplitter를 이용해서 여러 Chunk로 나눠서 Document를 생성해주는 역할을 한다.

위 코드는 이렇게 chunk한 것들을 docs list 변수에 보관한다.
##### HuggingFaceEmbedding

``` python
from langchain.embeddings import HuggingFaceEmbeddings

model_name = "jhgan/ko-sbert-nli"
encode_kwargs = {'normalize_embeddings': True}
ko_embedding = HuggingFaceEmbeddings(
    model_name=model_name,
    encode_kwargs=encode_kwargs
)
```

OpenAIEmbedding 만으로는 token이 감당이 안되기 때문에 HuggingFaceEmbeddings를 이용해서 받아서 사용한다.

##### Vector Store 정의하기

```python
from langchain_chroma import Chroma
from langchain_text_splitters import RecursiveCharacterTextSplitter


child_splitter = RecursiveCharacterTextSplitter(chunk_size=400, chunk_overlap=100)
parent_splitter = RecursiveCharacterTextSplitter(chunk_size=800, chunk_overlap=100)

vector_store = Chroma(
    embedding_function=ko_embedding,
    collection_name="parent-document"
)
```

##### ParentDocumentRetriever 정의하기

```python
from langchain_core.stores import InMemoryStore
from langchain.retrievers import ParentDocumentRetriever

store = InMemoryStore()

retriever = ParentDocumentRetriever(
    vectorstore=vector_store,
    docstore=store,
    child_splitter=child_splitter,
)

retriever.add_documents(docs)
```

vectorstore는 문서 벡터를 저장할 저장소를 지정한다. docstore 매개 변수는 문서 데이터(ParentDocument)를 저장할 장소이다. child_splitter는 하위 문서 분할기에 사용되고, Parent_splitter는 상위 문서를 분할하는데 사용한다.

>[!caution]
>`작은 청크는 vectorstore`, `큰 청크는 docstore`이다.

테스트는 다음과 같이 해볼 수 있다.

```python
# 유사도 검색을 수행합니다.
sub_docs = vectorstore.similarity_search("Word2Vec")
# sub_docs 리스트의 첫 번째 요소의 page_content 속성을 출력합니다.
print(sub_docs[0].page_content)


# 문서를 검색하여 가져옵니다.
retrieved_docs = retriever.invoke("Word2Vec")
# 검색된 문서의 첫 번째 문서의 페이지 내용의 길이를 반환합니다.
print(retrieved_docs[0].page_content)
```

## 질문 & 확장

(없음)

## 출처(링크)

- https://wikidocs.net/234164
- https://www.youtube.com/watch?v=J2AsmUODBak&t=1796s
## 연결 노트










