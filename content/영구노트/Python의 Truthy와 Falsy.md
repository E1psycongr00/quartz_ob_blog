---
tags:
  - 완성
  - Python
aliases: 
date: 2024-09-26
title: Python의 Truthy와 Falsy
---
작성 날짜: 2024-09-26
작성 시간: 12:22


----
## 내용(Content)

### Truthy와 Falsy

>[!summary]
> 특정 value를 평가할 때 True인지 False 인지 판단하는 기준, Python에서는 if문과 단축 평가에서 판단 기준으로 쓰인다.

python에서 어떤 변수가 Truthy한지 Falsy한지 판단할 때, 변수에 할당된 객체 상태가 `__bool__()` 함수를 적용했을 때 `False`가 나오거나 `__len__()` 결과가 `0` 이 나오는 경우 Falsy하다고 한다.

### Falsy한 자료 상태

1. False
2. None
3. 0 (정수)
4. 0.0 (부동소수점)
5. 0j (복소수)
6. "" (빈 문자열)
7. \[\] (빈 리스트)
8. {} (빈 딕셔너리)
9. () (빈 튜플)
10. set() (빈 집합)
11. range(0) (빈 범위)

### Truthy한 자료 상태

1. True
2. 0이 아닌 모든 숫자 (예: 1, -1, 3.14)
3. 비어있지 않은 문자열
4. 비어있지 않은 리스트, 튜플, 딕셔너리, 집합
5. 사용자 정의 객체 (기본적으로)


### 정리

1. 빈 컨테이너(리스트, 딕셔너리, 문자열 등)는 Falsy이다.
2. 0을 제외한 모든 숫자는 Truthy 이다.
3. None은 Falsy이다.
4. 사용자 정의 클래스의 인스턴스는 기본적으로 Truthy이지만, **bool**() 또는 **len**() 메서드를 정의하여 이 동작을 변경할 수 있다.
## 질문 & 확장

(없음)

## 출처(링크)

- https://testdriven.io/tips/ba9f859e-ab3d-4ff5-bc44-aebf70b13260/
- https://www.geeksforgeeks.org/truthy-vs-falsy-values-in-python/
## 연결 노트

- [[Truthy or Falsy]]
