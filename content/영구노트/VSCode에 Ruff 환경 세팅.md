---
tags:
  - 완성
  - VSCode
  - Python
  - Ruff
aliases: 
date: 2024-09-25
title: VSCode에 Ruff 환경 세팅
---
작성 날짜: 2024-09-25
작성 시간: 14:02


----
## 내용(Content)

### Ruff 설치

```shell
poetry add --group dev ruff
```

로 설치해준다.
###  Ruff Extension

[[Ruff란|Ruff]] 를 이용하면 매우 빠르게 린터와 포맷팅을 지원받을 수 있다. 이를 정적 검사와 함께 자동으로 린터와 포맷팅이 가능하게 해 줄 수 있는데, VSCode에서는 Ruff Extension을 지원한다.

![[Pasted image 20240925140601.png]]

이것을 설치해준다.

### Setting.json 설정하기

이제 VSCode에 자동으로 적용되게 하기 위해선 다음과 같은 설정이 필요하다.

```json
{
    "python.analysis.autoImportCompletions": true,
    "python.analysis.fixAll": ["source.unusedImports"],  
    "editor.defaultFormatter": "charliermarsh.ruff",
    "editor.formatOnSave": true,
    "files.autoSave": "afterDelay",
    "editor.codeActions.triggerOnFocusChange": true,
    "editor.codeActionsOnSave": {
        "source.fixAll" : "always",
        "source.organizeImports": "always"
    }
}
```

제일 중요한 건 `editor.codeActions.triggerOnFucusChange`가 true여야 한다는 사실이다.
이것이 true여야 codeActionsOnSave에 적용해 놓았던 설정들이 정상적으로 작동한다.
default는 false인데 false이면 "ctrl + s" 를 눌러야만 ruff가 동작한다.

## 질문 & 확장

(없음)

## 출처(링크)

- https://gyuha.tistory.com/m/551

## 연결 노트

- [[Ruff란]]
- [[Ruff 특정 모듈 linter 체크에서 제외하기]]
- [[Ruff Github Actions 예시]]






