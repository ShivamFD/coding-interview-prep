# 📝 React: Todo List with Add & Delete

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-useState%20%2B%20Lists-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

In this problem, we will build a classic **Todo List** component using React.

**Features required:**
- Add a new todo
- Display the list of todos
- Delete a todo
- Prevent adding empty todos

This is a very common interview question to test:
- `useState` with arrays
- Controlled inputs
- Rendering lists with unique keys
- Updating state immutably

---

## 🎯 Problem Statement

Create a functional component called `TodoList` that:

1. Has an input field and an "Add" button
2. Shows all added todos in a list
3. Each todo has a "Delete" button
4. Does not allow empty todos to be added
5. Uses only `useState` (no external libraries)

---

## 💻 Solution

```jsx
import { useState } from "react";

function TodoList() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState("");

  const handleAdd = () => {
    if (input.trim() === "") return; // prevent empty todos

    const newTodo = {
      id: Date.now(),
      text: input.trim()
    };

    setTodos([...todos, newTodo]);
    setInput(""); // clear input
  };

  const handleDelete = (id) => {
    setTodos(todos.filter((todo) => todo.id !== id));
  };

  return (
    <div>
      <h2>Todo List</h2>

      <input
        type="text"
        placeholder="Enter a todo"
        value={input}
        onChange={(e) => setInput(e.target.value)}
        onKeyDown={(e) => e.key === "Enter" && handleAdd()}
      />
      <button onClick={handleAdd}>Add</button>

      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>
            {todo.text}
            <button onClick={() => handleDelete(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TodoList;
```

---

## 🧠 Explanation

- We use two pieces of state:
  - `todos` → array of todo objects
  - `input` → controlled input value

- When adding a todo:
  - We create a new object with a unique `id` (`Date.now()`)
  - We use the spread operator `[...todos, newTodo]` to keep the state immutable

- When deleting:
  - We use `.filter()` to create a new array without the deleted item

- Always provide a unique `key` prop when rendering lists.

---

## 💡 Key Interview Points

- Why do we need a unique `id`? (Because index as key is not recommended)
- Why do we use spread operator / filter instead of `push` / `splice`? (Immutability)
- How to clear the input after adding?
- How to prevent empty todos?

---

## 🔥 Bonus Challenges

1. Add a "Clear All" button
2. Show total number of todos
3. Disable the Add button when input is empty
4. Add a checkbox to mark todo as completed
```
