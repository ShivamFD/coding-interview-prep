# 🛡️ Node.js + Express: Custom Middleware & Error Handling

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Middleware%20%2B%20Error%20Handling-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Middleware functions are functions that have access to the request object (`req`), response object (`res`), and the next middleware function in the cycle (`next`).

They are used for:
- Logging
- Authentication
- Validation
- Error handling
- Parsing request body
- etc.

In this problem we will implement:
1. A custom logger middleware
2. A simple authentication middleware
3. A centralized error handling middleware

---

## 🎯 Problem Statement

Create an Express application that includes:

1. A **Logger Middleware** that logs method, url, and time
2. A **Auth Middleware** that checks for a token in headers
3. Routes that use the auth middleware
4. A **Centralized Error Handler** middleware
5. Proper error responses

---

## 💻 Solution

```js
const express = require("express");
const app = express();

app.use(express.json());

// ============================================
// 1. Custom Logger Middleware
// ============================================
const logger = (req, res, next) => {
  const time = new Date().toLocaleString();
  console.log(`[${time}] ${req.method} ${req.url}`);
  next(); // pass control to the next middleware
};

// ============================================
// 2. Authentication Middleware
// ============================================
const auth = (req, res, next) => {
  const token = req.headers["authorization"];

  if (!token) {
    return res.status(401).json({ message: "No token provided" });
  }

  if (token !== "Bearer mysecrettoken") {
    return res.status(403).json({ message: "Invalid token" });
  }

  // Attach user info to request (optional)
  req.user = { id: 1, name: "Rahul" };
  next();
};

// ============================================
// Apply logger globally
// ============================================
app.use(logger);

// ============================================
// Public Route
// ============================================
app.get("/", (req, res) => {
  res.json({ message: "Welcome to the API" });
});

// ============================================
// Protected Routes
// ============================================
app.get("/profile", auth, (req, res) => {
  res.json({
    message: "This is a protected route",
    user: req.user
  });
});

app.get("/dashboard", auth, (req, res) => {
  res.json({ message: `Welcome to dashboard, ${req.user.name}` });
});

// ============================================
// Route that intentionally throws an error
// ============================================
app.get("/error", (req, res, next) => {
  const err = new Error("Something went wrong!");
  err.status = 500;
  next(err); // pass error to error handling middleware
});

// ============================================
// 3. Centralized Error Handling Middleware
// ============================================
// Note: Error handling middleware must have 4 parameters
app.use((err, req, res, next) => {
  const statusCode = err.status || 500;
  res.status(statusCode).json({
    success: false,
    message: err.message || "Internal Server Error"
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

### 1. Logger Middleware
- Runs on every request
- Logs method, URL and timestamp
- Calls `next()` to pass control forward

### 2. Auth Middleware
- Checks for `Authorization` header
- If token is missing → 401
- If token is wrong → 403
- If token is correct → attaches user to `req` and calls `next()`

### 3. Error Handling Middleware
- Must have **4 parameters**: `(err, req, res, next)`
- Catches any error passed via `next(err)`
- Sends a clean JSON error response

---

## 💡 Key Interview Points

- Middleware runs in the order they are defined
- Always call `next()` otherwise the request will hang
- Error handling middleware must have 4 arguments
- Difference between `app.use()` and route-specific middleware
- How to create reusable middleware
- How to protect routes using middleware

---

## 🔥 Bonus Challenges

1. Create a middleware that checks if the request body contains `name` and `email`
2. Create a rate limiter middleware (simple version using a counter)
3. Move routes into a separate `routes` folder using `express.Router()`
```
