---
tags:
  - 완성
  - Typescript
  - NextJS
  - 빌드
aliases: 
date: 2024-04-03
title: nextjs build 이전에 스크립트 실행 방법
---
작성 날짜: 2024-04-03
작성 시간: 20:26


----
## 내용(Content)
### nextjs pre build, post build

>[!summary]
>스크립트를 작성하고 package.json에서 실행할 실행 명령어의 pre 접두어를 붙여서 정의해면 된다. 만약 build를 끝나고 script를 작업해야 하는 경우 post 접두어를 붙여주면 된다.

nextjs 를 사용하다 보면 빌드 이전에 꼭 해야 할 스크립트를 실행해야 할 필요성이 있을 때가 있다.

### example
예를 들면 API Router를 구성하고 해당 db를 초기화하고 가져와야 하는 작업을 진행한다고 가정하자. db를 초기화할 때 여러 데이터를 db에 저장하는 초기화 작업을 진행하는 경우 staticProps로 진행한다면 매번 페이지를 빌드할 때 db를 초기화한다. 이 과정은 매우 비효율적이다. 그렇기 때문에 빌드 이전에 한번만 db를 초기화해야 할 수도 있다.

이 때 prebuild.js를 작성하고, 이를 package.json에서 다음과 같이 작성해주면 된다.

```json
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "copy-image": "node ./bin/pre-build.mjs",
    "predev" : "npm run copy-image",
    "prebuild" : "npm run copy-image"
  },
```

## 질문 & 확장

(없음)

## 출처(링크)
- https://velog.io/@nyoung113/package.json-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%8B%A4%ED%96%89-%EC%9D%B4%EC%A0%84%EC%97%90-%EB%8B%A4%EB%A5%B8-%EB%AA%85%EB%A0%B9%EC%96%B4-%EA%B0%99%EC%9D%B4-%EC%8B%A4%ED%96%89%ED%95%98%EA%B3%A0-%EC%8B%B6%EC%9D%84-%EB%95%8C-pre-%EC%A0%91%EB%91%90%EC%82%AC

## 연결 노트
- [[node 인자 처리하기|node 인자 처리하기]]
- [[node 복잡한 인자 처리하기]]









