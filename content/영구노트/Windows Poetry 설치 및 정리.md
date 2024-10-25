---
tags:
  - 완성
  - Python
  - Poetry
aliases:
  - Poetry 설치
date: 2024-09-13
title: Windows Poetry 설치 및 정리
---
작성 날짜: 2024-09-13
작성 시간: 18:14


----
## 내용(Content)

### Poetry란

>[!summary]
>Poetry는 파이썬 의존성을 효율적으로 관리하기 위한 도구

![[Pasted image 20241010190127.png]]

![[Pasted image 20241010190137.png]]
poetry를 활용하면 python dependency를 효율적으로 다룰 수 있다. 

python은 패키지 관리를 위해서 가상 환경을 권장하고 venv, anaconda 와 같은 환경을 많이 이용했다. 하지만 이러한 도구들의 단점은 패키지 버전에 따라 자동으로 관리해주지 않는다는 것이다. poetry는 마치 node.js 진영의 yarn 처럼 .lock 파일이 존재하고, pyproject.toml에서 패키지를 세부적으로 관리 가능하다.

poetry를 사용하면 장점은 다음과 같다.

1. 자동으로 패키지 버전을 관리 -> 실행 이전에 패키지 버전별 호환성을 미리 파악 가능
2. build가 가능하고 명령어로 requirements.txt 등을 생성 가능하다.
3. publish를 이용해 pypi에 쉽게 배포가 가능하다.
4. 문서가 쉽게 잘 정리되어 있다.

### Poetry 설치시 유의할 점

Windows의 경우 python을 microsoft store에서 설치하지 말고 정식 python 사이트에서 직접 설치하자. 그렇지 않으면 powershell로 poetry 설치 시 경로 관련 문제 때문에 오류가 많이 발생하고, 설치에 성공하더라도 패키지 설치시에 알 수 없는 이유가 발생할 가능성이 있다. 처음에 microsoft store에서 python 설치하다가 개 고생했으니.. 꼭 알아두자

### vscode와 poetry

poetry로 의존성을 등록하면 처음에 python interpreter를 경로에 지정해줘야 한다. 그 이후부터는 알아서 잘 인식한다.

## 질문 & 확장

(없음)

## 출처(링크)

- https://teddylee777.github.io/poetry/poetry-tutorial/
- https://python-poetry.org/

## 연결 노트


