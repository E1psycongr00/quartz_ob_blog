---
tags:
  - 완성
  - 솔루션
  - React
  - Typescript
aliases: 
date: 2024-03-07
title: React Context API와 Reducer로 상태 관리하기
---
작성 날짜: 2024-03-07
작성 시간: 22:21


----

## 문제 & 원인


![[TaskContainer 구조(draw).svg]]

[[State 로직을  Reducer로 분리하기]] 편을 보면 분리하더라도 예제만으로 보면 왜 분리하는 지 쓸모 없어 보인다. 여전히 [[Props Driling]] 때문에 구현이 복잡하다.

## 해결 방안
### Context API
![[Context Reducer(draw)|700]]
Context API로 컴포넌트를 만들면 자식 컴포넌트들은 깊이에 상관없이 상태에 접근할 권한을 얻는다. 이를 활용해서 자식의 컴포넌트들이 부모 컴포넌트로부터 props를 받아오지 않고 Hook을 이용해 바로 접근이 가능하기 때문에 중간에 데이터를 전달할 props가 필요 없어지고 상태 끌어올리기와 같은 기법을 사용하지 않고 구현이 가능하다.

Context와 Reducer를 함께 사용하면 직접 필요한 상태를 바로 필요한 부분에 주입해서 사용할 수 있다. 그리고 변경된 상태를 공유한다.

위 문제의 경우 TaskController, AddTask, TaskList, Task 모두 Tasks라는 상태를 공유하며, 각각 다른 컴포넌트에서 조작이 주어지기 때문에 handler와 tasks props를 Driling 해야 하고 설계와 구현이 복잡하게 느껴질 수 있다. 4개의 컴포넌트 모두 tasks를 공유하기 떄문에 이들의 상태를 같은 스코프에서 접근하면 필요한 부분에 바로 주입해서 사용 가능할 것이다.

### 구현하기

#### Context 생성하기
```ts
import { createContext } from "react";

export const TasksContext = createContext<TaskState[]>([]);
export const TasksDispatchContext = createContext<React.Dispatch<TaskAction>>(null!);
```

기존에 구현했던 reducer.ts에 다음을 코드를 추가한다. createContext를 통해 Tasks 상태를 보관하는 Context와 상태에 접근하기 위한 handler를 보관할 TasksDispatchContext를 생성한다.

>[!warining] null !
>null!를 사용하면 null이 아님을 보장함을 알린다. 사용할 때는 반드시 null이 아니여야 한다.그렇지 않으면 오류가 발생한다.

#### Provider 컴포넌트 생성하기
```ts
import { useReducer } from "react";
import tasksReducer, {
	TasksContext,
	TasksDispatchContext,
} from "./tasksReducer";

type TasksProviderProps = {
	children: React.ReactNode;
};

export default function TasksProvider({ children }: TasksProviderProps) {
	const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
	return (
		<TasksContext.Provider value={tasks}>
			<TasksDispatchContext.Provider value={dispatch}>
				{children}
			</TasksDispatchContext.Provider>
		</TasksContext.Provider>
	);
}


const initialTasks = [
	{ id: 0, text: "Learn React" },
	{ id: 1, text: "Learn Next.js" },
	{ id: 2, text: "Learn TypeScript" },
];
```

#### TaskContainer 리팩토링

```ts
import AddTask from "./AddTask";
import TaskList from "./TaskList";
import TasksProvider from "./reducer/TasksProvider";

export default function TaskContainer() {
	return (
		<TasksProvider>
			<div className="m-4">
				<AddTask />
				<TaskList/>
			</div>
		</TasksProvider>
	);
}
```

#### AddTask 리팩토링
```ts
import React, { useContext, useState } from "react";
import { ADD, TasksDispatchContext } from "./reducer/tasksReducer";

const AddTask = () => {
	const [text, setText] = useState("");
	const dispatch = useContext(TasksDispatchContext);
	
	const handleAddTask = () => {
		dispatch({
			type: ADD,
			payload: text,
		});
		setText("");
	};

	return (
		<div>
			<input
				type="text"
				value={text}
				onChange={e => setText(e.target.value)}
				className="border border-gray-300 rounded-md p-2"
			/>
	
			<button
				onClick={handleAddTask}
				className="border border-gray-300 rounded-md p-2 ml-2"
>	
				Add Task
			</button>
		</div>
	);
};


export default AddTask;
```

#### Task 리팩토링
```ts

interface TaskProps {
	task: TaskState;
}


export default function Task({ task }: TaskProps) {
	const [editMode, setEditMode] = useState(false);
	const [editText, setEditText] = useState(task.text);
	
	const dispatch = useContext(TasksDispatchContext);
	
	const handleEditClick = () => {
		setEditMode(true);
	};
	
	const handleSaveClick = () => {
		dispatch({
			type: EDIT,
			payload: { id: task.id, text: editText },
		});
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
				onClick={() => dispatch({
					type: DELETE,
					payload: task.id,
				})}
>	
				Delete
			</button>
		</div>
	);
}
```
## 질문 & 확장
- 컴포넌트가 복잡하고 상태 동기화가 복잡한 경우 Context는 최고의 해결책이 될 수 있다.
- Context.Provider 자식 컴포넌트 모두에게 전역으로 상태를 공유하는 만큼 전역 상태가 가질 수 있는 부작용이 있다. 이 때문에 상태 동기화가 복잡한 경우 사용하되 컴포넌트를 잘 모듈화해야 할 것 같다.
- 이를 패턴화해서 사용하는 라이브러리가 Redux다. 그러나 Redux는 새로운 라이브러리를 설치해야 하는 만큼 bundle의 사이즈가 커지는 부작용이 있기 때문에 Context로만으로 가능하고 많지 않다면 리액트만으로도 충분할 것 같다.
## 출처(링크)
- https://ko.react.dev/learn/scaling-up-with-reducer-and-context#step-1-create-the-context

## 연결 노트
