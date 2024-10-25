---
tags:
  - 완성
  - JS
  - React
  - Typescript
aliases: 
date: 2024-08-07
title: CRA 와 Typescript 프로젝트 생성하기
---
작성 날짜: 2024-08-07
작성 시간: 17:47


----
## 내용(Content)

### 앞서

CRA 프로젝트에 Typescript를 넣은 세팅을 할 계획이다. 이 때 설치는 `npm` 대신 `yarn`을 이용해서 할 생각이다.

### 1. CRA + typescript 생성하기

```shell
yarn create react-app my-app --template typescript
```

이렇게 프로젝트를 생성하면 개발에 필요한 프레임에 맞게 프로젝트를 생성해준다.
my-app 부분에 나만의 프로젝트 폴더명을 기입하면 된다.

![[Pasted image 20240807175119.png]]

.eslintrc와 .prettierrc는 내가 추가한 것

### 2. eslint와 prettier 세팅

vscode extensions에서 다음 2개를 설치한다.

![[Pasted image 20240807175226.png]]

![[Pasted image 20240807175239.png]]

```eslintrc
{
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint", "prettier"],
  "extends": [
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended"
  ],
  "parserOptions": {
    "ecmaVersion": "latest",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "rules": {
    "react/jsx-uses-vars": "error",
    "react/prop-types": 0,
    "react/react-in-jsx-scope": 0,
    "@typescript-eslint/explicit-module-boundary-types": 0
  },
  "settings": {
    "import/resolver": {
      "node": {
        "extensions": [".js", ".jsx", ".ts", ".tsx"]
      }
    }
  }
}
```

```prettierrc
{
  "arrowParens": "always",
  "bracketSpacing": true,
  "jsxBracketSameLine": false,
  "jsxSingleQuote": false,
  "orderedImports": true,
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "useTabs": false,
  "printWidth": 80
}
```

이렇게 2개를 작성하고 formatter에서 default을 prettier로 변경해주면 된다. 그러면 prettier 포맷과 일치하지 않으면 script 실행시 오류가 발생한다. 그렇기에 저장 시에 formatter가 적용되게 FormatOnSave도 설정해주자.

### 3. 라이브러리 세팅

어느 정도 설치는 되는데 부족한 부분을 더 설치해주자.

```shell
yarn @types/node eslint prettier eslint-config-prettier eslint-plugin-prettier @typescript-eslint/parser --dev
```

### 4. tsconfig를 세팅해주자

```json
{
  "compilerOptions": {
    "target": "es6",
    "lib": ["dom", "dom.iterable", "esnext"],
    "baseUrl": "./src",
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  },
  "include": ["src"]
}


```

## 질문 & 확장

(없음)

## 출처(링크)

- https://bolob.tistory.com/entry/React-CRAcreate-react-app-TypeScript-%EC%84%B8%ED%8C%85%ED%95%98%EA%B8%B0

## 연결 노트
