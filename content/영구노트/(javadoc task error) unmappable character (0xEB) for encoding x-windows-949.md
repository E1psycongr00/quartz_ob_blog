---
tags:
  - 완성
  - 솔루션
  - 에러
  - JAVA
aliases: 
title: (javadoc task error) unmappable character (0xEB) for encoding x-windows-949
date: 2023-10-30
---
작성 날짜: 2023-10-30
작성 시간: 20:44


----

## 문제 & 원인

javadoc을 기반으로 docs에서 json을 추출하는 빌드 스크립트를 작성 중 발생한 오류이다.
위 문제는 x-windows-949에서 한글 인코딩이 불가능해서 발생한 오류이다.

Task를 짠 build script는 다음과 같다.

```kotlin
register<Javadoc>("jsonDoclet") {  
    dependsOn("compileJava")  
    source = sourceSets["main"].allJava  
    classpath = sourceSets["main"].compileClasspath  
    destinationDir = javadocDir  
    options.docletpath = configurations["jsondoclet"].files.toList()  
    options.doclet = "capital.scalable.restdocs.jsondoclet.ExtractDocumentationAsJsonDoclet"  
    options.memberLevel = JavadocMemberLevel.PACKAGE  
}
```

![[Pasted image 20231030204409.png]]
![[Pasted image 20231030204435.png]]
## 해결 방안
```kotlin
options.encoding = "UTF-8"  
```
를 추가해주면 인코딩 오류가 발생하지 않는다
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
