# ⚛️ React: Accordion Component

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Accordion-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

An Accordion is a common UI pattern where only one (or multiple) sections can be expanded at a time.

This problem teaches you:
- Managing open/close state
- Conditional rendering
- Reusable component structure
- Handling single vs multiple open items

---

## 🎯 Problem Statement

Create an `Accordion` component that:

1. Displays a list of items (title + content)
2. Only one item can be open at a time
3. Clicking on an open item closes it
4. Has a clean and simple UI

---

## 💻 Solution

```jsx
import { useState } from "react";

const data = [
  {
    id: 1,
    title: "What is React?",
    content: "React is a JavaScript library for building user interfaces. It is component-based and declarative."
  },
  {
    id: 2,
    title: "What is a Component?",
    content: "A component is a reusable, independent piece of UI. Components can be functional or class-based."
  },
  {
    id: 3,
    title: "What is State?",
    content: "State is a built-in object that stores data about the component. When state changes, the component re-renders."
  },
  {
    id: 4,
    title: "What are Props?",
    content: "Props are read-only inputs passed from a parent component to a child component."
  }
];

function Accordion() {
  const [openId, setOpenId] = useState(null);

  const handleToggle = (id) => {
    setOpenId((prev) => (prev === id ? null : id));
  };

  return (
    <div style={{ maxWidth: "600px", margin: "40px auto", fontFamily: "sans-serif" }}>
      <h2>React Accordion</h2>

      {data.map((item) => {
        const isOpen = openId === item.id;

        return (
          <div
            key={item.id}
            style={{
              border: "1px solid #ddd",
              borderRadius: "6px",
              marginBottom: "10px",
              overflow: "hidden"
            }}
          >
            {/* Header */}
            <button
              onClick={() => handleToggle(item.id)}
              style={{
                width: "100%",
                padding: "14px 16px",
                textAlign: "left",
                background: isOpen ? "#f0f0f0" : "#fff",
                border: "none",
                cursor: "pointer",
                fontSize: "16px",
                fontWeight: "600",
                display: "flex",
                justifyContent: "space-between",
                alignItems: "center"
              }}
            >
              {item.title}
              <span>{isOpen ? "−" : "+"}</span>
            </button>

            {/* Content */}
            {isOpen && (
              <div style={{ padding: "14px 16px", background: "#fafafa", lineHeight: "1.5" }}>
                {item.content}
              </div>
            )}
          </div>
        );
      })}
    </div>
  );
}

export default Accordion;
```

---

## 🧠 Explanation

1. We keep a single state `openId` that stores the id of the currently open item.
2. When a header is clicked:
   - If it is already open → set `openId` to `null` (close it)
   - Otherwise → set `openId` to that item’s id
3. We conditionally render the content only when `openId === item.id`.
4. Using a unique `id` as key is important for list rendering.

---

## 💡 Key Interview Points

- How do you allow multiple items to be open at the same time?
- Difference between controlled and uncontrolled accordion
- How would you add animation to the open/close transition?
- How would you make this component reusable (accept data as props)?
- Accessibility considerations (ARIA attributes)

---

## 🔥 Bonus Challenges

1. Allow multiple accordion items to stay open at the same time
2. Make the Accordion controlled (accept `value` and `onChange` props)
3. Add smooth open/close animation using CSS
4. Extract a reusable `AccordionItem` component
```
