---
tags:
  - 완성
  - 솔루션
  - React
  - Redux
  - Typescript
aliases:
  - Redux & TS
date: 2024-03-08
title: Redux를 활용해 React 상태 관리하기
---
작성 날짜: 2024-03-08
작성 시간: 19:08


----
## 문제 & 원인
![[Context Reducer(draw).svg]]
[[React Context API와 Reducer로 상태 관리하기]]를 활용하면 [[Props Driling]] 없이 컴포넌트가 바로 상태를 활용하고 동기화가 가능하다. 매우 좋은 방법 중 하나지만 만약 상태가 복잡해 매번 이렇게 컴포넌트를 관리해야 한다면 타입 선언에 많은 코드를 짜야 하기 때문에 힘들다.
## 해결 방안
### Redux
Redux를 활용하면 위의 패턴을 완전히 전역 상태에서 활용한다. 그렇기 때문에 style 가이드가 있고 마치 프레임워크처럼  공식 문서에서 추천하는 스타일에 맞게 쓰면 위의 그림과 같은 작업을 컴포넌트를 단순하게 사용 가능하다.

![[Redux 동작 원리(draw).svg]]

Redux에서는 store를 이용해 상태를 전역으로 관리한다. 각 도메인 별로 reducer를 생성 store에 등록할 수 있고 컴포넌트는 해당 도메인의 reducer로 dispatch를 하거나 selector를 활용해 상태를 가져올 수 있다.

이전에 사용한 React context API로도 위와 같이 구현하고 패키지를 잘 나누면 사용 가능하지만 Redux를 이용하면 짧은 코드로 쉽게 구현 가능하다.

