---
tags:
  - 완성
  - 부하테스트
  - K6
aliases: 
title: k6 Metric
date: 2023-12-07
---
작성 날짜: 2023-12-07
작성 시간: 12:58


----
## 내용(Content)
### K6 매트릭

#### 기본 매트릭

|METRIC NAME|TYPE|설명|
|---|---|---|
|vus|Gauge|현재 활성화된 가상 사용자 수|
|vus_max|Gauge|가능한 최대 가상 사용자 수 (성능에 영향을 주지 않도록 VU 리소스는 미리 할당됨)|
|iterations|Counter|가상 사용자가 JS 스크립트 (기본 함수)를 실행한 총 횟수|
|iteration_duration|Trend|한 번의 완전한 반복을 완료하는 데 걸리는 시간, 설정 및 해체에 소요되는 시간을 포함|
|dropped_iterations|Counter|VU 또는 시간 부족으로 시작되지 않은 반복 횟수|
|data_received|Counter|수신된 데이터의 양|
|data_sent|Counter|전송된 데이터의 양|
|checks|Rate|성공적인 체크의 비율|

#### Http 관련 매트릭(Http 요청할때만 생성)

|METRIC NAME|TYPE|설명|
|---|---|---|
|http_reqs|Counter|k6가 생성한 총 HTTP 요청 수|
|http_req_blocked|Trend|요청을 시작하기 전에 차단된(무료 TCP 연결 슬롯을 기다리는) 시간|
|http_req_connecting|Trend|원격 호스트에 TCP 연결을 설정하는 데 걸린 시간|
|http_req_tls_handshaking|Trend|원격 호스트와 TLS 세션을 핸드셰이킹하는 데 걸린 시간|
|http_req_sending|Trend|원격 호스트에 데이터를 전송하는 데 걸린 시간.|
|http_req_waiting|Trend|원격 호스트의 응답을 기다리는 데 걸린 시간|
|http_req_receiving|Trend|원격 호스트로부터 응답 데이터를 받는 데 걸린 시간|
|http_req_duration|Trend|요청에 걸린 총 시간. (http_req_sending + http_req_waiting + http_req_receiving)|
|http_req_failed|Rate|setResponseCallback에 따른 실패한 요청의 비율|



## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










