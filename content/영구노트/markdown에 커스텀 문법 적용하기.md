---
tags:
  - 완성
  - ast
  - SyntaxTree
aliases: 
title: markdown에 커스텀 문법 적용하기
date: 2024-02-28
---
작성 날짜: 2024-02-28
작성 시간: 15:04


----

## 문제 & 원인
블로그를 이전하거나 또는 Publishing 할 때 기존의 에디터에서 제공하지 않는 Markdown 문법을 사용하면 Html로 렌더링되지 않아 문제가 발생한 일이 있을 것이다.

예를 들면 Github에서 새로 제공하는 markdown-alert라는 기능일 것이다.  이것은 Obsidian에서 콜아웃처럼 동작하는데 문제는 기본 마크 다운 문법에는 없다는 것이며, 에디터마다 문법이 서로 조금씩 다르다.

### Github Alert

![](Pasted%20image%2020240228153619.png)

그 중 Note가 어떤 HTML을 생성했는지 살펴보면 다음과 같다.

![](Pasted%20image%2020240228153825.png)

div 내에 2개의 p 태그를 정의하는 데 하나는 title을 정의하고 다른 하나는 단순 p태그로 이루어져 있고 Block문에 메인 컨텐츠를 담고 있다.

문서를 살펴보면 Github Alert는 중첩이 허용되지 않는다고 한다.

### Obsidian Callout
Obsidian의 콜아웃은 더 많은 기능을 지원한다. 
- 중첩 사용 가능
- Obsidian title 사용가능
- 콜아웃을 접었다 폈다 할 수 있음

간단하게 어떻게 HTML로 렌더링 됬는지 살펴보자

![](Pasted%20image%2020240228154426.png)

![](Pasted%20image%2020240228154608.png)

Obsidian callout의 경우  callout 클래스 div가 존재한다.
그리고 그 내부엔 callout-title과 callout- content 2개의 div 태그로 나뉘어 진다.

callout-title을 더 파고 들면
![](Pasted%20image%2020240228154758.png)

callout-icon과 callout-title-inner가 정의되어 있음을 알 수 있다.
만약 callout title에 text가 없다면 callout-title-inner는 생성되지 않는다.


## 해결 방안
### 해결 전략
Obsidian 방식은  많은 기능을 지원하고 처음 markdown 문법을 조작하고자하는 나에겐 어려웠다. 그래서 상대적으로 쉬운 GIthub Alert 방식으로 html이 렌더링 되게 만들고 싶었다.

그 전에 markdown으로 html로 렌더링하기 위해서 unified, remark, rehype을 사용했다.

[[unified & remark & rehype 동작 과정]] 을 살펴보면 parsing 자체를 조작하는 것보다 중간에 ast를 조작하는 것이 우리 목표를 달성하기 더 쉽다. 

ast 를 중간에 조작해서 github-alert 문법을 호환하고 html로 렌더링 할 수 있도록 해보자



>[!info] unified
>unified는 [[unist]] 트리에 대한 프로세서라 할 수 있다. unified는 소스를 파싱에 적절한 ast를 만들고 트랜스포머로 순회하면서 ast를 조작한 후에 컴파일러로 결과를 출력한다. 파이프라인 패턴으로 Plugin을 붙이며 사용한다.

>[!info] remark
>mdast(markdown을  ast로 표현 ) 기반으로 동작하는  플러그인이라 할 수 있다.   mdast 트리 를 제어하고 트랜스포머로 조작할 수 있다.
>

>[!note] rehype
>rehype은 hast(html을 ast로 표현) 기반으로 동작하는 플러그인이다. hast를 제어하고 트랜스 포머를 활용해 트리를 조작 가능하다.

### 문제 해결하기
#### mdast를 변형해서 해결하기

```
>[!note]
>hello world
```

위와 같은 markdown을 파싱하면 다음과 같은 mdast를 얻을 수 있다.


```
{
  type: 'root',
  children: [
    {
      type: 'blockquote',
      children: [
        {
          type: 'paragraph',
          children: [
            {
              type: 'text',
              value: '[!note]\r\nhello world',
              position: {
                start: { line: 1, column: 2, offset: 1 },
                end: { line: 2, column: 13, offset: 22 }
              }
            }
          ],
          position: {
            start: { line: 1, column: 2, offset: 1 },
            end: { line: 2, column: 13, offset: 22 }
          }
        }
      ],
      position: {
        start: { line: 1, column: 1, offset: 0 },
        end: { line: 2, column: 13, offset: 22 }
      }
    }
  ],
  position: {
    start: { line: 1, column: 1, offset: 0 },
    end: { line: 2, column: 13, offset: 22 }
  }
}
```

mdast를 분석하면 blockquote 타입으로 생성되고 내부에 paragraph 타입이 생성되었다. paragraph 내부에는 text 타입이 존재한다.

쉽게 설명하면 html로 렌더링 되는 태그는 다음과 같다.

- blockquote => blockquote
- paragraph => p
- text => 태그 안에 정보 ex) `<p>{text}</p>`

Github Alert로 만들기 위해서는 
- blockquote => 클래스 네임이 들어간 div 태그로 바꿔야 하며
- class => `github-alert github-alert-${alertType}` 로 바뀌어야 한다.
- 내부의 p 태그는 2개의 p 태그가 존재해야 하며 첫 p 태그는 github-alert-title로 정의된다

