---
tags:
  - 완성
  - JS
  - Typescript
  - Bundler
  - esbuild
aliases: 
date: 2024-04-05
title: esbuild 사용해보기
---
작성 날짜: 2024-04-05
작성 시간: 15:41


----
## 내용(Content)
### esbuild
>[!summary]
>esbuild는 Go 언어로 개발된 빠르게 build 가능한 번들러 프로젝트이다.

전통적인 js 번들러 도구는 상대적으로 느린 빌드 시간을 가지는데 esbuild는 굉장히 빠르게 빌드 가능하다.

공식 홈페이지에서도 빠른 빌드 시간을 장점으로 내세우고 있다. 

![[Pasted image 20240405160206.png]]

아직 현재 시간 기준(2024-04-05) 기준으로 공식 버전(1.0.0)이 나오지 않았기 때문에 완전히 안정적이라 볼 수도 없고 기능도 제한적일 수 있다. 그러나 최근 사람들이 빠른 빌드 속도와 편리함으로 인해 esbuild를 번들러를 활용한 포스팅이 꽤나 많이 보인다.

### esbuild 번들링
[공식 홈페이지 format](https://esbuild.github.io/api/#format)에서 format을 통해 어떤 목적으로 bundling 할지 선택할 수 있다. 그러나 쉽게 사용하는 방법은 [공식 홈페이지 platform](https://esbuild.github.io/api/#platform)을 활용하면 된다.

platform은 3가지를 제공한다.

- browser: default format
- node cjs: 형태로 출력되며, es6의 import/export 문법은 자동으로 변환됨
- neutral: ESM 형태로 출력된다.

esbuild 사용 예시는 다음과 같다.

```text
esbuild src/index.ts --bundle --platform=node --outdir=dist
```


>[!info] IIFE
> immediately-invoked function expression 약자로 브라우저에서 동작하는 포맷이다. 

>[!info] CJS
> commonJS의 약자로 Node에서 default로 동작하는 포맷이다.

>[!info] ESM
> ECMA Script 라는 뜻으로 import/export를 이용해 module를 관리할 수 있다. `.mjs` 확장자를 이용해야 사용가능하다.
>

### 빌드 스크립트 작성하기
esbuild 명령 사용시 옵션이 길어지면 작성하기 불편하고 관리하기 힘들 수 있다. 이런 경우 js를 활용해 빌드 스크립트를 작성할 수 있다. ( [공식 홈페이지 빌드 스크립트](https://esbuild.github.io/getting-started/#build-scripts) )

```js
import * as esbuild from "esbuild";

esbuild.build({
	entryPoints: ["src/index.ts"],
	bundle: true,
	outfile: "dist/index.js",
	platform: "node",
	packages: 'external'
}).catch(() => process.exit(1));
```


```text
node build.mjs
```

따로 스크립트 파일을 만들어서 실행만 해주면 되기 때문에 간단하다.

>[!tip]
>무조껀 빌드 스크립트를 짜는 것이 좋은 것만은 아니다. 설정이 간단한데 build script를 따 사용하는 것은 오히려 프로젝트의 복잡성을 증가시킬수도 있다.

### tsconfig 설정하기
```json
{
	"compilerOptions": {
		"module": "esnext",
		"moduleResolution": "node",
		"esModuleInterop": true,
		"resolveJsonModule": true,
		"allowJs": true,
		"declaration": true,
		"target": "esnext",
		"outDir": "./dist",
		"strict": true,
		"forceConsistentCasingInFileNames": true,
		"strictNullChecks": true,
		"paths": {
			"*": ["node_modules/*"]
		},
		"emitDeclarationOnly": true,
		"baseUrl": "./src"
	}
}
```

사용하면서 설정한 tsconfig 옵션이다.

module, target, allowJS, esModuleInterop, resolveJsonModule 등은 esbuild에 영향을 끼치는 옵션이기에 중요하다.

그 외에 declaration이나 emitDeclarationOnly는 esbuild에 영향을 주는 옵션은 아니다. 그 이유는 esbuild는 타입 선언에는 관심이 없기 떄문이다.

type.d.ts 를 생성하기 위해 pacakage.json에 scripts에 tsc를 추가한다.

```json
"scripts": {
  "build": "tsc && node build.mjs",
}
```

>[!caution]
>npm 패키지로 만들려고 모듈화 하는 경우 타입 선언은 IDE를 더 잘 사용할 수 있게 해주고 빌드의 가독성을 높일 수 있기 때문에 중요한 옵션이 될 수 있다. 이를 활용하기 위해서는 tsc 명령어를 활용해야 하는데 esbuild로 번들링을 만들고 컴파일 하기 때문에 emitDeclarationOnly를 이용해 tsc 자체 컴파일을 사용하지 않고 타입 선언만 만들어 주는 것이 도움이 될 수 있다.


### esbuild-register
esbuild register를 활용하면 typescript 파일을 node로 바로 쉽게 실행 가능하다. 개발하면서 의도한대로 잘 동작하는지 테스트하거나, 스크립트 파일을 실행해야 하는 경우 유용하게 사용할 수 있다.

```shell
node -r esbuild-register src/index.ts
```

esbuild와 동일하게 동작하며, node에서 바로 실행 가능하게 해준다. ts-node보다 이게 훨씬 빠르고 편리하다고 생각한다.
## 질문 & 확장
- web에서는 정식 버전 출시가 되지 않아 프로덕션 build 용으로 부담스러운데 npm 패키지 용도로는 아주 잘 사용할 수 있을 것 같다. 모듈 관리하기도 쉽고, 설정도 비교적 간편하기 때문이다.
- import .js .mjs를 붙일 필요 없다. 이런 경우에는 babel을 사용하면 해결할 수 있었지만, esbuild의 경우 알아서 번들링 과정에서 트랜스파일링 해주기 때문에 편리한 것 같다.

## 출처(링크)
- https://www.peterkimzz.com/extremely-faster-esbuild-than-webpack#esbuild-%EC%82%AC%EC%9A%A9%ED%95%B4%EB%B3%B4%EA%B8%B0
- https://esbuild.github.io/
## 연결 노트










