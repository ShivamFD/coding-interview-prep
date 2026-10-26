# ⚛️ React: Conditional Rendering

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Conditional%20Rendering-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Conditional Rendering means showing different UI based on certain conditions.

In React, we commonly use:

- `if / else`
- Ternary operator (`? :`)
- Logical AND (`&&`)
- Early return

---

## 🎯 Problem Statement

Create a component called `LoginStatus` that:

1. Has a state `isLoggedIn`
2. Shows different messages and buttons based on login status
3. Uses multiple ways of conditional rendering

---

## 💻 Solution

```jsx
import { useState } from "react";

function LoginStatus() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [user, setUser] = useState(null);

  const handleLogin = () => {
    setIsLoggedIn(true);
    setUser({ name: "Rahul" });
  };

  const handleLogout = () => {
    setIsLoggedIn(false);
    setUser(null);
  };

  // Method 1: Early return
  if (!isLoggedIn) {
    return (
      <div>
        <h2>Please Login</h2>
        <button onClick={handleLogin}>Login</button>
      </div>
    );
  }

  return (
    <div>
      <h2>Welcome back!</h2>

      {/* Method 2: Ternary Operator */}
      <p>
        {user ? `Hello, ${user.name}` : "Hello, Guest"}
      </p>

      {/* Method 3: Logical AND (&&) */}
      {isLoggedIn && (
        <button onClick={handleLogout}>Logout</button>
      )}

      {/* Method 4: Ternary for two different elements */}
      {isLoggedIn ? (
        <p style={{ color: "green" }}>Status: Online</p>
      ) : (
        <p style={{ color: "red" }}>Status: Offline</p>
      )}
    </div>
  );
}

export default LoginStatus;
```

---

## 🧠 Explanation

### Different ways of Conditional Rendering:

1. **if / else or early return**  
   Best when the entire component changes.

2. **Ternary Operator (`? :`)**  
   Best when you want to choose between two elements.

3. **Logical AND (`&&`)**  
   Best when you want to show something only if a condition is true.

---

## 💡 Key Interview Points

- Difference between ternary and `&&`
- Why we should be careful with `&&` and falsy values (`0`, `""`)
- When to use early return
- Common patterns for loading states, error states, and authentication

---

## 🔥 Bonus Challenges

1. Add a loading state (`isLoading`) and show "Loading..." while true
2. Create a component that shows different UI for Admin vs Normal user
3. Show a warning message only when the user is logged in and has no profile picture
```
