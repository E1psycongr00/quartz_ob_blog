---
tags:
  - 완성
  - 솔루션
  - LLM
  - Langchain
  - LangGraph
aliases:
  - with_structured_output
date: 2024-10-19
title: with_structured_output과 프롬프트 관계
---
작성 날짜: 2024-10-19
작성 시간: 18:34


----

## 문제 & 원인

```python
QUESTION_CLASSIFIER_PROMPT: str = """당신은 YouTube 댓글 분석을 위한 고급 AI 어시스턴트입니다. 주어진 질문의 의도를 파악하고 적절한 키워드를 선택하며, YouTube URL 또는 동영상 ID를 추출해야 합니다.

가능한 키워드:
- 정치성향
- 악성댓글유저
- None

키워드 설명:
1. 정치성향: 댓글의 정치적 입장이나 성향을 파악하고자 할 때 사용합니다.
   예시 질문: "이 댓글의 정치적 성향은 어떤가요?", "댓글 작성자의 정치 성향을 분석해주세요."

2. 악성댓글유저: 댓글 작성자가 악성 사용자인지 판단하고자 할 때 사용합니다.
   예시 질문: "이 사용자가 악성 댓글러인지 판단해주세요.", "이 댓글이 악성 댓글인가요?"

3. None: 위의 두 카테고리에 해당하지 않거나 의도를 명확히 파악할 수 없는 경우 사용합니다.
   예시 질문: "이 댓글에 대해 어떻게 생각하나요?", "이 영상의 조회수는 얼마인가요?"

YouTube URL 또는 동영상 ID 형식:
- 전체 URL 형식: https://www.youtube.com/watch?v=VIDEO_ID
- 단축 URL 형식: https://youtu.be/VIDEO_ID
- 동영상 ID: 11자리 영숫자 문자열 (예: dQw4w9WgXcQ)

지침:
1. 주어진 질문을 주의 깊게 분석하세요.
2. 질문의 의도가 정치성향 분석인지, 악성댓글 판단인지 파악하세요.
3. 둘 다 해당하지 않거나 불분명한 경우 'None'을 선택하세요.
4. 가장 적합한 키워드 하나만 선택하세요.
5. YouTube URL이나 동영상 ID가 포함되어 있는지 확인하고 추출하세요.
6. URL이나 ID가 없는 경우 빈 문자열("")을 사용하세요.

출력 형식:
{
    "keyword": "선택한 키워드",
    "youtube_url_or_id": "추출한 YouTube URL 또는 동영상 ID"
}

주의: 
- JSON 형식으로 정확히 출력하세요.
- keyword는 반드시 '정치성향', '악성댓글유저', 'None' 중 하나여야 합니다.
- youtube_url_or_id는 추출된 URL/ID 또는 빈 문자열("")이어야 합니다.
- 다른 설명이나 추가 정보는 포함하지 마세요.
- 추출된 URL/ID는 반드시 유효한 형식이어야 합니다.
"""

```

이런 프롬프트를 작성하고, 

```python
llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)
structured_llm = llm.with_structured_output(QuestionClassifierOutput)
prompt = ChatPromptTemplate.from_messages(
    [
        ("system", QUESTION_CLASSIFIER_PROMPT),
        ("human", "{question}"),
    ]
)

chain_llm = prompt | structured_llm
```

이렇게 코드를 작성했으나 오류가 발생했다. JSON 형식 응답과 매핑이 일치하지 않아서 문제가 생기는 것이였다.

## 해결 방안

`with_structure_output`을 사용하는 경우에는 프롬프트에 출력 형식을 명시할 필요가 없다. 응답에 맞춰서 출력 형식을 프롬프트를 `with_structure_output` 가 자동으로 등록해주기 때문이다.


## 질문 & 확장

(없음)

## 출처(링크)

- https://rudaks.tistory.com/entry/langchain-%EC%9D%91%EB%8B%B5%EA%B0%92%EC%9D%84-Json%EC%9C%BC%EB%A1%9C-%EC%B6%9C%EB%A0%A5%ED%95%98%EA%B8%B0-%EC%9C%84%ED%95%9C-%EB%8B%A4%EC%96%91%ED%95%9C-%EB%B0%A9%EB%B2%95

## 연결 노트
