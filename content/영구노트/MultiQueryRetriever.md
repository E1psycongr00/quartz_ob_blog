---
tags:
  - 완성
  - Python
  - Langchain
  - RAG
  - Retriever
aliases:
  - 다중 쿼리 검색기
date: 2024-09-27
title: MultiQueryRetriever
---
작성 날짜: 2024-09-27
작성 시간: 13:38


----
## 내용(Content)

### MultiQueryRetriever

>[!summary]
>주어진 사용자 입력 쿼리에 대해 그와 유사한 질문을 여러 개 생성 임베딩해서 Vector Store에 쿼리하는 검색기

![[MultiQueryRetriever 합집합 (draw).svg]]

MultiQueryRetriever는 LLM을 활용해 vector store 검색을 위한 쿼리를 여러 개 생성해서 검색의 질을 높인다. 그렇기 때문에 프롬프트를 어느 정도 자동화해주는 효과가 있다. 

여러 쿼리를 생성해서 관련된 다양한 답변을 얻을 수 있기 때문에 거리 검색의 한계를 어느 정도 극복하고 질문에 관련된 풍부한 내용을 얻을 수 있다. 하지만 LLM을 사용하므로 기존 Retriever에 비해 비용이 더 들어간다.



### 동작 과정

![[Multi Query Retriever 동작 원리 (draw).svg]]

### 코드

#####  web 사이트 text 불러오기

```python
from langchain_community.document_loaders import WebBaseLoader

loader = WebBaseLoader("https://www.etnews.com/20240926000220?mc=ns_001_00001")
data = loader.load()
data
```

```text
[Document(metadata={'source': 'https://www.etnews.com/20240926000220?mc=ns_001_00001', 'title': "'배터리 1위' 中 CATL, 한국 법인 세운다 - 전자신문", 'description': '미래를 보는 창 - 전자신문', 'language': 'ko'}, page_content="\n\n\n\n\n\n'배터리 1위' 中 CATL, 한국 법인 세운다 - 전자신문\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n \n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n \n\n \n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n로그인회원가입\n\n\n\n\n\n\n\n\nSW\nIT\n경제\n전자\n모빌리티\n플랫폼/유통\n과학\n정치\n오피니언\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n뉴스\n속보\nSW\nIT\n경제\n전자\n모빌리티\n플랫폼/유통\n과학\n\n\n\xa0\n정치\n오피니언\n국제\n전국\n스포츠\n특집\n연재\n\n\n라이프\n연예\n포토\n공연전시\n생활문화\n\n\n비주얼IT\n이슈플러스\nHot 영상\n뷰포인트\n인포그래픽\n\n\n부가서비스\nConference\nallshowTV\n시사용어\nPDF서비스\n\n\n서비스안내\n신문구독신청\n콘텐츠구매\n초판서비스\n회원서비스\n내 스크랩\n\n\n이용안내\n지면광고안내\n행사문의\n디지털광고안내\n이용약관\n개인정보취급방침\n고충처리\n\n\n회사소개\n전자신문 소개\n전자신문인터넷 소개\n연혁\nCI소개\n회사위치\n 
```

WebBaseloader를 이용해서 특정 URL의 뉴스 정보를 긁어서 가져온다. 그래서 data를 출력하면 source와 함께 text 내용을 긁어온다. 핵심 내용은 page_content에 저장되어 있다.

##### Text Splitter (Chunk 쪼개기)

```python
from langchain_text_splitters import CharacterTextSplitter


text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=100)
splits = text_splitter.split_documents(data)
```

이런 방식으로 Document 객체 형태로 데이터가 생성된다. 이 데이터를 직접 VectorStore에 넣어도 되긴 하지만 너무 크기 때문에 쪼개서 문서(Chunk)를 생성해보자.

##### Vector DB 정의하기

```python
from langchain_community.vectorstores import FAISS
from langchain_openai import OpenAIEmbeddings


vector_db = FAISS.from_documents(splits, OpenAIEmbeddings())
```

##### MultiQueryRetriever 정의하기

```python
from langchain_openai import ChatOpenAI
from langchain.retrievers import MultiQueryRetriever

question = "CATL은 어떤 배터리가 강점이야?"

llm = ChatOpenAI(model="gpt-3.5-turbo")

retriever_from_llm = MultiQueryRetriever.from_llm(
    llm=llm,
    retriever=vector_db.as_retriever(),
)
```

이 코드를 해석하면 MultiQueryRetriever 인스턴스를 생성하는데 from_llm을 통해 llm으로 검색 쿼리를 생성한다.

```python
retriever_from_llm.get_relevant_documents(query=question)
```


## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










