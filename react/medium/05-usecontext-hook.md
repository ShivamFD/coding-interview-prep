# ⚛️ React: useContext Hook

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-useContext-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`useContext` is a React Hook that lets you read and subscribe to context from your component.

It is used to **avoid prop drilling** (passing props through many levels of components).

### When to use Context?
- Theme (dark/light mode)
- Current authenticated user
- Language / localization
- Any global state that many components need

### Basic Syntax

```js
const value = useContext(MyContext);
```

---

## 🎯 Problem Statement

Create a simple Theme Switcher application using Context API:

1. Create a `ThemeContext`
2. Provide a theme value (`light` or `dark`) and a toggle function
3. Consume the context in multiple components using `useContext`
4. Change the background and text color based on the theme

---

## 💻 Solution

### 1. ThemeContext.js

```jsx
import { createContext, useContext, useState } from "react";

const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");

  const toggleTheme = () => {
    setTheme((prev) => (prev === "light" ? "dark" : "light"));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  return useContext(ThemeContext);
}
```

---

### 2. App.js

```jsx
import { ThemeProvider } from "./ThemeContext";
import Header from "./Header";
import Content from "./Content";

function App() {
  return (
    <ThemeProvider>
      <Header />
      <Content />
    </ThemeProvider>
  );
}

export default App;
```

---

### 3. Header.js

```jsx
import { useTheme } from "./ThemeContext";

function Header() {
  const { theme, toggleTheme } = useTheme();

  return (
    <header
      style={{
        padding: "20px",
        backgroundColor: theme === "light" ? "#f0f0f0" : "#333",
        color: theme === "light" ? "#000" : "#fff"
      }}
    >
      <h1>My App</h1>
      <button onClick={toggleTheme}>
        Switch to {theme === "light" ? "Dark" : "Light"} Mode
      </button>
    </header>
  );
}

export default Header;
```

---

### 4. Content.js

```jsx
import { useTheme } from "./ThemeContext";

function Content() {
  const { theme } = useTheme();

  return (
    <main
      style={{
        padding: "40px",
        minHeight: "300px",
        backgroundColor: theme === "light" ? "#ffffff" : "#222",
        color: theme === "light" ? "#000" : "#fff"
      }}
    >
      <h2>Current Theme: {theme}</h2>
      <p>This content changes based on the theme.</p>
    </main>
  );
}

export default Content;
```

---

## 🧠 Explanation

1. We create a Context using `createContext()`.
2. We wrap the part of the tree that needs the data with a `Provider`.
3. Any child component can access the value using `useContext` (or a custom hook like `useTheme`).
4. When the context value changes, all consuming components re-render.

### Benefits of the custom hook (`useTheme`)
- Cleaner imports
- Easier to add error handling later
- Better developer experience

---

## 💡 Key Interview Points

- What problem does Context API solve? (Prop drilling)
- Difference between Context API and Redux / Zustand
- When should you **not** use Context? (For high-frequency updates)
- Can we use multiple contexts?
- How does `useContext` cause re-renders?
- Best practices: Create a custom hook for each context

---

## 🔥 Bonus Challenges

1. Add a Language Context (English / Hindi) along with Theme
2. Persist the theme in `localStorage` so it survives page refresh
3. Create a User Context that stores logged-in user information
```
