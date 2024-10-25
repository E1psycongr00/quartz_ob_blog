---
tags:
  - 완성
  - Spring
  - Validation
  - Error
aliases: 
title: ConstraintViolationException
date: 2023-12-07
---
작성 날짜: 2023-12-07
작성 시간: 14:25


----
## 내용(Content)

### 발생 시점
ConstraintViolation은 java bean validation의 validator에서 유효성 검사 결과가 fail인 경우 발생하는 예외이다. 

Spring에서는 컨트롤러 외에 @Validated가 붙은 bean의 경우 유효성 검사 중 발생하는 예외라 할 수 있겠다.

### ConstraintViolation
해당 예외는 ConstraintViolation 객체를 Set 형태로 저장한다.

web에서 예외가 발생시 저장하는 BindResult와는 다르게 ConstraintViolation은 code정보가 없기 때문에 만약 MessageSource와 연동하고자 하면 직접 추출해야 한다.

ConstraintViolation에서는 대략 다음의 정보들을 얻을 수 있다.

- 유효성 검사에 수행된 **어노테이션 정보**
- **RootBean과 LeafBean 경로**
- 검사한 **property의 path**
	- 파라미터에 validation을 적용한 경우 `메서드이름.target타입이름` 이런식으로 출력된다.
	- ex) Controller의 findItem에서 Id를 검사하는 경우 -> findItem.id
- invalidValue를 얻어올 수 있다.

#### ConstraintDescriptor
getConstraintDescriptor 메서드를 통해 타입을 호출할 수 있다.

descriptor는 다음과 같은 메서드들을 가지고 있다.

- getAnnotation()
- getAttributes()

위의 정보는 어노테이션을 얻어오고 Attribute로 부터 Annotation에 지정된 인자를 인덱스 순서로 가져온다. 

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










