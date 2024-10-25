---
tags:
  - 완성
  - JAVA
aliases: 
title: JDK Proxy(동적 Proxy)
date: 2023-11-15
---
작성 날짜: 2023-10-26
작성 시간: 22:22


----
## 내용(Content)

### 정적 프록시의 단점

프록시 패턴을 설계해보면 알지만 귀찮은 점이 첫번쨰이고, 사실 프록시는 프레임워크에서 lazy loading 또는 객체를 래핑해서 객체의 기능은 보존하고 부가적인 기능을 지원하기 위해 많이 사용한다. 그런데 정적 프록시만으로는 이러한 요구사항에 한계점이 있기 떄문에 동적 프록시가 필요하다.

### 프록시 종류

- JDK Dynamic Proxy
	- 인터페이스 기반
- CGLib
	- 클래스 기반

### JDK 프록시 사용법

#### 인터페이스와 구현체 정의하기

JDK 프록시는 자바의 Reflection 패키지에 포함되어 있다. 사용하기 위해서는 InvocationHandler를 작성해야 한다.

```java
public interface Hello {  
  
    void sayHello(String name);  
}
```

```java
public class HelloImpl implements Hello {  
    @Override  
    public void sayHello(String name) {  
       System.out.println("Hello " + name);  
    }  
}
```

간단한 인터페이스를 선언해주고 구현체를 하나 생성한다.

#### InvocationHandler 작성하기

```java
public class HelloInvocationHandler implements InvocationHandler {  
  
    private final Object target;  
  
    public HelloInvocationHandler(Object target) {  
       this.target = target;  
    }  
  
    @Override  
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
       System.out.println("This is wrapper");  
       Object result = method.invoke(target, args);  
       System.out.println("hahaha");  
       return result;  
    }  
}
```

invocationHandler를 작성해준다. 여기서 실제 매핑하는 작업이 이루어진다.

#### Proxy 사용하기

```java
public class Main {  
  
    public static void main(String[] args) throws IOException {  
       Hello hello = new HelloImpl();  
       HelloInvocationHandler handler = new HelloInvocationHandler(hello);  
       Hello proxy = (Hello)Proxy.newProxyInstance(  
          Hello.class.getClassLoader(),  
          new Class[] {Hello.class},  
          handler  
       );  
       proxy.sayHello("World");  
    }  
}
```

proxy를 생성할 떄 Proxy.newProxyInstance 메서드를 호출하고, 클래스 로더와 실제 인스턴스와 handler를 이용한다. 그 이후 호출하면 다음과 같은 결과를 얻는다.

>[!test] 테스트 결과
>![[Pasted image 20231112135207.png]]
## 질문 & 확장


## 출처(링크)
- https://velog.io/@codemcd/%EB%8F%99%EC%A0%81-%ED%94%84%EB%A1%9D%EC%8B%9CDynamic-Proxy-with-Spring
- https://velog.io/@newtownboy/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4-%ED%94%84%EB%A1%9D%EC%8B%9C%ED%8C%A8%ED%84%B4Proxy-Pattern

## 연결 노트

- [[프록시 패턴]]









