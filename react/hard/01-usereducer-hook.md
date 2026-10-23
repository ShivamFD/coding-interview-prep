# ⚛️ React: useReducer Hook

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-useReducer-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`useReducer` is a React Hook used for managing **complex state logic**.

It is an alternative to `useState` and is preferred when:
- State has multiple sub-values
- Next state depends on the previous state
- State update logic is complex

### Basic Syntax

```js
const [state, dispatch] = useReducer(reducer, initialState);
```

- `reducer` → a pure function `(state, action) => newState`
- `dispatch` → function used to send actions

---

## 🎯 Problem Statement

Create a **Todo App** using `useReducer` instead of multiple `useState`.

Features required:
1. Add a new todo
2. Toggle todo completion
3. Delete a todo
4. Clear all completed todos
5. Show count of active todos

---

## 💻 Solution

```jsx
import { useReducer, useState } from "react";

// 1. Initial State
const initialState = {
  todos: [],
  nextId: 1
};

// 2. Reducer Function
function todoReducer(state, action) {
  switch (action.type) {
    case "ADD_TODO":
      return {
        ...state,
        todos: [
          ...state.todos,
          {
            id: state.nextId,
            text: action.payload,
            completed: false
          }
        ],
        nextId: state.nextId + 1
      };

    case "TOGGLE_TODO":
      return {
        ...state,
        todos: state.todos.map((todo) =>
          todo.id === action.payload
            ? { ...todo, completed: !todo.completed }
            : todo
        )
      };

    case "DELETE_TODO":
      return {
        ...state,
        todos: state.todos.filter((todo) => todo.id !== action.payload)
      };

    case "CLEAR_COMPLETED":
      return {
        ...state,
        todos: state.todos.filter((todo) => !todo.completed)
      };

    default:
      return state;
  }
}

// 3. Component
function TodoApp() {
  const [state, dispatch] = useReducer(todoReducer, initialState);
  const [input, setInput] = useState("");

  const handleAdd = () => {
    if (input.trim() === "") return;
    dispatch({ type: "ADD_TODO", payload: input.trim() });
    setInput("");
  };

  const activeCount = state.todos.filter((todo) => !todo.completed).length;

  return (
    <div>
      <h2>Todo App (useReducer)</h2>

      <input
        type="text"
        value={input}
        onChange={(e) => setInput(e.target.value)}
        onKeyDown={(e) => e.key === "Enter" && handleAdd()}
        placeholder="Add a new todo"
      />
      <button onClick={handleAdd}>Add</button>

      <p>Active Todos: {activeCount}</p>

      <ul>
        {state.todos.map((todo) => (
          <li key={todo.id}>
            <span
              style={{
                textDecoration: todo.completed ? "line-through" : "none",
                cursor: "pointer"
              }}
              onClick={() => dispatch({ type: "TOGGLE_TODO", payload: todo.id })}
            >
              {todo.text}
            </span>
            <button onClick={() => dispatch({ type: "DELETE_TODO", payload: todo.id })}>
              Delete
            </button>
          </li>
        ))}
      </ul>

      <button onClick={() => dispatch({ type: "CLEAR_COMPLETED" })}>
        Clear Completed
      </button>
    </div>
  );
}

export default TodoApp;
```

---

## 🧠 Explanation

### How useReducer works:

1. We define a `reducer` function that receives current `state` and an `action`.
2. Based on `action.type`, it returns a **new state**.
3. We call `dispatch({ type: "...", payload: ... })` to trigger updates.
4. React calls the reducer and re-renders the component with the new state.

### Why useReducer is better here:
- All state update logic is centralized in one place (the reducer)
- Easier to test
- More predictable than multiple `useState` calls
- Scales better as the app grows

---

## 💡 Key Interview Points

- Difference between `useState` and `useReducer`
- When should you prefer `useReducer`?
- What is an action object? Why do we use `type` and `payload`?
- Can we use `useReducer` with Context API? (Yes – very common pattern)
- Is the reducer required to be a pure function? (Yes)
- How does `useReducer` help avoid stale closures?

---

## 🔥 Bonus Challenges

1. Add an "Edit Todo" feature
2. Add a filter (All / Active / Completed)
3. Persist todos in `localStorage` using `useEffect`
4. Convert this into a global state using `useReducer` + `useContext`
```
