---
tags:
  - 완성
  - Python
  - Langchain
  - LCEL
aliases: 
date: 2024-09-30
title: RunnableLambda
---
작성 날짜: 2024-09-30
작성 시간: 18:41


----
## 내용(Content)

### RunnableLamda

>[!summay]
>사용자 정의 함수를 받아서 Runnable하게 만드는 방법.

RunnableLamda는 단일 인자만 들어가며, 다양한 인자를 입력으로 넣고 싶다면 dict를 활용한다. RunnableLamda를 이용하면 사용자 함수를 쉽게 넣어서 pipeline을 구성할 수 있다.

```python
def length_function(text):  # 텍스트의 길이를 반환하는 함수
    return len(text)

runnable = RunnableSequence(
    {"a": itemgetter("input_1") | RunnableLambda(length_function),
     "b": {"text1": itemgetter("input_1"), "text2": itemgetter("input_2")} | RunnableLambda(lambda x: len(x["text1"]) + len(x["text2"])) }
     | RunnableLambda(lambda x: x["a"] + x["b"])
)

runnable.invoke({'input_1': '안녕하세요', 'input_2': '반갑습니다'})
```

![[Pasted image 20240930190424.png]]

RunnableSequence은 Runnable한 객체만 들어갈 수 있다. 그러나 itemgetter는 단순히 value 단일 값만 반환하는데 이것을 `|`로 연결하고 RunnableLamda를 사용하면 사용자 함수를 Runnable한 객체로 만들 수 있다. 
첫번째 a는 단순한 사용이며, 두번째 b의 경우에는 2개의 인자를 dictionay 형태로 받아서 처리하고 있다. 마지막 RunnableLamda는 이렇게 가져온 데이터의 dict를 a, b key를 이용하여 단일 값으로 리턴한다. 

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










