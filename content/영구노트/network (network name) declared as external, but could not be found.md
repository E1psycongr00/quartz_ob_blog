---
tags:
  - 완성
  - Docker
  - Errror
aliases: 
title: network (network name) declared as external, but could not be found
date: 2023-12-18
---
작성 날짜: 2023-12-18
작성 시간: 12:59


----

## 문제 & 원인
```yaml
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
      - INFLUXDB_ADMIN_USER_PASSWORD=bitnami123
      - INFLUXDB_ADMIN_USER_TOKEN=admintoken123
      - INFLUXDB_HTTP_AUTH_ENABLED=false
      - INFLUXDB_DB=myk6db
  
  grafana:
    image: bitnami/grafana:latest
    container_name: grafana
    ports:
      - "3005:3005"
    networks:
      - monitoring
    depends_on:
      - influxdb
  
networks:
  monitoring:
    external: true
```

이렇게 작성하는데 다음과 같은 에러 문구가 나온다.

**network (network name) declared as external, but could not be found**

## 해결 방안
위의 문제가 발생하는 이유는 docker 로 네트워크를 실제로 생성하지 않았기 때문에 찾을 수 없어 발생하는 문제이다. 기본적으로 네트워크를 지정하지 않으면 자체적으로 네트워크를 생성해서 docker-compose 끼리 연결하지만 networks를 기입하면 새로 생성하는 것이 아닌 기존의 네트워크를 가져와서 활용한다.

우선 네트워크가 존재하는지 확인해본다

```bash
docker network ls
```

![[Pasted image 20231218130440.png]]

위와 같이 network 정보가 나온다. 지금은 network를 생성했기 때문에 monitoring이라는 name이 있지만 만약 생성하지 않았다면 없을 것이다.

network 를 생성하는 방법은 다음과 같다.

```bash
docker network create monitoring
```

그 이외의 명령어는 아래를 참고해서 사용하면 되겠다. docker network만 입력해도 나온다

![[Pasted image 20231218130631.png]]
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
