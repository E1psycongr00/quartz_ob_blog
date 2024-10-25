---
tags:
  - 완성
  - 솔루션
  - Python
  - Ruff
aliases: 
date: 2024-10-23
title: Ruff 특정 모듈 linter 체크에서 제외하기
---
작성 날짜: 2024-10-23
작성 시간: 10:27


----

## 문제 & 원인

```shell
ruff run check
```

![[Pasted image 20241023102725.png]]

googleapiclient 모듈의 `__init__.pyi`에서 linter check시 오류가 발생한다. 

RUFF 린터 방식을 CI로 사용하는 경우 문제가 발생할 수 있기 떄문에 이를 해결해주어야 한다.

## 해결 방안

`pyproject.toml`에 Ruff 관련 설정을 추가한다.

지금의 문제는 특정 모듈이 미사용 import 경고가 표시되며, `F401` `F404` 로 문제가 발생한다.
이를 해결하기 위해서 특정 모듈의 파일을 무시하도록 설정하면 된다.


```toml

[tool.ruff.lint.per-file-ignores]
"**/*.pyi" = ["F401", "F404"]  
```

이 방식을 이용하면 googleapiclient 모듈의 문제를 해결할 수 있다.


## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트

- [[Ruff란]]