---
tags:
  - 완성
  - 솔루션
  - Python
  - 단축평가
aliases: 
date: 
title: Python에서 Optional한 데이터 잘 활용하기
---
작성 날짜: 2024-09-26
작성 시간: 12:37


----

## 문제 & 원인

```python
os.getEnv()
```

이런 function들을 보면 리턴값이 `str | None` 형태이다. 이 형태는 python 3.10+에서 union 타입 형태로 위와 같은 형태는 `Optional[str]`과 동일하다.


## 해결 방안

### 전통적인 방법

```python
def func(x: str | None):
	if x is None:
		return ""
	return x
```

모든 프로그램에 기본이 되는 방법으로 이해하기 가장 쉽지만 반대로 코드가 길어지고 라인이 늘어지니 코드가 많아 보이는 문제점이 있다.

### 삼항 연산자

```python
def func(x: str | None) -> str:
    return "" if x is None else x
```

이 방법도 짧게 쓰는 경우 삼항 연산자를 활용한다.

그러나 3항 연산자 방법은 에러를 발생시키고 싶을 때는 조금 어렵다.

### 단축 평가 활용하기

```python
def func(x: str | None):
	return str(x or "") # x or ""
```

[[단축 평가 계산|단축 평가]]를 활용하면 쉽게 str 타입으로 반환할 수 있다.

여기서 `str(x or "")`을 사용하면 x 가 만약 truthy하지 못하다면 빈 문자열을 출력하고, truthy하다면 문자열 x를 리턴하도록 한다. 만약 타입이 확실히 문자열이라면 str 대신 `x or ""`로 써도 무방하다.

### 에러로 처리하고 싶은 경우

#### 전통적인 방법

```python
def func(x):
	if x is None:
		raise ValueError("None 안되용")
	return x
```

#### 제너레이터 표현식 트릭을 이용한 예외처리

```python
def func(x: str | None):
	return (_ for _ in ()).throw(ValueError("x가 Falsy하네")) if x is None else ""
```

raise는 state문이기 때문에 lambda expression같은 단일 표현식에서 사용할 수 없다. 그렇기 때문에 빈 제너레이터를 이용한 트릭으로 예외발생 트릭을 만든다. 단축 평가에서도 이와 비슷하게 사용가능하다. 그러나 단축 평가는 None 뿐만 아니라 빈 문자열("")도 Falsy하기 때문에 조심해서 사용해야 한다.

>[!caution]
>빈 제너레이터 트릭은 JS의 IIFE와 비슷한 느낌이 들어서 좋아 보이지만 사실 잘 쓰지 않는 트릭일 뿐만 아니라 코드의 가독성을 해칠 우려가 있다.
>그래서 신중하게 사용해야하고 팀원과 같이 프로젝트를 진행하는 경우 충분한 대화가 필요하다.

>[!tip]
>`(_ for _ in ()).throw(ValueError("x가 Falsy하네"))`는 빈 튜플을 이용해 제너레이터 표현식을 표현하고 throw을 던지는 트릭을 활용한 것이다.

### 결론

예외 처리를 해야 하는 경우에는 if문을 쓰는 것이 가장 안전하며, 특정 타입과 default 타입을 지정해 줄 수 있는 경우 `단축 평가`를 사용하면 코드를 짧으면서도 이해하기 쉽게 짤 수 있다.


## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트

- [[단축 평가 계산]]