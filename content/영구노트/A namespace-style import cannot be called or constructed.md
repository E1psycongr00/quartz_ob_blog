---
tags:
  - 완성
  - 솔루션
  - JS
  - Typescript
  - Error
aliases:
  - import * as 에러
date: 2024-03-22
title: A namespace-style import cannot be called or constructed
---
작성 날짜: 2024-03-22
작성 시간: 22:05


----

## 문제 & 원인
![[Pasted image 20240322221751.png]]


typescript에서 gray-matter를 사용하는데 위와 같은 에러가 떳다.

matter is not a function이라고 에러가 뜨는데 내가 사용했던 코드는 다음과 같다.

```ts
import * as matter from "gray-matter";

const {data, content} = matter(source);
```

## 해결 방안
### import * as 사용하지 않기
>[!summary]
>commonJS 모듈을 import할 때 esModuleInterop를 true로 설정하고 * 사용하지 않고 단순하게 import 하자.


위와 같은 에러가 뜨는 이유는 우선 tsconfig에 `esModuleInterop: true`로 설정한 경우다.
esMoudleInterop는  [typescript - tsconfig 파일의 esModuleInterop 이해 - 스택 오버플로 (stackoverflow.com)](https://stackoverflow.com/questions/56238356/understanding-esmoduleinterop-in-tsconfig-file) 를 참고하자.

import * as matter 를 이용해 import 하는 방식은 옛날 방식이기 때문에 
`import matter from "gray-matter"` 로 import해줘야 한다. 그러면 commonJS 파일로 정상적으로 import 가능하다.

## 질문 & 확장

(없음)

## 출처(링크)
- [typescript - A namespace-style import cannot be called or constructed, and will cause a failure at runtime - Stack Overflow](https://stackoverflow.com/questions/49256040/a-namespace-style-import-cannot-be-called-or-constructed-and-will-cause-a-failu)
- [{ tsconfig.json } 제대로 알고 사용하기 (velog.io)](https://velog.io/@sooran/tsconfig.json-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)
## 연결 노트
