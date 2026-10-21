# ⚛️ React: Custom Hooks

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Custom%20Hooks-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

A **Custom Hook** is a JavaScript function whose name starts with `"use"` and that can call other Hooks.

Custom Hooks let you extract component logic into reusable functions.

### Why use Custom Hooks?
- Reuse stateful logic across multiple components
- Keep components clean and focused
- Share logic without using higher-order components or render props

### Rules of Custom Hooks
- Name must start with `use` (e.g., `useFetch`, `useLocalStorage`)
- Can call other Hooks (useState, useEffect, etc.)
- Follow the Rules of Hooks

---

## 🎯 Problem Statement

Create the following two custom hooks and use them in components:

1. **`useToggle`** → A hook to toggle a boolean value
2. **`useLocalStorage`** → A hook that syncs state with localStorage

---

## 💻 Solution

### 1. useToggle Hook

```jsx
import { useState } from "react";

function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);

  const toggle = () => setValue((prev) => !prev);
  const setTrue = () => setValue(true);
  const setFalse = () => setValue(false);

  return [value, toggle, setTrue, setFalse];
}

export default useToggle;
```

**Usage:**

```jsx
import useToggle from "./useToggle";

function ToggleExample() {
  const [isOpen, toggle, open, close] = useToggle(false);

  return (
    <div>
      <p>Status: {isOpen ? "Open" : "Closed"}</p>
      <button onClick={toggle}>Toggle</button>
      <button onClick={open}>Open</button>
      <button onClick={close}>Close</button>
    </div>
  );
}
```

---

### 2. useLocalStorage Hook

```jsx
import { useState, useEffect } from "react";

function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    try {
      const stored = localStorage.getItem(key);
      return stored ? JSON.parse(stored) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });

  useEffect(() => {
    try {
      localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error("Error saving to localStorage", error);
    }
  }, [key, value]);

  return [value, setValue];
}

export default useLocalStorage;
```

**Usage:**

```jsx
import useLocalStorage from "./useLocalStorage";

function LocalStorageExample() {
  const [name, setName] = useLocalStorage("username", "");

  return (
    <div>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Enter your name"
      />
      <p>Hello, {name || "Guest"}!</p>
    </div>
  );
}
```

---

## 🧠 Explanation

### useToggle
- Manages a boolean state
- Returns the current value + helper functions (`toggle`, `setTrue`, `setFalse`)
- Very useful for modals, dropdowns, dark mode, etc.

### useLocalStorage
- Initializes state from `localStorage` (if available)
- Automatically updates `localStorage` whenever the state changes
- Handles JSON parsing and error cases
- Extremely common in real applications

---

## 💡 Key Interview Points

- Custom Hooks must start with the word `use`
- They allow you to reuse stateful logic
- They do not share state between components (each call has its own state)
- Difference between Custom Hooks and regular functions
- How would you create a `useFetch` custom hook?
- Rules of Hooks still apply inside custom hooks

---

## 🔥 Bonus Challenges

1. Create a `useFetch(url)` custom hook that returns `{ data, loading, error }`
2. Create a `useCounter(initialValue, step)` hook
3. Improve `useLocalStorage` to support removing the item
```
