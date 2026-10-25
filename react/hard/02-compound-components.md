# ⚛️ React: Compound Components Pattern

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Compound%20Components-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

The **Compound Components** pattern is an advanced React pattern that allows multiple components to work together as a single unit while sharing implicit state.

It provides a clean and flexible API (similar to how native HTML elements like `<select>` and `<option>` work).

### Real-world examples:
- Accordion
- Tabs
- Dropdown Menu
- Modal with Header/Body/Footer
- Toggle Group

### Why use this pattern?
- Flexible composition
- Clean public API
- Better separation of concerns
- More control for the consumer of the component

---

## 🎯 Problem Statement

Build a flexible **Accordion** component using the Compound Components pattern.

Requirements:
- `Accordion` (parent)
- `Accordion.Item`
- `Accordion.Header`
- `Accordion.Panel`
- Only one item can be open at a time
- Clean and readable usage

---

## 💻 Solution

```jsx
import { createContext, useContext, useState } from "react";

// 1. Create Context
const AccordionContext = createContext();
const AccordionItemContext = createContext();

// 2. Parent Component
function Accordion({ children, defaultIndex = 0 }) {
  const [openIndex, setOpenIndex] = useState(defaultIndex);

  return (
    <AccordionContext.Provider value={{ openIndex, setOpenIndex }}>
      <div className="accordion">{children}</div>
    </AccordionContext.Provider>
  );
}

// 3. Item Component
function AccordionItem({ children, index }) {
  return (
    <AccordionItemContext.Provider value={{ index }}>
      <div className="accordion-item">{children}</div>
    </AccordionItemContext.Provider>
  );
}

// 4. Header Component
function AccordionHeader({ children }) {
  const { openIndex, setOpenIndex } = useContext(AccordionContext);
  const { index } = useContext(AccordionItemContext);

  const isOpen = openIndex === index;

  return (
    <button
      onClick={() => setOpenIndex(isOpen ? null : index)}
      style={{
        width: "100%",
        padding: "12px",
        textAlign: "left",
        fontWeight: "bold",
        cursor: "pointer",
        background: isOpen ? "#e0e0e0" : "#f5f5f5",
        border: "1px solid #ccc"
      }}
    >
      {children} {isOpen ? "−" : "+"}
    </button>
  );
}

// 5. Panel Component
function AccordionPanel({ children }) {
  const { openIndex } = useContext(AccordionContext);
  const { index } = useContext(AccordionItemContext);

  if (openIndex !== index) return null;

  return (
    <div style={{ padding: "12px", border: "1px solid #ccc", borderTop: "none" }}>
      {children}
    </div>
  );
}

// 6. Attach sub-components
Accordion.Item = AccordionItem;
Accordion.Header = AccordionHeader;
Accordion.Panel = AccordionPanel;

export default Accordion;
```

---

### Usage Example

```jsx
import Accordion from "./Accordion";

function App() {
  return (
    <div style={{ maxWidth: "500px", margin: "50px auto" }}>
      <h2>Compound Components - Accordion</h2>

      <Accordion defaultIndex={0}>
        <Accordion.Item index={0}>
          <Accordion.Header>What is React?</Accordion.Header>
          <Accordion.Panel>
            React is a JavaScript library for building user interfaces.
          </Accordion.Panel>
        </Accordion.Item>

        <Accordion.Item index={1}>
          <Accordion.Header>What is useState?</Accordion.Header>
          <Accordion.Panel>
            useState is a Hook that lets you add state to functional components.
          </Accordion.Panel>
        </Accordion.Item>

        <Accordion.Item index={2}>
          <Accordion.Header>What are Compound Components?</Accordion.Header>
          <Accordion.Panel>
            A pattern where components work together to form a complete UI unit.
          </Accordion.Panel>
        </Accordion.Item>
      </Accordion>
    </div>
  );
}

export default App;
```

---

## 🧠 Explanation

1. We use **two contexts**:
   - One for the parent (`openIndex`)
   - One for each item (`index`)

2. Sub-components (`Header`, `Panel`) consume the context to know whether they should be open or closed.

3. By attaching components as static properties (`Accordion.Item`, `Accordion.Header`), we create a clean and elegant API.

4. The consumer of the component has full control over the structure and styling.

---

## 💡 Key Interview Points

- What problem does the Compound Components pattern solve?
- Difference between Compound Components and Render Props
- How does context help in this pattern?
- Pros and cons of this pattern
- Real-world libraries that use this pattern (Radix UI, Headless UI, Chakra UI, React Aria)
- How would you make multiple items open at the same time?

---

## 🔥 Bonus Challenges

1. Allow multiple accordion items to be open at the same time
2. Add animation when opening/closing the panel
3. Build a Tabs component using the same Compound Components pattern
4. Make the Accordion controlled (accept `value` and `onChange` props)
```
