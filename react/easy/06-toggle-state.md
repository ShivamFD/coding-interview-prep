# ⚛️ React: Toggle State (Boolean)

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Toggle%20State-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Toggling a boolean value is one of the most common patterns in React.

We use it for:
- Show / Hide content
- Open / Close modal or dropdown
- Dark / Light mode
- Expand / Collapse sections

---

## 🎯 Problem Statement

Create a component that:

1. Has a boolean state `isVisible`
2. Shows or hides a message based on that state
3. Has a button to toggle the visibility

---

## 💻 Solution

```jsx
import { useState } from "react";

function ToggleMessage() {
  const [isVisible, setIsVisible] = useState(false);

  const handleToggle = () => {
    setIsVisible(!isVisible);
  };

  return (
    <div>
      <h2>Toggle Example</h2>

      <button onClick={handleToggle}>
        {isVisible ? "Hide Message" : "Show Message"}
      </button>

      {isVisible && (
        <p style={{ marginTop: "15px", color: "green" }}>
          Hello! This message is now visible.
        </p>
      )}
    </div>
  );
}

export default ToggleMessage;
```

---

### Alternative: Using previous state (Recommended)

```jsx
const handleToggle = () => {
  setIsVisible((prev) => !prev);
};
```

This is safer when the new state depends on the old state.

---

## 🧠 Explanation

- `useState(false)` creates a boolean state.
- `setIsVisible(!isVisible)` flips the value between `true` and `false`.
- We use conditional rendering (`{isVisible && ...}`) to show the message only when the state is `true`.
- The button text also changes based on the current state.

---

## 💡 Key Interview Points

- How do you toggle a boolean state in React?
- Why is using the previous state function (`prev => !prev`) better?
- Difference between `{condition && <Component />}` and ternary operator
- Common real-world uses of toggle state

---

## 🔥 Bonus Challenges

1. Create a dark/light mode toggle
2. Create an accordion-style expand/collapse component
3. Disable the button for 1 second after each click
```
