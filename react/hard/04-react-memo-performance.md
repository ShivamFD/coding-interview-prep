# ⚛️ React: React.memo & Performance Optimization

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-React.memo%20%26%20Performance-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

By default, when a parent component re-renders, all of its child components also re-render — even if their props haven’t changed.

`React.memo` is a higher-order component that memoizes a functional component and only re-renders it when its props change.

Combined with `useCallback` and `useMemo`, it becomes a powerful performance optimization tool.

---

## 🎯 Problem Statement

Create an example that demonstrates:

1. Unnecessary re-renders of a child component
2. How `React.memo` prevents those re-renders
3. How `useCallback` is required when passing functions as props

---

## 💻 Solution

```jsx
import { useState, useCallback, memo } from "react";

// ---------------- Child Component ----------------
const Child = memo(function Child({ name, onClick }) {
  console.log("Child rendered");

  return (
    <div style={{ border: "1px solid #ccc", padding: "10px", marginTop: "10px" }}>
      <p>Child Name: {name}</p>
      <button onClick={onClick}>Click Me</button>
    </div>
  );
});

// ---------------- Parent Component ----------------
function Parent() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState("Rahul");

  // Without useCallback → new function every render → Child re-renders
  // With useCallback → same function reference → Child does not re-render
  const handleClick = useCallback(() => {
    console.log("Button clicked");
  }, []);

  return (
    <div>
      <h2>React.memo Performance Example</h2>

      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment Count</button>

      <br /><br />

      <input
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Change name"
      />

      <Child name={name} onClick={handleClick} />
    </div>
  );
}

export default Parent;
```

---

## 🧠 Explanation

### What happens without React.memo?
Every time the parent re-renders (when `count` changes), the `Child` component also re-renders — even if its props (`name` and `onClick`) stay the same.

### What React.memo does?
It performs a **shallow comparison** of props.  
If props are the same, React skips rendering the component.

### Why do we need useCallback?
Functions are recreated on every render.  
Even if the logic is the same, a new function reference is created, which makes `React.memo` think that props have changed.

`useCallback` keeps the same function reference between renders.

---

## 💡 Key Interview Points

- What is `React.memo` and how does it work?
- Difference between `React.memo`, `useMemo`, and `useCallback`
- When should you use `React.memo`? (Not everywhere)
- Why does passing an inline function break memoization?
- What is shallow comparison?
- Common performance mistakes in React

---

## 🔥 Bonus Challenges

1. Add a second child component and optimize both
2. Use `useMemo` to optimize an expensive calculation in the parent
3. Create a custom comparison function for `React.memo`
4. Explain when using `React.memo` can actually hurt performance
```
