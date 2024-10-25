---
tags:
  - 완성
  - JAVA
aliases: 
title: Java 배포판(distribution) (Oracle JDK vs Open JDK)
date: 2023-10-04
---
작성 날짜: 2023-10-04
작성 시간: 14:33


----
## 내용(Content)
Oracle JDK와 Open JDK 와 같은 자바 배포판의 특성을 이해하기 위해 이 둘을 비교하고,  어떤 배포판은 선택해야 좋을 지 알아보자

### Oracle JDK vs Open JDK
|             | Oracle JDK                           | Open JDK         |
| ----------- | ------------------------------------ | ---------------- |
| 출시 일정   | 3년마다 릴리즈                       | 6개월마다 릴리즈 |
| 라이센스    | Oracle Binary Code License Agreement | GNU GPL          |
| 성능        | 응답성 및 JVM 성능이 뛰어남          | 보통             |

> **Oracle Binary Code License Agreement**
> 상업용 라이센스 없이는 비즈니스, 상업용, 프로덕션 용도로 사용 불가능

> **GNU General Public License**
> 완전히 자유로운 오픈소스 라이센스


Java SE 11 이후로는 둘 간의 기능 및 빌드가 동일하다.  해당 정보는 [오라클 블로그 게시물](https://blogs.oracle.com/java/post/oracle-jdk-releases-for-java-11-and-later) 에 자세히 나와 있다.

### 어떤 JDK를 선택해야 할까

Java SE 11 이후로는 둘 간의 사실상 큰 차이는 없지만 그래도 어떤 JDK를 선택해야 할지 고민해보자

손쉬운 방법은 Oracle JDK를 선택하는 것이지만 항상 그것이 답은 아니다. 비용적인 문제 또는 그 밖에 문제들이 있을 수 있기 떄문이다.

본인이 Linux기반이라면 Linux에 타입에 따라 지원하는 OpenJDK를 선택하는 것이 바람직할 수 있다.  
예를 들면 AWS EC2 인스턴스를 쓰고 Amazon 기반 리눅스를 사용한다면 Amazon Coretto를 사용하는 것이 도움이 될 수 있다. 그 이유는 Amazon에서 제공하는 Open JDK이고 프로덕션 환경의 성능, 보안 및 안정성이 더욱 향상되도록 설계되어 있기 때문이다.

TCK 유무나 LTS(장기 지원) 여부도 중요할 수 있다. 

> **TCK**
> 기술 호환성 키트로 불림
> JSR(Java 사양 요청)의 특정 구현이 준수되었는지 명목상으로 확인하는 테스트 모음

어떤 벤더의 JDK를 쓸지는 해당 위키를 참고 할 수 있다. https://en.wikipedia.org/wiki/OpenJDK

### 완전한 OpenJDK 개요
Rafeal Winterhalter는 OS, 아키텍처, 라이선스, 자원 및 유지 관리 기간을 포함해 사용 가능한 모든 OpenJDK 빌드의 훌륭한 목록을 편집해서 제공한다.

해당 주소에서 확인 가능하다 https://rafael.codes/openjdk/

## 질문 & 확장
-  Java 11 이후로는 Oracle JDK와 Open JDK가 사실상 동일하니, 11 이후 JDK를 쓰는 게 어떤 JDK를 쓰더라도 안정성을 보장할 수 있겠군

## 출처(링크)
- https://namu.wiki/w/Java/%EB%B2%84%EC%A0%84#s-14
- https://www.marcobehler.com/guides/a-guide-to-java-versions-and-features
- https://broccolies.com/2021/04/22/oracle-jdk%EC%99%80-open-jdk%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EB%B9%84%EA%B5%90/
- https://www.baeldung.com/oracle-jdk-vs-openjdk
- https://www.redhat.com/en/topics/application-modernization/openjdk-vs-oracle-jdk
- https://www.openlogic.com/blog/java-experts-openjdk-vs-oracle-jdk
## 연결 노트
- [[무료와 독점 JDK 모음]]









