---
tags:
  - 완성
  - Docker
  - DataBase
aliases: 
title: k6와 docker image로 만들어진 influxdb v2 연결하기
date: 2023-12-21
---
작성 날짜: 2023-12-21
작성 시간: 17:37


----
## 내용(Content)
### influxdb v2
influxdb v2는 influxdb v1과 달리 web ui로 손 쉽게 상호 작용이 가능하고,  좀 더 쉬운 쿼리를 제공한다.  web ui 제공은 dashboard도 제공하기 때문에 데이터 시각화도 Flux와 함께 표현 가능하다.

[Influx v2 공식 사이트](https://docs.influxdata.com/influxdb/v2/)

### docker compose 로 influxdb v2 build하기
```yml
version: "3.7"

services:
  influxdb:
    image: bitnami/influxdb:latest
    container_name: influxdb
    ports:
      - "8086:8086"
    networks:
      - monitoring
    environment:
      - INFLUXDB_ADMIN_USER_PASSWORD=admin123
      - INFLUXDB_ADMIN_USER_TOKEN=admintoken123
      - INFLUXDB_HTTP_AUTH_ENABLED=false
      - INFLUXDB_DB=myk6db
    volumes:
      - ./influx-persistance:/bitnami/influxdb
  grafana:
    image: bitnami/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    networks:
      - monitoring
    depends_on:
      - influxdb
    volumes:
      - ./data:/var/lib/grafana
  
networks:
  monitoring:
    external: true
```

bitnami는 최신 influxdb를 간단하게 image로 빌드가 가능하다. 

### windows에서 xk6 build하기

k6 만으로는 Influxdb v2에 접근할 수 없다. extend 버전이 필요하다.
[xk6 build하고 사용하기](https://k6.io/docs/results-output/real-time/influxdb/)

```bash
# Install xk6
go install go.k6.io/xk6/cmd/xk6@latest

# Build the k6 binary
xk6 build --with github.com/grafana/xk6-output-influxdb
```

이것은 Go를 이용해 xk6 바이너리 파일을 빌드하는 것이다. 당연히 Go가 설치되어 있어야 한다.

Windows의 경우에 go install은 $GOPATH 에 package가 설치된다.
![[Pasted image 20231221174904.png]]

그리고 xk6를 build하면 실행할 로컬 디렉토리에 확장된 k6 binary 파일이 생긴다.

![[Pasted image 20231221175050.png]]

![[Pasted image 20231221175122.png]]


### k6 스크립트를 실행하고 influxdb에 데이터 전달하기

script를 작성하고 실행한다. 여러 변수들을 전달해야 하기 때문에 windows 스크립트를 짯다

```bat
set K6_INFLUXDB_ORGANIZATION=35f0fc21b269d98e
set K6_INFLUXDB_BUCKET=load_test
set K6_INFLUXDB_TOKEN=admintoken123
set K6_INFLUXDB_ADDR=http://localhost:8086

k6 run script.js -o xk6-influxdb
```

정상적으로 실행되면 다음과 같은 결과를 얻는다

![[Pasted image 20231221175431.png]]

이건 결과고 실행되는 동안에도 실시간으로 결과를 influxdb에서 얻을 수 있다.

### InfluxDB v2 DashBoard에서 데이터 확인하기

flux를 이용해 dashboard에 데이터를 시각화 가능하다.

localhost:8086에 접속하자
![[Pasted image 20231221175626.png]]
위와 같은 로그인 화면이 나타난다.  로그인 하자.

![[Pasted image 20231221175725.png]]

DashBoard에 들어가면 기존에 만들었던 dashboard를 import도 가능하나, 우리는 새로운 DashBoard를 구성해보자

![[Pasted image 20231221175900.png]]

각 데이터를 보여주는 네모난 칸들은 Cell이다. Add Cell를 통해 Cell을 추가하려고 하면

![[Pasted image 20231221175934.png]]

다음과 같은 화면이 뜨는데 어떻게 쿼리할 건지 정해주면 된다. SCRIPT EDITOR를 누르면 스크립트를 직접 짤 수도 있다.
## 질문 & 확장


(없음)

## 출처(링크)
- https://docs.influxdata.com/platform/
- https://k6.io/docs/results-output/real-time/influxdb/
- https://velog.io/@heka1024/Grafana-k6%EC%9C%BC%EB%A1%9C-%EB%B6%80%ED%95%98-%ED%85%8C%EC%8A%A4%ED%8A%B8%ED%95%98%EA%B8%B0

## 연결 노트

- [[InfluxDB 소개]]
- [[Docker를 활용해 influxdb와 grafana 연결하기]]









