---
tags:
  - 완성
  - 솔루션
  - NextJS
aliases: 
date: 2024-03-05
title: nextjs의 getStaticProps가 매번 실행되는 문제
---
작성 날짜: 2024-03-05
작성 시간: 23:18


----

## 문제 & 원인
`[...slug].tsx`의 getStaticProps에 대해서 console.log 를 찍고 

```terminal
npm run dev
```

를 실행하면 매번 콘솔이 찍힌다. 이 뜻은 계속해서 getStaticProps가 요청시 계속해서 실행된다는 의미이다.
## 해결 방안

getStaticProps는 빌드 시간에 한 번만 실행되지만 2가지 경우에는 getStaticProps가 추가적으로 실행될 수 있다.

- getStaticPath에서 fallback이 True인 경우
- 개발 모드 (npm run dev 또는 yarn run dev)로 실행한 경우

>[!warning] getStaticPath fallback => true
> 빌드시 생성되지 않은 동적 페이지에 대해서 404가 아닌 페이지가 생성되고 캐싱된다. 이런 경우 getStaticProps가 다시 실행된다.

>[!warning] npm run dev
>개발 모드에선 매번 요청마다 getStaticProps가 실행된다. 그 이유는 개발 중에 데이터 변경이나 코드 수정을 실시간으로 반영하기 위함이다.

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
