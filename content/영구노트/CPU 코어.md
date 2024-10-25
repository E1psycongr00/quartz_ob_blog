---
tags:
  - 완성
  - CS
  - CPU
aliases: 
title: CPU 코어
date: 2024-01-13
---
작성 날짜: 2024-01-03
작성 시간: 16:30


----
## 내용(Content)
### 코어
CPU 안에서 일하는 부품 중 하나이다. 예를 들어 CPU 성능이 1코어라면 코어 하나가 여러 연산 작업을 처리한다.

![[코어(draw).svg]]

옛날에는 CPU는 싱글 코어로 충분했고 잘 동작했다.
궁금한 점은 어떻게 작업하는 코어는 하나인데 프로그램을 4개나 띄우고 작업할 수 있을까? 이는 코어가 다른 작업을 수행할 때 매우 빠르게 스위칭해서 다른 작업을 수행하기 때문에 사람 눈에는 여러 작업을 모두 처리하는 것처럼 보인다.

>[!info] CPU
[CPU](https://ko.wikipedia.org/wiki/%EC%A4%91%EC%95%99_%EC%B2%98%EB%A6%AC_%EC%9E%A5%EC%B9%98)(중앙 처리 장치)는 기억, 연산, 제어 3개의 기능을 처리하는 장치이다.  CPU의 성능은 크게 코어 수와 [[CPU 클럭 속도|클럭]]을 통해 결정한다.

>[!info] 마이크로 프로세서
>각종 전자 부품과 반도체 칩을 하나의 작은 칩에 내장한 형태

>[!info] Processor
>프로세서는 컴퓨터 운영을 위해 기본적인 명령어들을 처리하기 위한 논리 회로를 의미. 소프트웨어 신호를 다른 하드웨어로 보내는 제어장치와 사칙연산과 논리 연산을 담당하는 연산장치로 구성된다. 최근에는 프로세서는 CPU와 비슷한 의미로 쓰인다. 

### 싱글 코어의 한계

![[Pasted image 20240103180319.png]]

위 자료는 https://github.com/karlrupp/microprocessor-trend-data?tab=readme-ov-file 의 자료로 50년 동안의 CPU의 동향 데이터이다. 보면 Single 쓰레드 성능과 클럭 속도는 2000년대 까지는 급격히 증가하다가 그 이후로는 꺾인다. 반대로 코어수는 늘어나고 있다.

이런 Trend가 생기는 이유는 CPU의 성능이 올라갈수록 발열 문제가 심각해지기 때문이다. 


![[불타는 CPU(draw).svg|400]]

싱글 코어의 가장 큰 문제는 발열이다. 클럭 속도가 높아질수록 한번에 처리하는 양은 증가하지만 그와 비례해서 발열이 많이 발생한다. 발열이 높으면 오히려 성능이 떨어지고 심각한 경우에는 부품이 손상될 우려가 있다.

### 멀티 코어
트렌드에 경향을 보면 코어 수가 증가함을 알 수 있는데, 멀티 코어를 활용하면 코어끼리 분업하고 소통하면서 전체 성능을 끌어올리고, 클럭을 낮출 수 있다. 이는 발열 문제를 해결하면서 성능을 올릴 수 있는 좋은 방안이 된다. 다만 멀티 코어에 최적화되지 않은 프로그램의 경우 소통에 긴 시간이 걸려 성능이 떨어질 수도 있다.

### 멀티 코어 멀티 쓰레드
멀티 코어에 이어서 인텔과 AMD는 코어가 더 많은 일을 효과적으로 처리하도록 hyper-threading 방식(intel)과 SMT(AMD) 기술을 적용했다.

자세한 내용은 [[CPU 쓰레드(Thread)]]를 참고하자

### 코어에 관련된 영상(지식 거니)


[00:41](https://www.youtube.com/watch?v=_dhLLWJNhwY#t=41.752584194731575): 기본 코어의 정의
[02:47](https://www.youtube.com/watch?v=_dhLLWJNhwY#t=167.721131): 싱글 코어의 한계.. 멀티 코어의 등장
[04:49](https://www.youtube.com/watch?v=_dhLLWJNhwY#t=289.147323): 스레드 시작
[05:55](https://www.youtube.com/watch?v=_dhLLWJNhwY#t=355.283127): 1코어에 2쓰레드??
[09:02](https://www.youtube.com/watch?v=_dhLLWJNhwY#t=542.762696): 코어가 많다고 무조껀 좋은가?






## 질문 & 확장

(없음)

## 출처(링크)
- https://ko.wikipedia.org/wiki/%EC%A4%91%EC%95%99_%EC%B2%98%EB%A6%AC_%EC%9E%A5%EC%B9%98
- https://donghoson.tistory.com/entry/CPU-%ED%94%84%EB%A1%9C%EC%84%B8%EC%84%9C-%EC%BD%94%EC%96%B4-%EA%B0%99%EC%9D%80-%EC%9A%A9%EC%96%B4%EC%9D%B8%EA%B0%80
- https://www.youtube.com/watch?v=_dhLLWJNhwY
- https://github.com/karlrupp/microprocessor-trend-data
## 연결 노트
- [[CPU 클럭 속도]]
- [[CPU 쓰레드(Thread)]]








