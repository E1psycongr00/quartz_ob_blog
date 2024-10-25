---
tags:
  - 완성
  - Husky
  - Git
  - GitHook
  - Commit
aliases: 
title: Husky
date: 2024-02-22
---
작성 날짜: 2024-02-22
작성 시간: 12:53


----
## 내용(Content)
### Husky
>[!summary]
>modern 네이티브 깃 훅을 쉽게 만들어준다


Husky를 활용하면 Git 과 관련된 이벤트(commit, push 등등)가 동작하기 위한 조건이나 룰을 매우 쉽게 작성할 수 있다.

### 설치 및 셋팅
[husky 설치](https://typicode.github.io/husky/get-started.html) 의 npm 기준으로 설치 방법을 설명하겠다.

1. npm install -D husky
2. husky init (.git이 있어야 한다. 없다면 git init)
3. husky init 이후 생성된 husky 폴더에 파일을 추가해 작성한다.

husky v9으로 오면서 add같은 명령어는 deprecated 되었지만 매우 쉽게 사용 가능하다.

![[Pasted image 20240222130303.png]]

commit-msg는 내가 생성한 거고 husky init 명령어를 입력하면 husky 폴더가 생성되는데  __ 폴더를 보면 안에 사용하고 싶은 이벤트를 생각해서 .husky 폴더에 파일을 만들면 된다. 내부에는 

![[Pasted image 20240222130941.png]]

위와 같이 터미널에서 실행하고 싶은 명령어를 입력해주면 된다. 
## 질문 & 확장

(없음)

## 출처(링크)
- https://typicode.github.io/husky/

## 연결 노트
- [[Husky를 활용해 Commit Convention 강제하기]]









