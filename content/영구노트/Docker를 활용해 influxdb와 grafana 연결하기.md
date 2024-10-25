---
tags:
  - 완성
  - Docker
  - Grafana
  - 인프라
aliases: 
title: Docker를 활용해 influxdb와 grafana 연결하기
date: 2023-12-18
---
작성 날짜: 2023-12-18
작성 시간: 12:19


----
## 내용(Content)
### influxdb와 grafana를 연결하는 이유
influxdb는 TSDB로 대규모 시계열 데이터를 처리하는데 특화된 Database이다. 시계열 데이터에 대한 쿼리나 데이터 삽입의 편의성을 제공하지만, 단점은 시각화를 지원하지 않는다. Grafana는 공식적으로 InfluxDB에 대한 시각화 대시보드를 제공한다

### Docker를 이용해 인프라 구축하기

Docker를 활용하면 로컬 환경, 배포 환경에 상관없이 구축 가능하기 때문에 매우 편리하다.  Docker를 이용해 InfluxDB와 Grafana 이미지를 생성하고 인프라를 구축해보자

#### docker-compose를 이용해 이미지 생성하기

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
      - INFLUXDB_ADMIN_USER_PASSWORD=bitnami123
      - INFLUXDB_ADMIN_USER_TOKEN=admintoken123
      - INFLUXDB_HTTP_AUTH_ENABLED=false
      - INFLUXDB_DB=myk6db
    volumes:
      - ./influx-persistance:/bitnami/influxdb
  grafana:
    image: bitnami/grafana:latest
    container_name: grafana
    ports:
      - "3005:3000"
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

docker container들을 편리하게 생성 및 관리하기 위해 docker-compose를 작성해준다.

influxdb image 설정에 대한 자세한 정보는 https://hub.docker.com/r/bitnami/influxdb 를 참고하자

만약 network 관련 에러가 발생한다면 [[network (network name) declared as external, but could not be found]] 를 참고하자.

docker-compose ps 명령어를 통해 container가 잘 생성 됬는지 확인하자

![[Pasted image 20231218130824.png]]


grafana의 정보를 유지하고 싶다면 /var/lib/grafana를 바인드 마운트하면 된다.

#### grafana에 접속해 influxdb 연결하기

3000 포트를 3005로 포트포워딩을 했기 때문에 localhost:3005로 접속해보자. 그럼 다음과 같은 화면을 만난다.

![[Pasted image 20231218131211.png]]

admin/admin을 입력하자

![[Pasted image 20231218131256.png]]

들어가서 InfluxDB를 누르면 다음과 같은 창이 뜬다

![[Pasted image 20231218131424.png]]

우리는 Docker로 생성했기 때문에 URL에 Docker container 명을 입력한다. 우리는 InfluxDB로 생성했기 때문에 http://influxdb:8086 으로 입력해주면 된다.

InfluxDB 1.8+ ~ 2.0 버전부터는 InfluxQL을 안쓰기 때문에 Flux로 설정하고 2가지 정보를 입력해야 한다.

Organization과 token 2개만 입력해주면 연결이 완료된다.

![[Pasted image 20231218132409.png]]


#### token과 organization 얻어오기
organization은 influxdb 내부에 접속해서 쉽게 알 수 있다.

```bash
docker-compose exec -it influxdb /bin/bash
```

이렇게 하면 influxdb 컨테이너의 bash에 접근 가능하고 내부에서 명령어를 상호작용 할 수 있다.

```
influx org list --token admintoken123
```

아까 docker-compose에서 설정한 토큰을 입력하면 다음과 같이 토큰 ID와 네임이 나온다.

![[Pasted image 20231218132210.png]]

여기서 ID를 Organization에 넣어주면 된다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://velog.io/@makengi/InfluxDB-InFluxDB-Java-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8
- https://2juhyunju.tistory.com/46

## 연결 노트
- [[InfluxDB 소개]]
- [[network (network name) declared as external, but could not be found]]

