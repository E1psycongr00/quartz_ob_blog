---
tags:
  - 완성
  - d3js
  - DataVisualization
aliases: 
date: 2024-03-03
title: d3js & react 예제
---
작성 날짜: 2024-03-03
작성 시간: 11:06


----
## 내용(Content)
### D3js + React + Typescript
D3js는 처음 사용하면 너무 어렵게 느껴지는데 TypeScript로 예제를 분석하면서 라이브러리가 어떤 방식으로 동작하는 지 조금씩 이해해보자.

[Getting Start의 LineChart](https://d3js.org/getting-started) 예제를 typescript로 바꿔서 작성하면서 코드를 분석한다.

### 설치하기
npm 환경에서 예제 코드를 테스트 할 것이기 떄문에  다음과 같이 설치한다.

```shell
npm install d3
```

이것 만으로는 타입스크립트에서 타입을 인식할 수가 없다. 그래서 개발 의존성에 다음을 추가한다.

```shell
npm i -D @types/d3
```

그 외에 React 환경을 설치해준다.
### LinePlot 입력 정의하기

```ts
interface LinePlotProps {
	data: number[];
	width?: number;
	height?: number;
	marginTop?: number;
	marginRight?: number;
	marginBottom?: number;
	marginLeft?: number;
}
```

React 컴포넌트의 입력을 위해 Props 타입을 정의한다.

### useRef 사용한 이유
```ts
	const gx = useRef<SVGSVGElement | null>(null);
	const gy = useRef<SVGSVGElement | null>(null);

```

![[Pasted image 20240303190436.png]]

SVGSVGElement는 웹 API의 일부로 `<SVG>` 요소를 접근하는데 사용되는 인터페이스이다.

![[Pasted image 20240303190420.png]]

이 외에도 `SVGSVGElement`는 다양한 메서드를 제공하여 `<svg>` 요소를 조작할 수 있다. 예를 들어, 행렬 연산이나 시각적 렌더링 장치에서의 재그리기 시간을 제어하는 기능 등이 있다.

참고로, `SVGSVGElement`는 `SVGGraphicsElement`라는 부모 인터페이스로부터 속성을 상속받는다. 이는 `SVGSVGElement`가 SVG 그래픽 요소의 일반적인 속성과 메서드에 접근할 수 있음을 의미한다.

이렇게 `SVGSVGElement`는 SVG 요소를 조작하고 제어하는 데 필요한 다양한 속성과 메서드를 제공하므로, 웹 개발자들이 SVG 요소를 효과적으로 활용할 수 있게 돕는다.

>[!tip] useRef를 사용한 이유
>- userRef 는 React Hook 중 하나로, 이를 사용하면 컴포넌트의 라이프 사이클동안 지속적으로 값을 유지할 수 있다. 
>- 위의 경우  useRef를 이용해 생성된 참조 객체를 JSX의 ref 속성에 연결하여 DOM에 접근 가능하다.  DOM을 직접 조작하지 않고 DOM 요소에 접근하고 참조를 통해 저장하기 위해 사용된다는 것이다.
### d3-scale
```ts
	const x = d3
		.scaleLinear()
		.domain([0, data.length - 1])
		.range([marginLeft, width - marginRight]);

	const y = d3
		.scaleLinear()
		.domain(d3.extent(data) as [number, number])
		.range([height - marginBottom, marginTop]);
```

scaleLinear는 선형 스케일링이다. $y = mx + b$ 으로 스케일링 되며 domain은 실제 입력범위, range는 실제 출력 범위이다.

예를 들면 domain 이 `[0, 10,000]`이고 range 가 `[0, 100]`이라고 가정하자. 입력으로 5000이 주어졌다면 선형 스케일링의 결과로  출력은 50이 된다. domain이 선형적으로 축소되어 range 출력에 스케일링되는 것이다.



```js
// 예시
import * as d3 from "d3";

const x = d3.scaleLinear()
	.domain([0, 100])
	.range([0, 1000]);

console.log(x(50)); // 결과는 500 ... (10배 스케일링 됨)
```

### d3-shape
```ts
	const line = d3
		.line<number>()
		.x((d, i) => x(i) as number)
		.y(d => y(d) as number);
```

Lines line은 x,y를 정의해서 2차원 그래프를 그릴 수 있다.
위 식을 해석해보면 이전에 스케일링된 x,y를 이용해 data 배열로부터 data 값과 index를 가져와 index는 x 스케일링, d는 y 스케일링 한 것을 입력으로 받아 Line을 생성하는 것이다.


### useEffect와 d3.select
```ts
	useEffect(() => {
		if (gx.current) {
			d3.select(gx.current).call(d3.axisBottom(x));
		}
	}, [gx, x]);
	useEffect(() => {
		if (gy.current) {
			d3.select(gy.current).call(d3.axisLeft(y));
		}
	}, [gy, y]);
```

>[!summary]
>1. d3.select => gx.current (DOM 요소)를 선택
>2. .call => select한 요소에서 함수를 한번만 호출
>3. d3.axisBottom(x) => x축 생성



이 코드는 D3.js 라이브러리를 사용하여 차트의 x축을 그리는 역할을 한다.

`d3.select(gx.current)`는 D3의 `select` 함수를 사용하여 `gx.current` 요소를 선택한다.  여기서 `gx`는 React의 `ref` 객체이며, `current` 프로퍼티는 `ref`가 참조하는 DOM 요소를 가리킵니다. 이 DOM 요소는 차트의 x축을 그릴 공간을 나타낸다.

`.call(d3.axisBottom(x))`는 선택된 요소에 `d3.axisBottom(x)`를 호출한다.. `d3.axisBottom`은 D3의 함수로, 인자로 받은 스케일 `x`에 따라 하단(x축)에 축을 생성합니다. `call` 함수는 첫 번째 인자로 받은 함수(`d3.axisBottom(x)`)를 호출하며, 이 때 `this` 컨텍스트를 선택된 요소로 설정합니다. 즉, `d3.axisBottom(x)`는 `gx.current` 요소에 축을 그리게 됩니다.

따라서 이 코드는 `gx.current` 요소에 x축을 그리는 작업을 수행합니다. 이 작업은 `x` 스케일에 따라 x축의 범위와 눈금을 설정합니다.

>[!tip] useEffect 사용 이유
>useEffect와 useRef를 함께 사용하면 useEffect에서 입력으로 들어간 배열의 상태 값이 업데이트 될 때만 리렌더링 되도록 할 수 있다. 위의 코드의 경우 data값이 바뀌거나 스케일링 값들이 업데이트되면 축을 그리도록 코드를 짜고 있다. 이를 통해 불필요하게 렌더링되는 여러번 렌더링되는 일을 막을 수 있다.
### 전체 코드

```ts
import * as d3 from "d3";
import { useRef, useEffect } from "react";

interface LinePlotProps {
	data: number[];
	width?: number;
	height?: number;
	marginTop?: number;
	marginRight?: number;
	marginBottom?: number;
	marginLeft?: number;
}

export default function LinePlot({
	data,
	width = 640,
	height = 400,
	marginTop = 20,
	marginRight = 20,
	marginBottom = 30,
	marginLeft = 40,
}: LinePlotProps) {
	const gx = useRef<SVGSVGElement | null>(null);
	const gy = useRef<SVGSVGElement | null>(null);
	const x = d3
		.scaleLinear()
		.domain([0, data.length - 1])
		.range([marginLeft, width - marginRight]);
	const y = d3
		.scaleLinear()
		.domain(d3.extent(data) as [number, number])
		.range([height - marginBottom, marginTop]);
	const line = d3
		.line<number>()
		.x((d, i) => x(i) as number)
		.y(d => y(d) as number);

	useEffect(() => {
		if (gx.current) {
			d3.select(gx.current).call(d3.axisBottom(x));
		}
	}, [gx, x]);
	useEffect(() => {
		if (gy.current) {
			d3.select(gy.current).call(d3.axisLeft(y));
		}
	}, [gy, y]);
	return (

		<svg width={width} height={height}>
	
			<g ref={gx} transform={`translate(0,${height - marginBottom})`} />
	
			<g ref={gy} transform={`translate(${marginLeft},0)`} />
			<path
				fill="none"
				stroke="currentColor"
				strokeWidth="1.5"
				d={line(data) || ""}
			/>
			<g fill="white" stroke="currentColor" strokeWidth="1.5">
				{data.map((d, i) => (
					<circle
						key={i}
						cx={x(i) as number}
						cy={y(d) as number}
						r="2.5"
					/>
				))}
			</g>
		</svg>
	);
}
```

## 질문 & 확장

(없음)

## 출처(링크)
- https://d3js.org/d3-scale
- https://developer.mozilla.org/en-US/docs/Web/API/SVGSVGElement
- https://eddie-sunny.tistory.com/70
## 연결 노트
- [[typescript question mark parameter]]









