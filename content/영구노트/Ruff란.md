---
tags:
  - 완성
  - Python
  - Ruff
aliases:
  - Ruff
date: 2024-09-25
title: Ruff란
---
작성 날짜: 2024-09-25
작성 시간: 13:20


----
## 내용(Content)

### Ruff

>[!summary]
> Ruff는 매우 빠른 Python Linter 와 Code Formatter이며 Rust로 쓰여졌다.

공식 문서에 따르면 Ruff는 다른 린터에 비해 매우 빠르다고 한다. 공식 문서에 나와있는 속도로만 보면 압도적으로 빠르긴 하다.

![[Pasted image 20240925132155.png]]

Ruff는 Rust로 작성된 linter이자 formatter이다. flake8, black등 기존 라이브러리들의 규칙들을 대부분 지원해서 ruff하나만 있어도 프로젝트를 관리하는데 문제가 없다.

### Ruff 사용 예시

```python
# pyproject.toml
...
[tool.ruff]
line-length = 140 # 최대 길이를 140으로 늘려봤습니다.
 
[tool.ruff.lint]
select = [
    "F",  # pyflakes
    "I",  # isort
    "UP",  # pyupgrade
]
ignore = ["E501"]
```

Ruff는 pyproject.toml 또는 ruff.toml에서 설정이 가능하다. 때문에 poetry와도 굉장히 잘 맞고 같이 사용하면 편하다.

```shell
poetry add ruff
```

로 간단히 설치해주면 된다. poetry 설치에 대해서는 [[Windows Poetry 설치 및 정리]] 를 참고하자.
### CLI 에서 사용하기

terminal에서 ruff는 다음과 같이 사용 가능하다.

```shell
ruff check --fix

ruff format
```

## 질문 & 확장

이것 외에도 Github Actions나 pre-commit에 응용 가능하다.

## 출처(링크)

- https://docs.astral.sh/ruff/
- https://chaechae.life/blog/ruff-linter-formatter#cli%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0

## 연결 노트

- [[Windows Poetry 설치 및 정리]]








