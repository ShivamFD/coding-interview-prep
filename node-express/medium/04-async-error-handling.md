# 🛡️ Node.js + Express: Async Error Handling

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Async%20Error%20Handling-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

In Express, error handling works differently for synchronous and asynchronous code.

- Synchronous errors are automatically caught by Express.
- Asynchronous errors (from `async/await` or Promises) are **not** automatically caught.

If you don’t handle them properly, your server can crash or leave the request hanging.

### Common Problem

```js
app.get("/users", async (req, res) => {
  const users = await User.find(); // if this throws → server can crash
  res.json(users);
});
```

---

## 🎯 Problem Statement

Implement proper async error handling using two popular patterns:

1. **try/catch** inside every async route
2. **Wrapper function** (cleaner and recommended)

Also create a centralized error handling middleware.

---

## 💻 Solution

### 1. Basic Setup with Centralized Error Handler

```js
const express = require("express");
const app = express();

app.use(express.json());

// ============================================
// Sample async function that may fail
// ============================================
const getUsers = async () => {
  // Simulating a database call that fails
  throw new Error("Database connection failed");
};

// ============================================
// Pattern 1: try/catch in every route
// ============================================
app.get("/users-try-catch", async (req, res, next) => {
  try {
    const users = await getUsers();
    res.json(users);
  } catch (error) {
    next(error); // pass error to error handling middleware
  }
});

// ============================================
// Pattern 2: Async Wrapper Function (Recommended)
// ============================================
const asyncHandler = (fn) => {
  return (req, res, next) => {
    Promise.resolve(fn(req, res, next)).catch(next);
  };
};

// Now routes become very clean
app.get("/users", asyncHandler(async (req, res) => {
  const users = await getUsers();
  res.json(users);
}));

app.get("/users/:id", asyncHandler(async (req, res) => {
  const id = req.params.id;
  if (id > 10) {
    const error = new Error("User not found");
    error.statusCode = 404;
    throw error;
  }
  res.json({ id, name: "Rahul" });
}));

// ============================================
// Centralized Error Handling Middleware
// ============================================
app.use((err, req, res, next) => {
  const statusCode = err.statusCode || 500;

  res.status(statusCode).json({
    success: false,
    message: err.message || "Internal Server Error",
    // Only show stack in development
    stack: process.env.NODE_ENV === "development" ? err.stack : undefined
  });
});

// ============================================
// Start Server
// ============================================
const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

---

## 🧠 Explanation

### Why do we need special handling for async errors?

Express 4 does not catch rejected promises automatically.  
If an `await` throws and you don’t catch it, the error becomes an **unhandled promise rejection**.

### Two Solutions:

1. **try/catch** in every route → works but creates a lot of boilerplate.
2. **asyncHandler wrapper** → cleaner, reusable, and preferred in real projects.

### How the wrapper works:

```js
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};
```

- It takes your async function.
- Executes it.
- If it throws or rejects → automatically calls `next(error)`.

---

## 💡 Key Interview Points

- Why does Express not catch async errors by default?
- Difference between synchronous and asynchronous error handling
- What is the purpose of the `next` function in error handling?
- How does a centralized error middleware work?
- Best practice: always use a wrapper like `asyncHandler`
- How to send different status codes for different errors

---

## 🔥 Bonus Challenges

1. Create a custom `AppError` class that accepts `statusCode` and `message`
2. Add a 404 handler for undefined routes
3. Move the `asyncHandler` into a separate utility file
```
