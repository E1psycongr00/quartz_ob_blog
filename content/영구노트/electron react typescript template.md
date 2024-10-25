---
tags:
  - 완성
  - JS
  - Electron
  - React
  - Typescript
aliases: 
date: 2024-08-07
title: electron react typescript
---
작성 날짜: 2024-08-07
작성 시간: 18:48


----
## 내용(Content)

### electron에서 제공하는 템플릿 사용하기

내가 커스텀해서 사용해도 되지만 초기 세팅 과정에서 오류가 발생할 가능성이 크다. 다행히 electron에 webpack + typescript 템플릿을 제공한다.

```shell
npm init electron-app@latest my-new-app -- --template=webpack-typescript
```


### electron에서 제공하는 템플릿에 React 설치하기

```shell
// npm
npm install --save react react-dom
npm install --save-dev @types/react @types/react-dom

// yarn
yarn add react react-dom
yarn add @types/react @types/react-dom --dev
```


### app.tsx와 renderer.ts 작성하기

```tsx
// app.tsx
import React from 'react';
import {createRoot} from 'react-dom/client';

const root = createRoot(document.body);
root.render(<h2>Hello, world!</h2>);
```

```ts
//renderer.ts
import './app';
```

### 실행해보기

실행하기 앞서 package.json에서 script 명령어를 확인해본다.

![[Pasted image 20240808115936.png]]

위 그림을 보면 electron-forge를 기본적으로 사용함을 알 수 있다. electron-forge start를 해보자.

그럼 webpack에서 한번 컴파일하고 다음과 같이 앱이 잘 나옴을 알 수 있다.

![[Pasted image 20240808120126.png]]


## 질문 & 확장

- 템플릿을 사용하면 오류없이 초기 세팅을 할 수 있다. 특히 build 부분이 많이 까다로운데 이 부분을 신경 안써도 되기 떄문에 template을 사용하자.

## 출처(링크)

- https://www.electronforge.io/guides/framework-integration/react-with-typescript#integrate-react-code
- https://www.electronforge.io/templates/typescript-+-webpack-template

## 연결 노트

