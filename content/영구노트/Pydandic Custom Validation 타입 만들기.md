---
tags:
  - 완성
  - 솔루션
  - Python
  - Validation
  - Pydantic
aliases: 
date: 2024-09-28
title: Pydandic Custom Validation 타입 만들기
---
작성 날짜: 2024-09-28
작성 시간: 16:40


----

## 문제 & 원인

Pydandic을 사용하다보면 Default로 제공하는 것 외에 복잡한 로직이 들어가거나 타입을 재활용해야 하는 일이 생긴다. 너무 자주 사용하는 것들은 Pydandic에서 Validation을 제공하지만, 그렇지 않은 경우는 어떻게 해결 할 수 있는 지 방법을 살펴보자.

## 해결 방안
### 직접 선언하기

직접 선언하는 방식은 재활용 할 수는 없지만 class를 한 눈에 보기 쉽게 파악할 수 있는 장점이 있다.
#### @field_validator 사용하기

```python
class User(BaseModel):
    id: int
    name: str
    age: int

    @field_validator("age")
    def validate_age(cls, v):
        if v < 0:
            raise ValueError("나이는 무조껀 양수여야 합니다")
        return v

```

![[Pasted image 20240928164818.png]]

이렇게 validation을 잘 수행함을 알 수 있다. Pydantic을 활용하면 Pydantic_core.ValidationError로 덮어서 출력한다. 이 코드의 장점은 User라는 클래스에 Validator가 함꼐 있기 때문에 가독성이 높다는 점이다. 그러나 Field가 많다면 코드가 지저분해질 수 있는 단점이 있다. 재활용 또한 불가능하다.

#### Field에 직접 선언하기

```python
class User2(BaseModel):
    id: int
    name: str
    age: Annotated[int, Field(title="나이", description="나이는 무조껀 양수여야 합니다", gt=0)]
```

보통 Field는 fastapi와 함께 해당 필드가 무엇인지 문서화를 쉽게 하기 위해서 정의하는 타입이다. 간단한 커스텀 타입의 경우, 공식문서에서는 Field를 사용하기를 추천한다. 안에 포함되어 있는 annotated_types를 활용해서 Annotated 타입 안에서 Validation을 수행할 수 있도록 Pydantic이 지원하기 때문에 편하기 Validation을 활용할 수 있다.

### 타입 선언하기

직접 커스텀 타입으로 선언하면 쉽게 재활용 가능하다

#### Annotated 타입 선언하기

```python
Age = Annotated[
        int, Field(title="나이", description="나이는 무조껀 양수여야 합니다", gt=0)
    ]

class User2(BaseModel):
    id: int
    name: str
    age: Age

```

물론 이 방법은 에러 메시지를 직접 커스텀 할 수는 없다. 그러나 재활용할 수 있다는 장점이 있다.

엄청 복잡한 로직의 Validation은 불가능하지만, 크기, 길이, 패턴 등 핵심적으로 많이 사용하는 Validation은 지원하기 때문에 빠르게 재활용 가능하면서 개발이 가능하다. 또 Field로 사용되기 때문에 문서화할 수 있는 장점이 있다.

#### 직접 커스텀 클래스 선언하기

완전한 커스텀 타입 클래스를 제작 가능하다. 그러나 코드가 조금 길고 class 선언 시에 3개의 class method를 정의해준다.

```python
from typing import TYPE_CHECKING, Annotated, Any

from pydantic import GetCoreSchemaHandler, GetJsonSchemaHandler
from pydantic.json_schema import JsonSchemaValue
from pydantic_core import CoreSchema, PydanticCustomError, core_schema



def validatePositiveInt(value: int) -> int:
    if value <= -2:
        raise PydanticCustomError(
            "value_error", "-2 보다 큰 정수를 입력해주세요!!!!", {"reason": value}
        )
    return value


if TYPE_CHECKING:
    MyClassPositiveInt = Annotated[int, ...]
else:

    class MyClassPositiveInt:
        @classmethod
        def __get_pydantic_core_schema__(
            cls, source_type: Any, handler: GetCoreSchemaHandler
        ) -> CoreSchema:
            return core_schema.no_info_after_validator_function(
                cls._validate, core_schema.int_schema()
            )

        @classmethod
        def __get_pydantic_json_schema__(
            cls, core_schema: CoreSchema, handler: GetJsonSchemaHandler
        ) -> JsonSchemaValue:
            field_schema = handler(core_schema)
            field_schema.update(type="integer", format="integer")
            return field_schema

        @classmethod
        def _validate(cls, input_value: int, /) -> int:
            return validatePositiveInt(input_value)
```

코드가 긴데 코드를 분석해보면 만약 TYPECHECKING을 수행한다면 내가 선언한 타입은 Annotated 타입으로 선언한다. 이러면 정적 분석에서 MyClassPositiveInt 타입은 int 타입으로 인식한다.

그리고 타입 체킹이 아닌 실제 타입 인스턴스가 생성되는 경우 class 타입을 정의해준다.

```python
        @classmethod
        def __get_pydantic_core_schema__(
            cls, source_type: Any, handler: GetCoreSchemaHandler
        ) -> CoreSchema:
            return core_schema.no_info_after_validator_function(
                cls._validate, core_schema.int_schema()
            )
```

pydantic의 핵심 스키마를 정의한다. 

core_schema 의 `no_info_after_validator_function`은 validate function 인수와 어떤 스키마로 정의할지 인수를 받는다. 이 함수의 역할은 raw한 원시 타입 생성 이후에 타입 검증을 수행한다.

`no_info_after_validator_function` 과 `no_info_before_validator_function`의 차이점을 알아보자.

**no_info_after_validator_function** 
- 기본 타입 검증 이후에 실행.
- 이미 기본 타입으로 변환된 값을 받아 처리.
- 주로 추가적인 비즈니스 로직 검증에 사용.

**no_info_before_validator_function**
- 기본 타입 검증 이전에 실행.
- 원시 입력값(raw input)을 받아 처리.
- 주로 데이터 정제나 사전 처리에 사용.

예시는 다음과 같다.

```python
def before_validator(value: Any) -> Any:
    if isinstance(value, str):
        return value.strip()  # 문자열 앞뒤 공백 제거
    return value

def after_validator(value: int) -> int:
    if value <= 0:
        raise ValueError("양수만 허용됩니다")
    return value

# 사용 예
schema = core_schema.int_schema(
    before_validator_function=before_validator,
    after_validator_function=after_validator
)
```

아래의 `__get_pydantic_json_schema__` 는 json 변환시 정의할 타입이며, `_validate`는 `__get_pydantic_core_schema__`에서 schema 정의시 validation을 위한 function이다.

```python
def validatePositiveInt(value: int) -> int:
    if value <= -2:
        raise PydanticCustomError(
            "value_error", "-2 보다 큰 정수를 입력해주세요!!!!", {"reason": value}
        )
    return value

```

raise ValueError를 써도 되고, PydanticCustomError를 써도 된다. 좀 더 구체적인 에러메시지를 남기고 싶을 때 좋다.
## 질문 & 확장

(없음)

## 출처(링크)

- https://docs.pydantic.dev/latest/
- 
## 연결 노트