```js
import { visit } from "unist-util-visit";
import { u } from "unist-builder";

const REGEX = /\[!\s*(.*?)\](?:\s(.*))?/;


const DEFAULT_GITHUB_ICONS = {
    note: '<svg class="octicon octicon-info mr-2" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="M0 8a8 8 0 1 1 16 0A8 8 0 0 1 0 8Zm8-6.5a6.5 6.5 0 1 0 0 13 6.5 6.5 0 0 0 0-13ZM6.5 7.75A.75.75 0 0 1 7.25 7h1a.75.75 0 0 1 .75.75v2.75h.25a.75.75 0 0 1 0 1.5h-2a.75.75 0 0 1 0-1.5h.25v-2h-.25a.75.75 0 0 1-.75-.75ZM8 6a1 1 0 1 1 0-2 1 1 0 0 1 0 2Z"></path></svg>',
    tip: '<svg class="octicon octicon-light-bulb mr-2" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="M8 1.5c-2.363 0-4 1.69-4 3.75 0 .984.424 1.625.984 2.304l.214.253c.223.264.47.556.673.848.284.411.537.896.621 1.49a.75.75 0 0 1-1.484.211c-.04-.282-.163-.547-.37-.847a8.456 8.456 0 0 0-.542-.68c-.084-.1-.173-.205-.268-.32C3.201 7.75 2.5 6.766 2.5 5.25 2.5 2.31 4.863 0 8 0s5.5 2.31 5.5 5.25c0 1.516-.701 2.5-1.328 3.259-.095.115-.184.22-.268.319-.207.245-.383.453-.541.681-.208.3-.33.565-.37.847a.751.751 0 0 1-1.485-.212c.084-.593.337-1.078.621-1.489.203-.292.45-.584.673-.848.075-.088.147-.173.213-.253.561-.679.985-1.32.985-2.304 0-2.06-1.637-3.75-4-3.75ZM5.75 12h4.5a.75.75 0 0 1 0 1.5h-4.5a.75.75 0 0 1 0-1.5ZM6 15.25a.75.75 0 0 1 .75-.75h2.5a.75.75 0 0 1 0 1.5h-2.5a.75.75 0 0 1-.75-.75Z"></path></svg>',
    important:
        '<svg class="octicon octicon-report mr-2" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="M0 1.75C0 .784.784 0 1.75 0h12.5C15.216 0 16 .784 16 1.75v9.5A1.75 1.75 0 0 1 14.25 13H8.06l-2.573 2.573A1.458 1.458 0 0 1 3 14.543V13H1.75A1.75 1.75 0 0 1 0 11.25Zm1.75-.25a.25.25 0 0 0-.25.25v9.5c0 .138.112.25.25.25h2a.75.75 0 0 1 .75.75v2.19l2.72-2.72a.749.749 0 0 1 .53-.22h6.5a.25.25 0 0 0 .25-.25v-9.5a.25.25 0 0 0-.25-.25Zm7 2.25v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 9a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path></svg>',

    warning:
        '<svg class="octicon octicon-alert mr-2" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path></svg>',

    caution:
        '<svg class="octicon octicon-stop mr-2" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="M4.47.22A.749.749 0 0 1 5 0h6c.199 0 .389.079.53.22l4.25 4.25c.141.14.22.331.22.53v6a.749.749 0 0 1-.22.53l-4.25 4.25A.749.749 0 0 1 11 16H5a.749.749 0 0 1-.53-.22L.22 11.53A.749.749 0 0 1 0 11V5c0-.199.079-.389.22-.53Zm.84 1.28L1.5 5.31v5.38l3.81 3.81h5.38l3.81-3.81V5.31L10.69 1.5ZM8 4a.75.75 0 0 1 .75.75v3.5a.75.75 0 0 1-1.5 0v-3.5A.75.75 0 0 1 8 4Zm0 8a1 1 0 1 1 0-2 1 1 0 0 1 0 2Z"></path></svg>',

};

  
export default function remarkGithubAlert() {
    return tree => {
        visit(tree, "blockquote", (node, index, parent) => {
            const [paragraph] = node.children;
            if (paragraph?.type !== "paragraph") {
                return;
            }
  
            const [titleText, ...restText] = paragraph.children;
            if (titleText?.type !== "text") {
                return;
            }

            const alertContentNode = {
                type: "github-alert-content",
                data: {
                    hName: "p",
                    hProperties: {
                        className: "github-alert-content",
                    },
                },
                children: [...restText]
            };

            const match = titleText.value.match(REGEX);
  
            if (!match) {
                return;
            }

            const alertType = match[1].toLowerCase();
            if (DEFAULT_GITHUB_ICONS[alertType] === undefined) {
                return;
            }
  
            const alertTitleNode = {
                type: "github-alert-title",
                data: {
                    hName: "p",
                    hProperties: {
                        className: "github-alert-title",
                    },
                },
                children: [
                    u("html", DEFAULT_GITHUB_ICONS[alertType]),
                    u("text", match[2] || match[1]),
                ],
            };

  
            const githubAlertNode = {
                type: "github-alert",
                data: {
                    hName: "div",
                    hProperties: {
                        className: `github-alert github-alert-${alertType}`,
                    },
                },
                children: [alertTitleNode, alertContentNode],
            };

  
            parent.children[index] = githubAlertNode;
        });
    };
}
```

mdast에 hName이나 hProperties 속성을 data로 넘기는 이유는 mdast를 hast로 변경시켜주는 플러그인을 사용시 해당 속성을 이용해 tagName과 properties를 생성한다

mdast를 hast로 변경시 알고리즘에 대한 자세한 내용은 https://github.com/syntax-tree/mdast-util-to-hast#algorithm 를 참고한다
## 질문 & 확장

(없음)

## 출처(링크)
- https://help.obsidian.md/Editing+and+formatting/Callouts
- https://docs.github.com/ko/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax
- [Markdown이 약간 부족할 때 - sorto.me](https://sorto.me/posts/2022-02-20--markdown)
## 연결 노트
- [[unist]]









