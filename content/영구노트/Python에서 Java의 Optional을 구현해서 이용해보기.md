---
tags:
  - 완성
  - 솔루션
  - Python
aliases: 
date: 2024-09-26
title: Python에서 Java의 Optional을 구현해서 이용해보기
---
작성 날짜: 2024-09-26
작성 시간: 15:02


----

## 문제 & 원인

Optional 타입을 가독성 좋게 사용하기 원한다.


## 해결 방안

```python
from typing import Any, Callable


class Optional:
    def __init__(self, value=None):
        self._value = value

    @classmethod
    def of(cls, value: Any):
        return Optional(value)

    @classmethod
    def empty(cls):
        return Optional()

    def is_present(self):
        return self._value is not None

    def get(self):
        if self._value is None:
            raise ValueError("값이 없습니다.")
        return self._value

    def or_else(self, default: Any):
        return self._value if self._value is not None else default

    def or_else_throw(self, exception: Exception):
        if self._value is None:
            raise exception
        return self._value

    def map(self, func: Callable):
        if self._value is None:
            return Optional()
        return Optional(func(self._value))

    def flat_map(self, func: Callable):
        if self._value is None:
            return Optional()
        result = func(self._value)
        return result if isinstance(result, Optional) else Optional(result)



if __name__ == "__main__":
        print(Optional.empty().or_else_throw(ValueError("x 값이 없습니다.")))

```

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
