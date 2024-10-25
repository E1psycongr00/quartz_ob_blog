---
tags:
  - 완성
  - 솔루션
  - version
  - npm
  - 배포
  - 라이브러리
  - 명세
aliases: 
date: 
title: npm과 버저닝
---
작성 날짜: 2024-03-15
작성 시간: 21:37


----

## 문제 & 원인
npm 패키지는 어떻게 버전을 관리해야 하는가..? 라이브러리 패키지나 또는 내가 설계한 앱을 릴리즈 할 때 버저닝에 대한 고민이 생긴다.

## 해결 방안
### SemVer
라이브러리를 개발하거나 특정 앱을 개발해서 배포하는 경우 버저닝을 위한 명세가 존재하는 데 그것이 바로 [[Semantic Versioning 체계|SemVer]]이다. X.Y.Z의 특징을 지어서 라이브러리 배포시 버저닝을 정한다.

### major, minor, patch
npm에서는 버전을 관리할 때 major(주), minor(부), patch(수)로 관리한다.

npm에서 버전을 수동으로 숫자를 관리하는 것이 아니라 명령으로 관리하면 편한데 다음과 같다.

```shell
npm version patch
npm publish
```

```shell
npm version minor
npm publish
```

```shell
npm version major
npm publish
```


이렇게 명령어를 통해 버전을 관리하면 실수를 줄이고 쉽게 버전을 관리할 수 있다.
minor나 major 명령 수행시 그보다 낮은 순위의 버전은 모두 0으로 알아서 초기화된다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://junghyeonsu.com/posts/deploy-simple-util-npm-library/#%EB%AF%B8%EB%A6%AC%EB%B3%B4%EA%B8%B0

## 연결 노트
- [[Semantic Versioning 체계]]
