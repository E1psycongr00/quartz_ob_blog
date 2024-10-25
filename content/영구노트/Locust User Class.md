---
tags:
  - 완성
  - 부하테스트
  - Locust
aliases: 
title: Locust User Class
date: 2023-12-07
---
작성 날짜: 2023-12-07
작성 시간: 12:41


----
## 내용(Content)
### User class

사용자 클래스는 Locust 시뮬레이션시 반드시 필요한 클래스이다. locust에서 동시 사용자 수를 지정할 때 해당 User Class를 기반으로 인스턴스를 생성한다.

User Class가 가지는 특징을 알아보자

#### wait_time 속성
사용자 wait_time 속성을 지정하면 각 작업 실행 후 시간 지연이 발생한다. 따로 wait_time을 지정하지 않으면 작업이 완료되자 마자 다음 작업이 실행된다.

wait_time을 정의하는 방법은 두 가지가 있다.

- constant: 정해진 시간
	- constant_throughput: 작업이 초당 x회 실행되도록 보장하는 적응 시간
	- constant_pacing: 작업이 주기당
- between: 최소값과 최대값 사이의 임의 시간

#### locust에서 사용자 클래스 생성하기

예를 들어 여러 User를 정의한 경우

```python
class WebUser(User):
    weight = 3
    ...

class MobileUser(User):
    weight = 1
    ...
```

```shell
locust -f locust_file.py WebUser MobileUser
```

클래스를 인자를 넘기지 않은 경우는 정의한 모든 클래스를 각각 비슷하게 호출한다. 이 때 weight 속성을 주면 특정 User의 인스턴스를 더 많이 생성한다. 위 코드의 경우 WebUser 클래스가 MobileUser보다 3배 더 높은 확률로 생성된다.


정확하게 갯수를 지정하고 싶다면 fixed_count 속성을 주면 된다

```python
class AdminUser(User):
    wait_time = constant(600)
    fixed_count = 1

    @task
    def restart_app(self):
```

>[!summary]
>- weight은 더 많은 인스턴스를 생성할 확률을 부여함
>- fixed_count는 특별하게 지정된 인스턴스만 생성함

#### host 속성

host속성은 요청할 host를 지정하는 속성이다.

#### on_start(), on_stop()

User 또는 TaskSet은 해당 메서드를 선언해서 실행할 때와 중지될 때의 메서드를 정의 가능하다.
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
- [[Locust 스크립트 문법]]








