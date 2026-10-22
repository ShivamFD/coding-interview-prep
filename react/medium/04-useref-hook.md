# ⚛️ React: useRef Hook

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-useRef-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`useRef` is a React Hook that lets you create a **mutable reference** that persists across re-renders.

### Two main use cases of useRef:

1. **Accessing DOM elements** (most common)
2. **Storing mutable values** that do not cause a re-render when changed

### Basic Syntax

```js
const myRef = useRef(initialValue);

// Access the value
myRef.current
```

---

## 🎯 Problem Statement

Create the following examples using `useRef`:

1. Focus an input field when a button is clicked
2. Count how many times a component has rendered without causing extra re-renders
3. Create a simple stopwatch (start / stop) using useRef to store the interval ID

---

## 💻 Solutions

### 1. Focus Input on Button Click

```jsx
import { useRef } from "react";

function FocusInput() {
  const inputRef = useRef(null);

  const handleFocus = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Click the button to focus" />
      <button onClick={handleFocus}>Focus Input</button>
    </div>
  );
}

export default FocusInput;
```

---

### 2. Track Render Count (without causing re-render)

```jsx
import { useState, useRef, useEffect } from "react";

function RenderCounter() {
  const [count, setCount] = useState(0);
  const renderCount = useRef(1);

  useEffect(() => {
    renderCount.current = renderCount.current + 1;
  });

  return (
    <div>
      <p>Count: {count}</p>
      <p>This component has rendered {renderCount.current} times</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default RenderCounter;
```

---

### 3. Simple Stopwatch using useRef

```jsx
import { useState, useRef } from "react";

function Stopwatch() {
  const [time, setTime] = useState(0);
  const [isRunning, setIsRunning] = useState(false);
  const intervalRef = useRef(null);

  const start = () => {
    if (!isRunning) {
      setIsRunning(true);
      intervalRef.current = setInterval(() => {
        setTime((prev) => prev + 1);
      }, 1000);
    }
  };

  const stop = () => {
    setIsRunning(false);
    clearInterval(intervalRef.current);
  };

  const reset = () => {
    stop();
    setTime(0);
  };

  return (
    <div>
      <h2>Time: {time}s</h2>
      <button onClick={start}>Start</button>
      <button onClick={stop}>Stop</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}

export default Stopwatch;
```

---

## 🧠 Explanation

### Why useRef instead of useState?

- Changing `ref.current` does **not** trigger a re-render.
- Perfect for values that need to persist but shouldn’t cause the UI to update.
- Ideal for storing timer IDs, previous values, DOM nodes, etc.

### Key Points:
- `useRef` returns an object with a `.current` property.
- The value of `.current` can be changed freely.
- Unlike state, updating a ref does not re-render the component.

---

## 💡 Key Interview Points

- Difference between `useRef` and `useState`
- When should you use `useRef`?
- Can `useRef` replace `useState`? (No, because it doesn’t trigger re-render)
- How to access DOM elements using useRef
- How to store previous props/state using useRef
- Common use cases: timers, focus management, storing previous values

---

## 🔥 Bonus Challenges

1. Create a component that tracks the previous value of a state using useRef
2. Build a component that auto-focuses an input when it mounts
3. Create a “Click outside to close” dropdown using useRef
```
