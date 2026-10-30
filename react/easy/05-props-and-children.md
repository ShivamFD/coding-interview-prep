# ⚛️ React: Props and Children

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Props%20%26%20Children-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

**Props** (short for properties) are how we pass data from a parent component to a child component.

Props are **read-only**. A child component cannot modify the props it receives.

`children` is a special prop that allows components to wrap other components or content.

---

## 🎯 Problem Statement

Create the following components:

1. A `Greeting` component that accepts `name` and `age` as props
2. A `Card` component that uses the `children` prop to wrap content
3. A parent component that uses both

---

## 💻 Solution

```jsx
// Greeting Component
function Greeting({ name, age }) {
  return (
    <div>
      <h2>Hello, {name}!</h2>
      <p>You are {age} years old.</p>
    </div>
  );
}

// Card Component (using children)
function Card({ title, children }) {
  return (
    <div style={{
      border: "1px solid #ccc",
      borderRadius: "8px",
      padding: "16px",
      margin: "10px 0",
      boxShadow: "0 2px 5px rgba(0,0,0,0.1)"
    }}>
      <h3>{title}</h3>
      <div>{children}</div>
    </div>
  );
}

// Parent Component
function App() {
  return (
    <div style={{ maxWidth: "500px", margin: "40px auto" }}>
      <h1>Props & Children Example</h1>

      <Greeting name="Rahul" age={28} />
      <Greeting name="Priya" age={25} />

      <Card title="User Profile">
        <p>Name: Rahul Sharma</p>
        <p>Role: Frontend Developer</p>
        <button>Edit Profile</button>
      </Card>

      <Card title="Notifications">
        <ul>
          <li>New message received</li>
          <li>Your post was liked</li>
        </ul>
      </Card>
    </div>
  );
}

export default App;
```

---

## 🧠 Explanation

- Props are passed as attributes: `<Greeting name="Rahul" age={28} />`
- Inside the component, we destructure them: `{ name, age }`
- `children` contains whatever is placed between the opening and closing tags of a component
- This pattern is extremely common for reusable UI components (Cards, Modals, Layouts, etc.)

---

## 💡 Key Interview Points

- Props are read-only (one-way data flow)
- What is the difference between props and state?
- What is the `children` prop?
- How do you pass a function as a prop?
- What happens if you try to modify a prop inside a child component?
- Default props and PropTypes (or TypeScript)

---

## 🔥 Bonus Challenges

1. Create a `Button` component that accepts `label`, `onClick`, and `color` as props
2. Create a `Modal` component that uses `children` for content
3. Pass a function as a prop from parent to child and call it
```