### RTK(Redux Tookit)
Redux 코어와 React-redux만 사용하는 기존 Legacy 방식은 난이도가 높고 React Context Api를 이용하는 것과 큰 차이가 없다. Redux는 기본적으로 동기적으로 동작하기 때문에 비동기면 따로 라이브러리를 써야 하며 객체가 불변하지 않으면 사이드 이펙트가 발생할 수 있기 때문에 immr 라이브러리를 추가적으로 사용한다. 그러나 RTK를 사용하면 손쉽게 위오 같은 패턴을 사용가능하다. [공식 문서](https://ko.redux.js.org/tutorials/typescript-quick-start#use-typed-hooks-in-components) 에서도 RTK를 쓸 것을 추천한다.

### 구현하기
#### Store와 커스텀 Hook 구성하기
```ts
// store.ts
import { configureStore } from "@reduxjs/toolkit";
import taskReducer from "@/module/task/TaskSlice";


export const store = configureStore({
	reducer: {
		tasks: taskReducer,
	},
});


export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

store에는 구현한 reducer를 넣어주면 된다. 키를 이용해 나중에 selector가 접근할 때 쓰인다.

```ts
// hook.ts
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";

import type { RootState, AppDispatch } from "./store";

export const useAppDispatch: () => AppDispatch = useDispatch;
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

아마 모든 프로젝트에서 공통적인 코드가 될 듯하다. module 또는 도메인에 대해 독립적이다. 이렇게 커스텀 훅을 짜 놓으면 컴포넌트에서 상태를 가져오거나 dispatch할 때 편리하게 쓸 수 있다.

#### 최상위에 Provider 컴포넌트 선언하기
```ts
export default function Home() {
	return (
		<div>
			<Provider store={store}>
				<TaskContainer />
			</Provider>
		</div>
	);
}
```

#### Slice.ts 구성하기
```ts
export interface TaskState {
	id: number;
	text: string;
}

const initialTaskStates: TaskState[] = [
	{ id: 0, text: "Learn React" },
	{ id: 1, text: "Learn Next.js" },
	{ id: 2, text: "Learn TypeScript" },
];


export const taskSlice = createSlice({
	name: "task",
	initialState: initialTaskStates,
	reducers: {
		addTask: (state: TaskState[], action: PayloadAction<string>) => {
			return [...state, { id: state.length, text: action.payload }];
		},
		editTask: (state: TaskState[], action: PayloadAction<TaskState>) => {
			return state.map(t =>
				t.id === action.payload.id
					? { ...t, text: action.payload.text }
					: t
			);
		},
		deleteTask: (state: TaskState[], action: PayloadAction<number>) => {
			return state.filter(task => task.id !== action.payload);
		},
	},
});


export const { addTask, editTask, deleteTask } = taskSlice.actions;
export const selectTasks = (state: RootState) => state.tasks;
export default taskSlice.reducer;
```

RTK를 사용하니 확실히 타입 선언이 필요가 없어져 코드가 간결해졌음을 알 수 있다.

이 코드의 재밌는 점은 domain에 하나의 reducer를 넣고 reducer가 여러 action type을 가지는 것이 아니라 여러 reducer를 store에 넣고, reducer를 dispatch한다는 점이다.

>[!tip]
>[공식 문서](https://ko.redux.js.org/tutorials/typescript-quick-start#use-typed-hooks-in-components)에 보면 비동기 함수나 여러 가지 상황에 대해서 작성 가능하다. 내 예제를 심플한 경우니 심플하게 작성했다.
#### 컴포넌트에서 활용하기
```ts
// TaskList.tsx

import React from "react";
import { useAppSelector } from "@/module/hook";
import Task from "./Task";


export default function TaskList() {
	const tasks = useAppSelector(state => state.tasks);

	return (
		<ul>
			{tasks.map(task => (
				<li key={task.id}>
					<Task task={task} />
				</li>
			))}
		</ul>
	);
}
```

```ts
// AddTask.tsx

import React, { useContext, useState } from "react";
import { useAppDispatch } from "@/module/hook";
import { addTask } from "@/module/task/TaskSlice";


const AddTask = () => {
	const [text, setText] = useState("");
	const dispatch = useAppDispatch();
	
	
	return (
		<div>
			<input
				type="text"
				value={text}
				onChange={e => setText(e.target.value)}
				className="border border-gray-300 rounded-md p-2"
			/>
			<button
				onClick={() => dispatch(addTask(text))}
				className="border border-gray-300 rounded-md p-2 ml-2"
>	
				Add Task
			</button>
		</div>
	);
};

export default AddTask;
```

```ts
// Task.tsx

import React, { useState } from "react";
import { useAppDispatch } from "@/module/hook";
import { deleteTask, editTask } from "@/module/task/TaskSlice";
import { TaskState } from "@/module/task/TaskSlice";


interface TaskProps {
	task: TaskState;
}


export default function Task({ task }: TaskProps) {
	const [editMode, setEditMode] = useState(false);
	const [editText, setEditText] = useState(task.text);
	
	const dispatch = useAppDispatch();


	const handleEditClick = () => {
		setEditMode(true);
	};

	const handleSaveClick = () => {
		dispatch(
			editTask({
				id: task.id,
				text: editText,
			})
		);
		setEditMode(false);
	};


	const handleInputChange = (e: React.ChangeEvent<HTMLInputElement>) => {
		setEditText(e.target.value);
	};

	return (
		<div>
			{editMode ? (
				<input
					type="text"
					value={editText}
					onChange={handleInputChange}
					className="border border-black bg-gray-300 p-1"
				/>
			) : (
				<span>{task.text}</span>
			)}
			&nbsp;&nbsp;
			{editMode ? (
				<button
					className="border border-gray-300 rounded-md p-2 ml-1"
					onClick={handleSaveClick}
>	
					Save
				</button>
			) : (
				<button
					className="border border-gray-300 rounded-md p-2 ml-1"
					onClick={handleEditClick}
>	
					Edit
				</button>
			)}
			&nbsp;&nbsp;
			<button
				className="border border-gray-300 rounded-md p-2 ml-1"
				onClick={() => dispatch(deleteTask(task.id))}
>	
				Delete
			</button>
		</div>
	);
}
```
## 질문 & 확장

(없음)

## 출처(링크)
- https://ko.redux.js.org/tutorials/typescript-quick-start#use-typed-hooks-in-components
- https://react.vlpt.us/using-typescript/05-ts-redux.html
- https://kyounghwan01.github.io/blog/TS/React/redux-ts/#%E1%84%8F%E1%85%A1%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%90%E1%85%A5-%E1%84%91%E1%85%B3%E1%84%85%E1%85%A6%E1%84%8C%E1%85%A6%E1%86%AB%E1%84%90%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%A7%E1%86%AB-%E1%84%8F%E1%85%A5%E1%86%B7%E1%84%91%E1%85%A9%E1%84%82%E1%85%A5%E1%86%AB%E1%84%90%E1%85%B3

## 연결 노트










