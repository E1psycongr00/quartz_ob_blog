---
tags:
  - 완성
  - JAVA
aliases: 
date: 2024-10-06
title: JAVA Text Block
---
작성 날짜: 2024-10-06
작성 시간: 20:48


----
## 내용(Content)

### Text Block

>[!summary]
>java 17에서 새로 추가된 문법으로 `""" """` 이렇게 사용 가능하다.`\n`을 인식해서 텍스트를 처리하기 때문에 문자열을 블럭 단위로 유연하게 처리할 수 있다.

JAVA로 JSON 데이터를 문자열 형태로 표현하려면 여러 indent와 줄바꿈이 포함된 text block을 정의해야 할 수 있다. 기존의 String으로 이를 정의하려면 매우 불편하지만 Text Block 문법을 이용해서 쉽게 해결할 수 있다.

```java
public class App {
    public String getGreeting() {
        return "Hello World!";
    }

    public static void main(String[] args) {
        String stringTextBlokJson = 
                """
                {
                    "name": "현규",
                    "age": 22
                }
                """;
        String stringJson = "{\n" 
                + "    \"name\": \"현규\"\n"
                + "    \"age\": 22\n"
                + "}";
        
        System.out.println();
        System.out.println(stringTextBlokJson);
        System.out.println(stringJson);
    }
}
```

### indent 를 잘 활용해라

textBlock의 문제점은 의도치 않게 indent가 추가되거나 제거될 수 있다는 것이다. 그래서 text block를 의도에 맞게 indent를 제거하거나 사용하도록 할 수 있어야 한다.

![[Pasted image 20241006210845.png]]


indent를 어떻게 가져가느냐에 따라서 text Block의 indent가 달라질 수 있다. 가장 중요한 것은 닫을 때 `"""` indent 위치 기준으로 얼마나 indent을 지울지 결정한다. 위 그림을 보면 마지막에 추가적인 indent를 주기 위해서 `"""` indent를 줄여서 앞에다가 쓰고 그렇게 쓰면 기존 textblock 에 기존 textblock indent - 닫힌 `"""` indent 만큼의 남은 indent가 추가로 출력된다.


## 질문 & 확장

(없음)

## 출처(링크)

- https://howtodoinjava.com/java14/java-text-blocks/

## 연결 노트










