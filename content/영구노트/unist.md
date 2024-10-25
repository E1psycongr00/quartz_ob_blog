---
tags:
  - 완성
  - CS
  - SyntaxTree
aliases:
  - Universal Syntax Tree
title: unist
date: 2024-02-26
---
작성 날짜: 2024-02-26
작성 시간: 23:47


----
## 내용(Content)
### unist
>[!summary] unist
>javascript를 활용해 구문 분석과 Syntax Tree를 만들고 다룰 수 있게 해주는 라이브러리이다.

unist는  Syntax Tree의 General한 형식을 위해 만들어졌다고 한다. 즉,  [Web IDL](https://webidl.spec.whatwg.org/) 에서 제공하는 문법과 유사하게 ast의 기본적인 명세를 정의한 것이라고 보아도 좋다.

unist는 2가지 Syntax Tree 모두 지원한다.

- **concrete syntax tree**
- **abstract syntax tree**

### 쓰이는 곳
unist는 다음과 같은 곳에 활용될 수 있다.

- hast (HTML Abstract Syntax Tree)
- nlcst (자연어 Abstract Syntax Tree)
- mdast (Markdown Abstract Syntax Tree)
- xast (XML Abstract Syntax Tree)

 [Unified](https://github.com/unifiedjs/unified), [Remark](https://github.com/remarkjs/remark), [Rehype](https://github.com/remarkjs/remark) 라이브러리는 unist를 활용해 로직들을 플러그인화해서 markdown나 html 등 분석과 수정을 굉장히 쉽게 만들어준다.

### 활용
#### unist 트리 만들기

직접 json 으로 만들어도 되지만 힘들다. 이를 `unist-builder` 를 이용하면 쉽게 만들 수 있다.

```js
import { u } from "unist-builder";

const tree = u("root", {typeName: "A"}, [
    u("node", {typeName: "B"}, [
        u("leaf", {typeName: "E"}),
    ]),
    u("leaf", {typeName: "C"}),
]);

  

console.dir(tree, { depth: null });
```

![[Pasted image 20240227001612.png]]

`u(typename, 정의할 데이터 구조, [children])`

- typename은 트리의 유형을 정의한다. 탐색할 때 많이 이용된다.
- 정의할 데이터 구조는 js object로 구성해주면 된다. 간단하게 String 타입만 넣는 경우 value: "input string" 으로 생성된다
- `[ ]` 대괄호를 정의하면 여기에는 노드의 자식들을 정의한다. 이를 이용해 Tree 구조로 Tree를 구성할 수 있게 된다

#### 특정 노드 탐색하기

직접 로직을 짜도 되지만 간단하게 사용하는 경우 `unist-util-visit`을 활용할 수 있다. 기본적으로 Preorder Traverse로 동작한다. 재귀 탐색이 가능하며, callback 함수를 받아서 동작한다.

```js
import { u } from "unist-builder";
import { visit } from "unist-util-visit";


// 트리 생성
const tree = u("root", { typeName: "A" }, [
    u("node", { typeName: "B" }, [u("leaf", { typeName: "E" })]),
    u("leaf", { typeName: "C" }),
]);
  
console.dir(tree, { depth: null });

// 트리를 재귀적으로 탐색
visit(tree, "leaf", (node, index, parent) => {
    console.log(node.typeName);
});
```

visit에 "leaf"는 생략이 가능하다.  위 함수는 "leaf" type의 노드를 탐색하고 찾으면 Callback function을 실행하라는 뜻이다.


#### 그 외 utilities List

- [`unist-util-ancestor`](https://github.com/gorango/unist-util-ancestor) — get the common ancestor of one or more nodes
- [`unist-util-assert`](https://github.com/syntax-tree/unist-util-assert) — assert nodes
- [`unist-util-filter`](https://github.com/syntax-tree/unist-util-filter) — create a new tree with all nodes that pass the given function
- [`unist-util-find`](https://github.com/blahah/unist-util-find) — find a node by condition
- [`unist-util-find-after`](https://github.com/syntax-tree/unist-util-find-after) — find a node after another node
- [`unist-util-find-all-after`](https://github.com/syntax-tree/unist-util-find-all-after) — find nodes after another node or index
- [`unist-util-find-all-before`](https://github.com/syntax-tree/unist-util-find-all-before) — find nodes before another node or index
- [`unist-util-find-all-between`](https://github.com/mrzmmr/unist-util-find-all-between) — find nodes between two nodes or positions
- [`unist-util-find-before`](https://github.com/syntax-tree/unist-util-find-before) — find a node before another node
- [`unist-util-flat-filter`](https://github.com/unicorn-utterances/unist-util-flat-filter) — flat map version of `unist-util-filter`
- [`unist-util-flatmap`](https://gitlab.com/staltz/unist-util-flatmap) — create a new tree by expanding a node into many
- [`unist-util-generated`](https://github.com/syntax-tree/unist-util-generated) — check if a node is generated
- [`unist-util-index`](https://github.com/syntax-tree/unist-util-index) — index nodes by property or computed key
- [`unist-util-inspect`](https://github.com/syntax-tree/unist-util-inspect) — node inspector
- [`unist-util-is`](https://github.com/syntax-tree/unist-util-is) — check if a node passes a test
- [`unist-util-map`](https://github.com/syntax-tree/unist-util-map) — create a new tree by mapping nodes
- [`unist-util-modify-children`](https://github.com/syntax-tree/unist-util-modify-children) — modify direct children of a parent
- [`unist-util-parents`](https://github.com/syntax-tree/unist-util-parents) — `parent` references on nodes
- [`unist-util-position`](https://github.com/syntax-tree/unist-util-position) — get positional info of nodes
- [`unist-util-reduce`](https://github.com/GenerousLabs/unist-util-reduce) — recursively reduce a tree
- [`unist-util-remove`](https://github.com/syntax-tree/unist-util-remove) — remove nodes from trees
- [`unist-util-remove-position`](https://github.com/syntax-tree/unist-util-remove-position) — remove positional info from trees
- [`unist-util-replace-all-between`](https://github.com/unicorn-utterances/unist-util-replace-all-between) — replace nodes between two nodes or positions
- [`unist-util-select`](https://github.com/syntax-tree/unist-util-select) — select nodes with CSS-like selectors
- [`unist-util-size`](https://github.com/syntax-tree/unist-util-size) — calculate the number of nodes in a tree
- [`unist-util-source`](https://github.com/syntax-tree/unist-util-source) — get the source of a value (node or position) in a file
- [`unist-util-stringify-position`](https://github.com/syntax-tree/unist-util-stringify-position) — stringify a node, position, or point
- [`unist-util-visit`](https://github.com/syntax-tree/unist-util-visit) — recursively walk over nodes
- [`unist-util-visit-parents`](https://github.com/syntax-tree/unist-util-visit-parents) — recursively walk over nodes, with a stack of parents
- [`unist-util-visit-children`](https://github.com/syntax-tree/unist-util-visit-children) — visit direct children of a parent
- [`unist-util-visit-all-after`](https://github.com/mrzmmr/unist-util-visit-all-after) — visit nodes after another node
- [`unist-builder`](https://github.com/syntax-tree/unist-builder) — helper for creating trees
## 질문 & 확장

(없음)

## 출처(링크)
- https://github.com/syntax-tree/unist?tab=readme-ov-file#tree-traversal

## 연결 노트
- [[AST]]







