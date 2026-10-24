# ⚛️ React: useMemo & useCallback

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-useMemo%20%26%20useCallback-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`useMemo` and `useCallback` are performance optimization hooks in React.

| Hook          | Purpose                                      | Returns          |
|---------------|----------------------------------------------|------------------|
| `useMemo`     | Memoizes a **value**                         | Cached value     |
| `useCallback` | Memoizes a **function**                      | Cached function  |

They help avoid unnecessary re-calculations and re-renders.

---

## 🎯 Problem Statement

Create examples that demonstrate:

1. How `useMemo` prevents expensive calculations on every render
2. How `useCallback` prevents unnecessary re-creation of functions
3. A practical example with a child component that benefits from `React.memo` + `useCallback`

---

## 💻 Solution

### 1. useMemo Example (Expensive Calculation)

```jsx
import { useState, useMemo } from "react";

function ExpensiveCalculation() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  // This function is expensive (simulated)
  const expensiveValue = useMemo(() => {
    console.log("Calculating expensive value...");
    let result = 0;
    for (let i = 0; i < 100000000; i++) {
      result += i;
    }
    return result + count;
  }, [count]); // Only re-calculate when count changes

  return (
    <div>
      <h2>useMemo Example</h2>
      <p>Expensive Value: {expensiveValue}</p>
      <button onClick={() => setCount(count + 1)}>Increment Count</button>

      <br /><br />
      <input
        type="text"
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Type something..."
      />
      <p>Text: {text}</p>
    </div>
  );
}

export default ExpensiveCalculation;
```

---

### 2. useCallback Example

```jsx
import { useState, useCallback } from "react";

function Parent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  // Without useCallback → new function on every render
  // With useCallback → function is memoized
  const handleClick = useCallback(() => {
    console.log("Button clicked");
  }, []); // empty dependency → function created only once

  return (
    <div>
      <h2>useCallback Example</h2>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Type here"
      />
      <Child onClick={handleClick} />
    </div>
  );
}

function Child({ onClick }) {
  console.log("Child rendered");
  return <button onClick={onClick}>Click Me</button>;
}

export default Parent;
```

---

### 3. Best Practice: React.memo + useCallback

```jsx
import { useState, useCallback, memo } from "react";

const Child = memo(function Child({ onIncrement }) {
  console.log("Child rendered");
  return <button onClick={onIncrement}>Increment from Child</button>;
});

function Parent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  const increment = useCallback(() => {
    setCount((prev) => prev + 1);
  }, []);

  return (
    <div>
      <h2>Count: {count}</h2>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <Child onIncrement={increment} />
    </div>
  );
}
&
export default Parent;
```

---

## 🧠 Explanation

### useMemo
- Caches the **result** of a calculation
- Only re-computes when dependencies change
- Useful for expensive calculations

### useCallback
- Caches the **function itself**
- Returns the same function reference between renders (if dependencies haven’t changed)
- Extremely useful when passing functions to optimized child components (`React.memo`)

---

## 💡 Key Interview Points

- Difference between `useMemo` and `useCallback`
- When should you use them? (Don’t use them everywhere)
- What problem do they solve?
- How do they work with `React.memo`?
- Dependency array rules are the same as `useEffect`
- Overusing these hooks can make code slower and harder to read

---

## 🔥 Bonus Challenges

1. Create a filtered list of items using `useMemo`
2. Build a component that uses both `useMemo` and `useCallback` together
3. Explain when using these hooks can actually hurt performance
```
