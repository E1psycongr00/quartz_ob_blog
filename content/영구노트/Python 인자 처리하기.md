---
tags:
  - 완성
  - Python
aliases:
  - Python args
  - Python 인자
  - argparse
date: 2024-05-27
title: Python 인자 처리하기
---
작성 날짜: 2024-05-27
작성 시간: 16:19


----
## 내용(Content)

### argparse

>[!summary]
>python 스크립트 실행시 인자를 받아 처리하는 라이브러리이다. argparse를 이용하면
>`python hello.py -e environment` 이런 방식으로 실행이 가능하다.

```python
import argparse

parser = argparse.ArgumentParser()
parser.add_argument("-n", "--name", action="store", type=str)
parser.add_argument("-a", "--age", action="store", type=int)
parser.add_argument("-s", "--student", action="store_true", default=False)


args = parser.parse_args()


print(args.name)
print(args.age)
print(args.student)
```

action을 보면 store는 변수를 저장할 때 쓰이며, store_true는 인자의 유무를 판단할때만 사용한다.

위의 코드를 실행하면 다음과 같다.

![[Pasted image 20240527162229.png]]

## 질문 & 확장

(없음)

## 출처(링크)

- https://wikidocs.net/73785#command-line-argument

## 연결 노트

- [[node 인자 처리하기]]
- [[node 복잡한 인자 처리하기]]








