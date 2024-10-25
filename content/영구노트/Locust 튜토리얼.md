---
tags:
  - 완성
  - 부하테스트
  - Locust
aliases: 
title: Locust 튜토리얼
date: 2023-12-01
---
작성 날짜: 2023-12-01
작성 시간: 14:07


----
## 내용(Content)
### 설치하기

python 환경을 설치하고 다음과 같이 입력해준다.

```
pip install locust
```

### 사용해보기

locustfile.py 파일을 만들고 다음과 같은 코드를 작성한다.

```python
from locust import HttpUser, task  
  
  
class HelloWolrdUser(HttpUser):  
    @task  
    def hello_world(self):  
        self.client.get("/hello")  
        self.client.get("/world")
```

그리고 terminal에 다음과 같이 입력한다.

```
locust
```

또는 이렇게 명시적으로 파일 이름을 입력해줄 수 있다.

```
locust -f 파일명
```

그리고 localhost:8089에 접속하면 다음과 같은 인터페이스를 볼 수 있다.

![[Pasted image 20231201143922.png]]

start 버튼을 누르면 

![[Pasted image 20231201144001.png]]

위와 같이 통계나 차트 인터페이스를 볼 수 있고 csv를 다운받을 수 있다.



## 질문 & 확장

(없음)

## 출처(링크)
- https://docs.locust.io/en/stable/quickstart.html#locust-s-web-interface

## 연결 노트
- [[Locust 소개]]









