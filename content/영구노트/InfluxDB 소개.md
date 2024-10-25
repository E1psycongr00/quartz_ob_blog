---
tags:
  - 완성
  - DataBase
aliases: 
title: InfluxDB 소개
date: 2023-12-15
---
작성 날짜: 2023-12-15
작성 시간: 12:54


----
## 내용(Content)

- InfluxDB는 Time Series Data를 처리하기 위해 고안된 TSDB 중 하나이다. 
- 2013년에 Go 언어로 개발된 오픈소스 DB이다. 
- InfluxDB는 Tick Stack (Telegrapf, InfluxDB, CronoGraf, Kapacitor)의 필수 컴포넌트 중 하나로 쓰인다. 
- 분산환경과 스케일링을 지원하며 Restful API를 제공해서 API 통신이 가능하다.

![[Pasted image 20231215163308.png]]

TICK Stack 기반으로 구축한 시스템

- Telegraf는 Metrics와 Events를 수집
- InfluxDB는 TimeSeries Data를 관리
- Kapacitor: Real - time 스트리밍 전송 엔진
- Chronograf: 시각화 도구

### InfluxDB가 제공하는 핵심 기능

- 일정 주기마다 자동으로 새롭게 데이터를 저장
- 일정 주기마다 자동으로 오래된 데이터를 삭제
- NoSQL과 같이 스키마가 따로 존재하지 않기 때문에 다른 시계열 컬럼의 경우 자동으로 추가되고 유연하게 대처 가능

### InfluxDB와 RDB 용어 비교

| RDB               | Influx DB              |
| ----------------- | ---------------------- |
| column            | key                    |
| table             | measurement            |
| database          | database               |
| row               | point                  |
| indexed columns   | tags(string type only) |
| unindexed columns | fields                 |

#### Measurement

RDB의 테이블과 매칭되는 키워드이다.  RDB에서 여러 테이블을 가질 수 있는 것과 같이 InfluxDB에서도 여러 개의 measurement를 가질 수 있다.

#### Key(tag, field, time)

컬럼의 구성은 RDB와 다르게 차이점이 있다. RDB에서는 한개의 컬럼에 한 개의 데이터가 들어가는데, TSDB에서는 컬럼이 3가지 종류로 나뉘고, 각각의 컬럼은 (Key, Value) 형식으로 들어가게 된다.

>[!info] Tag Key
>- RDB에서 Indexed Column과 유사하며, 인덱싱되어 Select문으로 조회하는 기준이 된다.
>- Tag의 value로는 String 타입만 가능하므로 질의 시에 따옴표로 ' ' 로 감싸주어야 한다


>[!info] Field Key
>- RDB에서는 Indexed되지 않는 Column과 유사하며, 저장되는 데이터가 최소 1개이상의 field가 있어야 한다
>- Field value는 strings, floats, integer, boolean 이 가능하다

>[!info] Time Key
>- Unix Time을 기준으로 환산된다






## 질문 & 확장

(없음)

## 출처(링크)
- https://mangkyu.tistory.com/190
- https://squareyun.tistory.com/90
## 연결 노트
- [[TSDB]]
- [[Unix Time]]








