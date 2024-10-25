---
tags:
  - 완성
  - 솔루션
  - VSCode
  - Python
aliases: 
date: 2024-09-25
title: VSCode package index depth 설정하기
---
작성 날짜: 2024-09-25
작성 시간: 14:52


----

## 문제 & 원인

langchain 패키지의 경우 depth 흔히 호출하는 클래스나 모듈들이 depth가 깊은 경우가 많은데 pylance가 이를 잘 인식하지 못해서 개발 속도가 떨어진다.

예를 들어
StrOutputParser()의 경우, 인식하지 못하여 직접 import를 써줘야 하는 상황이 생기는데, 정확히 모른다면 일일히 문서를 찾아서 import 해줘야 하는 문제점이 생긴다.

```python
from langchain_core.output_parser import StrOutParser # 3depth
```
## 해결 방안

pylance에는 기본적으로 필요한 라이브러리에 대해서만 depth가 기본적으로 2로 설정되어 있다. 이는 Editor의 성능을 위해서다. 그러나 필요한 경우, 특정 라이브러리에 대해서 Package 인식 depth를 늘릴 수 있다.

![[Pasted image 20240925145231.png]]

vscode 셋팅에 가보면 Package Index Depths라는 항목이 있고, 이를 Edit in settings.json 클릭하고 다음과 같이 수정해준다.

```json
    "python.analysis.packageIndexDepths": [
        {
            "name": "*",
            "depth": 2
        },
        {
            "name": "langchain_core",
            "depth": 5
        },
        {
            "name": "langchain_community",
            "depth": 5
        }
    ],
```

상황에 따라서 얼마든지 다양하게 설정 가능하다.
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
