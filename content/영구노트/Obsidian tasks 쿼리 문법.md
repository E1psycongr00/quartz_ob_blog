---
tags:
  - Obsidian
aliases: 
title: Obsidian tasks 쿼리 문법
date: 2023-12-04
---
작성 날짜: 2023-12-04
작성 시간: 14:11

#완성 #Obsidian 

----
## 내용(Content)

tasks를 사용하면 효율적으로 task를 관리할 수 있다. tasks 플러그인의 장점은 task를 만들고 쿼리해서 가져오고 관리할 수 있다는 점이다.

몇 가지 task들을 가져올 수 있는 문법을 알아보자

#### 오늘까지 완료되지 않은 항목 조회
```
not done 
due today
```


#### 마감기한이 지나지 않았고, 완료 되지 않았으며, 2주 안에 해야 할 목록 조회

```text
not done
due after yesterday
due before in two weeks
```


#### 보관함 내에 tasks라고 heading을 달아 놓은 완료된 항목 조회
```text
done heading 
includes tasks
```

#### 파일 경로 중에 `Planner` 을 포함하고 2022년 1월 20일까지 완료되지 않은 모든 항목 조회하기

```text
not done
due on 2022-01-20
path includes Planner
```

#### 오늘부터 2주 동안 하지 않은 일 마감일과 수정 버튼 없애기
```text
not done
due after today
due before 2021-05-15
hide due date
hide edit button
```
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










