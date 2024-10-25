---
tags:
  - 완성
  - 부하테스트
  - Locust
aliases: 
title: Locust Task에 관하여
date: 2023-12-07
---
작성 날짜: 2023-12-07
작성 시간: 12:49


----
## 내용(Content)
### Task

task는 Locust에서 User가 실제로 실행할 동작을 정의해주는 역할을 한다.

@task 어노테이션을 이용해 task 작업을 지정할 수 있다. 여러 태스크를 만들 수 있는데 task의 빈도수(가중치)를 지정하려면 데코레이터에 정수 인자를 넘겨주면 된다.

```python
class MyUser(User):
    wait_time = between(5, 15)

    @task(3)
    def task1(self):
        pass

    @task(6)
    def task2(self):
        pass
```


### TaskSet을 만들고 유저 클래스에서 사용하기
Task가 복잡하거나 Task를 클래스로 나누고 싶은 경우 TaskSet을 활용할 수 있다.  TaskSet을 나누고 유저에 등록하는 방법을 알아보자

#### Nested

```python
from locust import SequentialTaskSet, constant, task, HttpUser  
  
  
class MyTaskSet(HttpUser):  
    @task  
    class MyTasks(SequentialTaskSet):  
  
        @task  
        def print(self):  
            print("hello")  
  
        @task  
        def print2(self):  
            print("world")  
  
    host = "https://mecastudy.com"  
    wait_time = constant(1)  
    tasks = [MyTasks]
```


#### 따로 정의하고 주입하기

```python
from locust import SequentialTaskSet, constant, between, task, HttpUser, TaskSet  
  
  
class MyTasks(TaskSet):  
  
    @task  
    def print(self):  
        print("hello")  
  
    @task  
    def print2(self):  
        print("world")
```


#### SequentialTaskSet
순차적인 task를 정의하고 싶은 경우에 사용한다.

```python
from locust import SequentialTaskSet, constant, between, task, HttpUser  
  
  
class MyTasks(SequentialTaskSet):  
  
    @task  
    def print(self):  
        print("hello")  
  
    @task  
    def print2(self):  
        print("world")  
  
  
class MyTaskSet(HttpUser):  
    host = "https://mecastudy.com"  
    wait_time = constant(1)  
    tasks = [MyTasks]
```


## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










