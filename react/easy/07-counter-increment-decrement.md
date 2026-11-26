# ⚛️ React: Counter with Increment, Decrement & Reset

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Counter-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

A counter is one of the best ways to practice `useState` and event handling in React.

In this problem you will learn:
- Updating state based on previous value
- Handling multiple buttons
- Conditional styling (optional)

---

## 🎯 Problem Statement

Create a `Counter` component that:

1. Shows the current count
2. Has an **Increment** button
3. Has a **Decrement** button
4. Has a **Reset** button
5. Does not allow the count to go below 0

---

## 💻 Solution

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  const handleIncrement = () => {
    setCount((prev) => prev + 1);
  };

  const handleDecrement = () => {
    setCount((prev) => (prev > 0 ? prev - 1 : 0));
  };

  const handleReset = () => {
    setCount(0);
  };

  return (
    <div style={{ textAlign: "center", marginTop: "50px", fontFamily: "sans-serif" }}>
      <h2>Counter App</h2>

      <h1 style={{ fontSize: "48px", margin: "20px 0" }}>{count}</h1>

      <div style={{ display: "flex", gap: "10px", justifyContent: "center" }}>
        <button onClick={handleIncrement} style={buttonStyle}>
          Increment
        </button>

        <button onClick={handleDecrement} style={buttonStyle}>
          Decrement
        </button>

        <button onClick={handleReset} style={{ ...buttonStyle, backgroundColor: "#ff4d4d" }}>
          Reset
        </button>
      </div>
    </div>
  );
}

const buttonStyle = {
  padding: "10px 18px",
  fontSize: "16px",
  cursor: "pointer",
  border: "none",
  borderRadius: "6px",
  backgroundColor: "#4CAF50",
  color: "white"
};

export default Counter;
```

---

## 🧠 Explanation

- We use `useState(0)` to create a state variable called `count`.
- We always update state using the function form `setCount(prev => ...)` when the new value depends on the old value. This is the recommended way.
- In `handleDecrement` we make sure the count never goes below zero.
- Each button has its own event handler for clarity.

---

## 💡 Key Interview Points

- Why should we use the function form of `setState` (`prev => prev + 1`)?
- What happens if we write `setCount(count + 1)` instead?
- How would you add a step value (e.g. +5 or -5)?
- Difference between state and props

---

## 🔥 Bonus Challenges

1. Add a step input so the user can choose how much to increment/decrement
2. Change the color of the count when it is greater than 10
3. Disable the Decrement button when count is 0
```
