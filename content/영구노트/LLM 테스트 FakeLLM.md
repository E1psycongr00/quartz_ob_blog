---
tags:
  - 완성
  - 솔루션
  - LLM
  - Langchain
  - 테스트
aliases: 
date: 2024-09-24
title: LLM 테스트 FakeLLM
---
작성 날짜: 2024-09-24
작성 시간: 11:35


----

## 문제 & 원인

단위 테스트를 하다 보면 외부에서 불러온 클래스나 모듈을 Mocking 해야 할 때가 있다. 그러나 LLM을 Mocking한다는 것은 필요한 테스트 응답을 만드는 것이 어렵고, LCEL 방식의 코드는 Mocking도 어려울 뿐더러 테스트 코드가 복잡해지거나, 어려워진다.

이런 경우 Fake LLM이 도움이 될 수 있다.


## 해결 방안

### FakeListLLM

Langchain에는 위의 문제를 해결하기 위해
`langchain.llms.fake` 패키지에서 `FakeListLLM`, `FakeStreamingListLLM`을 제공한다.
이들 LLM을 이용해서 Mocking과 단위 테스트를 쉽게 할 수 있다.

```python
import unittest
from langchain.llms.fake import FakeListLLM
from langchain.prompts import PromptTemplate

class TestRAGModule(unittest.TestCase):
    def test_rag_module_with_fake_llm(self):
        fake_responses = [
            "서울은 대한민국의 수도입니다. 대한민국의 정치, 경제, 문화의 중심지로서 중요한 역할을 하고 있습니다. 추가로 궁금한 사항이 있으시면 말씀해 주세요!"
        ]
        # FakeLLM 인스턴스 생성
        fake_llm = FakeListLLM(responses=fake_responses)
        template = "{country}의 수도는?"
        prompt = PromptTemplate.from_template(template)
        chain = prompt | fake_llm

        result = chain.invoke("대한민국의 수도는?")
        print(result)
        self.assertEqual(result, fake_responses[0])

if __name__ == '__main__':
    unittest.main()
```

위 코드는 python의 unittest를 활용해서 Test 코드를 작성해보았다. LLM을 상속받은 모델이기 때문에 LCEL방식도 무리없이 잘 동작하며 list 형식으로 응답 형식을 저장하고 리턴한다.
## 질문 & 확장

(없음)

## 출처(링크)

- https://rudaks.tistory.com/entry/langchain-Fake-LLM-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0

## 연결 노트
