---
tags:
  - 완성
  - IT
  - 네트워크
aliases:
  - web socket
title: Web Socket이란
date: 2023-10-07
---
작성 날짜: 2023-10-07
작성 시간: 17:21



----
## 내용(Content)
### Web Socket 
- 웹소켓은 TCP 접속을 통해 실시간 양방향 통신을 제공하는 컴퓨터 통신 프로토콜이다.
- 웹 소켓은 IETF에 의해 RFC 6455로 표준화 되었으며 웹소켓 API는 W3C에 의해 표준화되었다.

### 특징
- HTTP 프로토콜과 마찬가지로 OSI 7계층에 위치하고 있고 4계층의 TCP에 의존한다. 웹소켓은 80과 443 포트 위에 동작하도록 설계되어 있다.
- HTTP Polling과 같은 반이중통신 방식에 비해 더 낮은 부하를 사용하여 웹 브라우저와 웹 서버간의 통신을 가능하게 하며, 서버와 실시간 통신에 유리하다.
- 웹 소켓 프로토콜 사양은 2개의 새로운 URI 스킴으로 ws, wss를 정의하며 이들은 암호화되지 않은 연결과 암호화된 연결에 각각 사용된다.

### 동작 과정
![[웹소켓 동작 과정(draw).svg]]

### 프로토콜 핸드 쉐이크
**Client 요청**
```text
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
Origin: http://example.com
```


**Server 응답**
```text
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
Sec-WebSocket-Protocol: chat
```

## 질문 & 확장

(없음)

## 출처(링크)
- https://code-lab1.tistory.com/300
- https://appmaster.io/ko/blog/websocketiran-mueosimyeo-eoddeohge-saengseonghabnigga
- https://ko.wikipedia.org/wiki/%EC%9B%B9%EC%86%8C%EC%BC%93
- https://velog.io/@codingbotpark/Web-Socket-%EC%9D%B4%EB%9E%80
## 연결 노트
- [[Polling(폴링)]]









