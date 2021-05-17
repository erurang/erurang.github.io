---
layout: post
title:  "ToDoList로 연습해보기"
subtitle: "ToDoList로 연습해보기"
categories: web
tags: redux
comments: true

---

### ToDoList 만들어보기

이 글은 순전히 연습을 위한 글이기 때문에 상세설명은 없을예정, 해보다가 안되고 놓쳤던 부분을 공부하기위한 글임.

#### 모듈에 리덕스의 액션과 리듀서를 파일을 만들어주기

```
// 액션 
const ADD_TODO = "todos/ADD_TODO";
const TOGGLE_TODO = "todos/TOGGLE_TODO";

// 추가
let nextId = 1;
export const addTodo = (text) => ({
  type: ADD_TODO,
  todo: {
    id: nextId++,
    text,
  },
});

export const toggleTodo = (id) => ({
  type: TOGGLE_TODO,
  id,
});

const initialState = [
  // 데이터의 객체가 들어갈예정
];

export default function todos(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO:
      return state.concat(action.todo);
    case TOGGLE_TODO:
      return state.map((todo) =>
        todo.id === action.id ? { ...todo, done: !todo.done } : todo
      );

    default:
      return state;
  }
}
```

#### 투두 컨테이너 만들기

```
import React from "react";
import Todos from "../components/Todos";

import { useSelector, useDispatch } from "react-redux";
import { addTodo, toggleTodo } from "../modules/todos";

const TodosContainer = () => {
  const todos = useSelector((state) => state.todos);
  const dispatch = useDispatch();

  const onCreate = (text) => dispatch(addTodo(text));
  const onToggle = (id) => dispatch(toggleTodo(id));

  return <Todos todos={todos} onCreate={onCreate} onToggle={onToggle} />;
};

export default TodosContainer;

```

#### 투두 프레젠터 만들기

```
import React, { useState } from "react";

const TodoItem = ({ todo, onToggle }) => {
  return (
    <li
      style={{ textDecoration: todo.done ? "line-through" : "none" }}
      onClick={() => onToggle(todo.id)}
    >
      {todo.text}
    </li>
  );
};

function TodoList({ todos, onToggle }) {
  return (
    <ul>
      {todos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} onToggle={onToggle} />
      ))}
    </ul>
  );
}

function Todos({ todos, onCreate, onToggle }) {
  const [text, setText] = useState("");

  const onChange = (e) => {
    setText(e.target.value);
  };

  const onSubmit = (e) => {
    e.preventDefault();
    onCreate(text);
    setText("");
  };

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input value={text} onChange={onChange}></input>
        <button type="submit">등록</button>
      </form>
      <TodoList todos={todos} onToggle={onToggle} />
    </div>
  );
}

export default Todos;
```

![todo](https://user-images.githubusercontent.com/56789064/118530700-e96e9480-b77f-11eb-90c3-63117f2899f3.gif)

작동잘하네!! 다음으로 해볼것은 리덕스사가/썽크 인데...

이걸 해보기 전까지는 어떻게 리덕스에 연결해서 데이터를 따로쏴주는지 감을 못잡고있었는데
컴포넌트를 구분하고, 리덕스를 따로 만들어서 상태를 관리한다는것을 확실히 배우게되었지 않았나 싶다.