# 📝 Node.js + Express: Basic Custom Middleware (Logger)

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Custom%20Middleware-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Middleware functions are functions that have access to the request (`req`), response (`res`), and the next middleware function (`next`).

They sit in the middle of the request-response cycle and can:
- Execute any code
- Modify request or response objects
- End the request-response cycle
- Call the next middleware

In this problem we will create a simple **Logger Middleware**.

---

## 🎯 Problem Statement

Create an Express server that uses a custom logger middleware.

The logger should:
1. Print the HTTP method
2. Print the requested URL
3. Print the current time
4. Call `next()` so the request can continue

---

## 💻 Solution

```js
const express = require("express");
const app = express();

// ============================================
// Custom Logger Middleware
// ============================================
const logger = (req, res, next) => {
  const time = new Date().toLocaleString();
  console.log(`[${time}] ${req.method} ${req.url}`);
  next(); // Very important - pass control to the next middleware/route
};

// Apply middleware globally
app.use(logger);

// ============================================
// Routes
// ============================================
app.get("/", (req, res) => {
  res.json({ message: "Welcome to the home page" });
});

app.get("/about", (req, res) => {
  res.json({ message: "This is the about page" });
});

app.get("/users", (req, res) => {
  res.json({ message: "List of users" });
});

// ============================================
// Start Server
// ============================================
const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

---

### Expected Console Output (when you visit routes):

```
[17/11/2026, 2:45:00 pm] GET /
[17/11/2026, 2:45:05 pm] GET /about
[17/11/2026, 2:45:10 pm] GET /users
```

---

## 🧠 Explanation

- Middleware is just a normal function with three parameters: `req`, `res`, `next`.
- `app.use(logger)` applies the middleware to every incoming request.
- Always call `next()` unless you want to end the request early.
- If you forget `next()`, the request will hang.

---

## 💡 Key Interview Points

- What is middleware in Express?
- Why do we need to call `next()`?
- Difference between `app.use()` and route-specific middleware
- Order of middleware matters
- Common built-in middleware (`express.json()`, `express.static()`)

---

## 🔥 Bonus Challenges

1. Add the response status code to the logger (hint: use `res.on('finish')`)
2. Create a middleware that only logs POST requests
3. Create a middleware that blocks requests to `/admin` and sends a 403 response
```
