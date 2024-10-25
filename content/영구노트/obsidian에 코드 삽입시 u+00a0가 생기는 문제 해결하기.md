---
tags:
  - 완성
  - 솔루션
  - Obsidian
  - html
aliases: 
date: 2024-03-03
title: obsidian에 코드 삽입시 u+00a0가 생기는 문제 해결하기
---
작성 날짜: 2024-03-03
작성 시간: 12:47


----

## 문제 & 원인
vscode와 같은 에디터에서 코드를 복붙하다 보면 문제가 발생할 수 있다.
```ts
import * as d3 from "d3";
import { useRef, useEffect } from "react";

interface LinePlotProps {
    data: number[];
    width?: number;
    height?: number;
    marginTop?: number;
    marginRight?: number;
    marginBottom?: number;
    marginLeft?: number;
}
```

vscode에서 코드를 가져와 obsidian 코드에 복붙을 하고 다시 그 코드를 복사해 vscode에 붙이면 다음과 같은 문제가 발생한다.

![[Pasted image 20240303124939.png]]

python 과 같은 코드는 tab에 따라 scope가 결정되므로 이런 문제는 `복사 & 붙여 넣기`할 때 큰 문제가 될 수 있다. 매번 이런 오류를 보면 `복사 & 붙여 넣기` 할 때마다 코드에 대한 신용이 떨어질 수 밖에 없다.

>[!note]
>U+00a0는 No-Break-Space 줄여서 NBSP라 부르며 HTML 에서는 &NBSP로 쓰인다.
>markdown 언어의 경우 결국엔 렌더링할 때 HTML로 렌더링 하는데 코드 블럭에서 상황에 따라 코드의 빈 공간을 `&nbsp`로 처리하는 경우가 있다.

## 해결 방안
Obsidian에서는 이를 위해 저런 whitespace를 시각화할 수 있는 라이브러리를 제공한다.

![[Pasted image 20240303125619.png]]

위 라이브러리를 설치하고 

코드를 shift + tab => tab을 누르면 => 이런 식으로 `&nbsp` 에서 `tab`으로 바뀜을 알 수 있다.
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
