---
tags:
  - 완성
  - Python
  - Ruff
aliases: 
date: 2024-10-23
title: Ruff Github Actions 예시
---
작성 날짜: 2024-10-23
작성 시간: 10:57


----
## 내용(Content)

```yaml
name: Python application

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4
    - name: Set up Python 3.12
      uses: actions/setup-python@v4
      with:
        python-version: "3.12"
    - name: Install Poetry
      run: |
        curl -sSL https://install.python-poetry.org | python3 -
        echo "$HOME/.local/bin" >> $GITHUB_PATH
    - name: Install dependencies
      run: |
        poetry install
    - name: Lint with Ruff
      run: |
        poetry run ruff check .
    - name: Run tests
      run: |
           poetry run python -m unittest discover -v -p "*_test.py" tests

```

push 및 pull_request시 도작한다.

jobs는 build라는 이름이고 ubuntu-latest os 아래에서 동작하며 poetry를 설치해서 linter 체크와 unittest로 모든 테스트가 통과하는지 테스트한다.
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










