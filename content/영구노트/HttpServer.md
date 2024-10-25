---
tags: 
  - 완성
  - IT
  - JAVA
aliases: 
title: HttpServer
date: 2023-11-06
---
작성 날짜: 2023-11-06
작성 시간: 16:45

 #완성 #IT #JAVA 

----
## 내용(Content)

### HttpServer란
**com.sun.net.httpserver** 패키지에서 제공하는 클래스이다. 자세한 설명은 javadocs에 다음과 같이 적혀있다.

> This class implements a simple HTTP server. **A HttpServer is bound to an IP address and port number and listens for incoming TCP connections from clients on this address.** The sub-class HttpsServer implements a server which handles HTTPS requests.

핵심은 HttpServer는 IP 주소와 포트 번호로 바인딩되어 있고(묶여 있음) 클라이언트로부터 들어오는 TCP 연결을 수신한다고 볼 수 있다.

### HttpServer 사용법

> **One or more HttpHandler objects must be associated with a server in order to process requests.** Each such HttpHandler is registered with a root URI path which represents the location of the application or service on this server. The mapping of a handler to a HttpServer is encapsulated by a HttpContext object. **HttpContexts are created by calling createContext(String, HttpHandler)**. **Any request for which no handler can be found is rejected with a 404 response.** Management of threads can be done external to this object by providing a Executor object. If none is provided a default implementation is used.

특정 요청에 대해서 처리하려면 HttpHandler를 서버와 연결해야 한다.  createContext(String, HttpHandler) 메서드를 사용해서 경로와 요청을 처리하는 로직을 담당하는 Handler와 server를 uri 경로와 함께 연결할 수 있다.

Executor를 통해 쓰레드를 관리할 수 있고, 만약 특별한 입력이 없다면 기본 구현된 방법으로 쓰레드를 관리한다.


### Code

```java
import java.io.IOException;  
import java.net.InetSocketAddress;  
  
import com.sun.net.httpserver.HttpServer;  
  
public class Main {  
  
    public static void main(String[] args) throws IOException {  
       int port = 8080;  
       // 1. server 생성
       HttpServer server = HttpServer.create(new InetSocketAddress("0.0.0.0", port), 0);  
       // 2. uri path와 handler 맵핑
       server.createContext("/", new CheckHandler());  
       server.createContext("/sum", new SumHandler()); 

	   // 쓰레드 관련, 아무 입력도 없는 경우 기본 구현으로 처리
       server.setExecutor(null);  

	   // 서버 시작
       server.start();  
  
    }  
}
```
Ip와 port 인자를 활용해 InetSocketAddress 객체를 생성하고 이를 이용해 HttpServer를 생성한다.

## 질문 & 확장

(없음)

## 출처(링크)
- Javadocs

## 연결 노트
- [[HttpHandler]]









