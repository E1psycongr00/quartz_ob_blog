---
tags:
  - 완성
  - Spring
  - JAVA
aliases: 
title: MessageSource 분석하기
date: 2023-12-07
---

작성 날짜: 2023-12-07
작성 시간: 21:33


----
## 내용(Content)

### MessageSource란

> [!info]
> 메시지를 처리하기 위한 전략 인터페이스이고 국제화를 지원한다.
> 보통 2개의 구현체가 존재하는데 
> - **org.springframework.context.support.ResourceBundleMessageSource**: 커스텀이 제한적
> - **org.springframework.context.support.ReloadableResourceBundleMessageSource**: 커스텀이  자유로움
> 
> 

MessageSource는 Code 기반으로 동작한다. messages.properties 파일에서 key, value 형식으로 값들을 저장하고 이들을 매핑해서 출력해준다.

MessageSource의 특징은 다음과 같다.

- 다국어를 지원한다.
- code 형식으로 메시지를 처리한다.

#### getMessage
getMessage의 3개의 다른 인자 형태로 존재한다.
2개는 필수적으로 code가 들어가며  defaultMessage가 있느냐 없느냐에 따라 달라지고 나머지 하나는 MessageSourceResolvable 타입을 인자로 받아 처리한다.

MessageSOurceResolvable은 ObjectError, FieldError등이 속한다.



## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










