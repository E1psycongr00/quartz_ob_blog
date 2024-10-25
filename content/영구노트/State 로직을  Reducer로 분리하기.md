---
tags:
  - 완성
  - 솔루션
  - React
  - Typescript
aliases: 
date: 2024-03-07
title: State 로직을  Reducer로 분리하기
---
작성 날짜: 2024-03-07
작성 시간: 16:49


----

## 문제 & 원인
다음과 같은 TaskContainer가 있다.

```ts
export default function TaskContainer() {
	const [tasks, setTasks] = useState(initialTasks);
	const handleAddTask = (text: string) => {
		setTasks([...tasks, { id: tasks.length, text }]);
	};

	const handleEditTask = (task: TaskState) => {
		setTasks(
			tasks.map(t => (t.id === task.id ? { ...t, text: task.text } : t))
		);
	};
	
	const handleDeleteTask = (id: number) => {
		setTasks(tasks.filter(task => task.id !== id));
	};
	
	return (
		<div className="m-4">
			<AddTask onAddTask={handleAddTask} />
			<TaskList
				tasks={tasks}
				onEditTask={handleEditTask}
				onDeleteTask={handleDeleteTask}
			/>
		</div>
	);
}
```

위의 컴포넌트 구조는 다음과 같다.

![[TaskContainer 구조(draw).svg]]

## 해결 방안
### 복잡한 컨테이너 어떻게 해야 할까
이 컴포넌트도 복잡해 보이지만 실제로는 이것보다 더 복잡한 컴포넌트들도 많다. state가 많고 이런 state를 관리하기 위한 핸들러가 많다면 이를 컨트롤하는 부모 컴포넌트의 코드량은 많아지고 이해하기 힘들어진다. React에서는 이를 reducer를 활용해서 분리할 수 있다.

### useReducer로 로직 분리하기
TaskContainer의 구현 로직을 바깥으로 옮기고 이를 dispatch를 이용해 주입하면 구현 로직을 분리 할 수 있다.

![[useReducer 훅을 이용한 주입(draw).svg]]

구현은 다음과 같다.
```ts
export const ADD = "tasks/ADD" as const;
export const EDIT = "tasks/EDIT" as const;
export const DELETE = "tasks/DELETE" as const;


type TaskState = {
	id: number;
	text: string;
};


export const addTask = (text: string) => ({ type: ADD, payload: text });
export const editTask = (task: TaskState) => ({ type: EDIT, payload: task });
export const deleteTask = (id: number) => ({ type: DELETE, payload: id });

type TaskAction =
	| ReturnType<typeof addTask>
	| ReturnType<typeof editTask>
	| ReturnType<typeof deleteTask>;

export default function tasksReducer(tasks: TaskState[], action: TaskAction) {
	switch (action.type) {
		case ADD:
			return [...tasks, { id: tasks.length, text: action.payload }];
		case EDIT:
			return tasks.map(t =>
				t.id === action.payload.id
					? { ...t, text: action.payload.text }
					: t
			);
		case DELETE:
			return tasks.filter(task => task.id !== action.payload);
		default:
			return tasks;
	}
}
```

그리고 이를 useReducer를 이용해 외부에서 정의해둔 함수를 주입해서 사용한다.

```ts
import React, { useReducer, useState } from "react";
import AddTask from "./AddTask";
import TaskList from "./TaskList";
import tasksReducer, { ADD, DELETE, EDIT } from "./reducer/tasksReducer";


type TaskState = {
	id: number;
	text: string;
};

const initialTasks = [
	{ id: 0, text: "Learn React" },
	{ id: 1, text: "Learn Next.js" },
	{ id: 2, text: "Learn TypeScript" },
];


export default function TaskContainer() {
	const [tasks, dispath] = useReducer(tasksReducer, initialTasks);
	function handleAddTask(text: string) {
		dispath({
			type: ADD,
			payload: text,
		});
	}

	function handleEditTask(task: TaskState) {
		dispath({
			type: EDIT,
			payload: task,
		})
	}

	function handleDeleteTask(id: number) {
		dispath({
			type: DELETE,
			payload: id,
		});
	}
	return (
		<div className="m-4">
			<AddTask onAddTask={handleAddTask} />
			<TaskList
				tasks={tasks}
				onEditTask={handleEditTask}
				onDeleteTask={handleDeleteTask}
			/>
		</div>
	);
}
```


### Code
```ts
// AddTask.tsx

import React, { useState } from 'react';


interface TaskState {
	id: number;
	text: string;
}

interface AddTaskProps {
	onAddTask: (task: string) => void;
}


const AddTask: React.FC<AddTaskProps> = ({ onAddTask }) => {
	const [task, setTask] = useState('');
	const handleAddTask = () => {
		onAddTask(task);
		setTask('');
	};

	return (
		<div>
			<input
				type="text"
				value={task}
				onChange={(e) => setTask(e.target.value)}
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

```ts
// Task.tsx
import React, { useState } from "react";


interface TaskState {
	id: number;
	text: string;
}


interface TaskProps {
	task: TaskState;
	onEditTask: (task: TaskState) => void;
	onDeleteTask: (id: number) => void;
}


export default function Task({ task, onEditTask, onDeleteTask }: TaskProps) {
	const [editMode, setEditMode] = useState(false);
	const [editText, setEditText] = useState(task.text);
	
	const handleEditClick = () => {
		setEditMode(true);
	};
	
	
	const handleSaveClick = () => {
		onEditTask({ ...task, text: editText });
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
				onClick={() => onDeleteTask(task.id)}
>	
				Delete
			</button>
		</div>
	);
}
```


```ts
// TaskList.tsx

import React from 'react';
import Task from './Task';


type TaskState = {
	id: number;
	text: string;
}

type TaskListProps = {
	tasks: TaskState[];
	onEditTask: (task: TaskState) => void;
	onDeleteTask: (id: number) => void;
};

export default function TaskList({ tasks, onEditTask, onDeleteTask }: TaskListProps) {
	return (
		<div>
			{tasks.map((task, index) => (
				<Task
					key={index}
					task={task}
					onEditTask={onEditTask}
					onDeleteTask={onDeleteTask}
				/>
			))}
		</div>
	);
}
```
## 질문 & 확장
- 이 방법은 잘 사용하면 정말 좋은 방법인 것 같다. state가 많거나 이를 관리할 로직이 복잡해지는 경우 외부에 정의해서 주입하는 방식은 java 구현 코드가 많이 반복되거나 복잡한 로직을 분리해서 주입하는 전략 패턴과 유사해 마음에 들었다. 하지만 React에서 얼마나 자주 사용할 지 의문이 들긴 한다.

## 출처(링크)


## 연결 노트
