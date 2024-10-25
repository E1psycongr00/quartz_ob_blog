---
tags:
  - 완성
  - JS
  - Electron
aliases: 
date: 2024-08-05
title: Electron이란 무엇인가
---
작성 날짜: 2024-08-05
작성 시간: 20:34


----
## 내용(Content)

### Electron

>[!summary]
>Electron은 JavaScript, HTML, CSS를 사용하여 데스크톱 어플리케이션을 개발하기 위한 오픈소스 프레임워크이다.

특징
1. 웹 기술 기반: HTML, CSS, JavaScript를 사용해서 데스크톱 개발 가능
	1. 크로스 플랫폼: Windows, macOS, Linux 등 여러 운영체제에서 동작하는 애플리케이션을 만들 수 있음.
2. 오픈 소스: Github에서 개발 및 유지 보수되고 있음.
3. Chromium과 Node.JS 기반: 프론트엔드 => Chromium, Node.JS => 백엔드
4. 널리 사용됨: Visual Studio Code, Atom, Discord, Slack, Figma 등 많은 유명 애플리케이션들이 Electron을 사용하여 개발됨

### 웹 기반 크로스 플랫폼

Electron은 HTML,CSS, JS를 이용해서 Windows, Mac OS, Linux 3개의 운영체제와 호환되는 크로스 플랫폼 어플리케이션으로 GIthub 오픈소스이다. 그리고 데스크톱 개발을 웹 개발처럼 개발이 가능하기 때문에 웹 개발자들도 쉽게 데스크톱을 만들 수 있게 되었다. 만약 웹과 데스크톱 애플리케이션 모두를 고려하고 있다면 Electron은 좋은 선택지가 될 수 있다.

### 일렉트론으로 만들어진 어플리케이션


![[Pasted image 20240805204340.png]]

꽤나 들어본 유명한 애플리케이션들이 생각보다 많다.

## Electron의 동작 원리

![[Electron 구조(draw).svg]]

### Main Process

Main Process는 Node.js의 모든 모듈을 사용할 수 있고, 백엔드 영역이다. **package.json의 파일에서 main 스크립트를 실행하는 것이 main process이다.**
중요한 특징 중 하나는 메인 프로세스는 하나만 가질 수 있고, Rederer Process는 여러개 가질 수 있다.

### Renderer Process

Renderer Process는 HTML, JS, CSS로 이루어져 있고 프론트엔드 영역이다. Electron은 웹페이지를 보여주기 위해 내부적으로 Chrominum을 사용하고 있다.  이 때문에 Electron에서는 보여지는 각 웹페이지들은 자신의 프로세스에서만 동작하는데 이를 Renderer Process라 한다. 

### Process간 통신

>[!summary]
>IPC를 사용하며 제공하는 ipc 모듈은 다음과 같다.
>- ipcMain => main -> renderer
>- ipcRenderer => renderer -> main


일렉트론은 Main Process와 Renderer Process의 통신으로 이루어진 프레임워크이다. 그렇기에 IPC를 이용해 통신한다. IPC 모듈은 ipcMain과 ipcRenderer가 존재하며, ipcMain은 메인 프로세스에서 렌더러 프로세스의 비동기 통신을 담당하고 ipcRenderer는 Renderer => main  으로의 비동기 통신을 담당한다. 

ipcMain에서 데이터를 받을 때는 ipcMain.on(channel, listener) 함수를 사용하여 수신하고, 새로운 메시지가 도착하면, listener(event, args)로 호출된다. event.sender.send()를 이용해서 비동기 메시지에 응답할 수 있다.

ipcRenderer에서도 비슷하게 데이터를 받을 때는 ipcRenderer.on(channel, listener)를 호출하며, 데이터를 보낼때는 ipcRenderer.send(channel, args)를 사용한다.

remote 모듈을 이용해서 main process에서만 사용가능한 모듈을 렌더러 프로세스에서도 이용하게 만들 수 있다.

## 질문 & 확장

(없음)

## 출처(링크)

- https://ko.wikipedia.org/wiki/%EC%9D%BC%EB%A0%89%ED%8A%B8%EB%A1%A0_%28%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4_%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC%29
- https://cyberx.tistory.com/206
## 연결 노트
