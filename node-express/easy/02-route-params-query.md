# 🛣️ Node.js + Express: Route Parameters & Query Strings

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Route%20Params%20%26%20Query-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

In Express, we can receive data from the client in multiple ways:

| Type              | Example URL                          | Access in Express          |
|-------------------|--------------------------------------|----------------------------|
| Route Parameters  | `/users/15`                          | `req.params.id`            |
| Query Strings     | `/search?keyword=react&page=2`       | `req.query.keyword`        |

Understanding the difference is very important.

---

## 🎯 Problem Statement

Create an Express server that demonstrates:

1. Route parameters
2. Multiple route parameters
3. Query strings
4. Combining both

---

## 💻 Solution

```js
const express = require("express");
const app = express();

app.use(express.json());

// ============================================
// 1. Single Route Parameter
// ============================================
app.get("/users/:id", (req, res) => {
  const userId = req.params.id;

  res.json({
    message: "Single route parameter",
    userId: userId
  });
});

// ============================================
// 2. Multiple Route Parameters
// ============================================
app.get("/users/:userId/posts/:postId", (req, res) => {
  const { userId, postId } = req.params;

  res.json({
    message: "Multiple route parameters",
    userId,
    postId
  });
});

// ============================================
// 3. Query Strings
// ============================================
// Example: /search?keyword=react&limit=10
app.get("/search", (req, res) => {
  const { keyword, limit = 5 } = req.query;

  res.json({
    message: "Query strings example",
    keyword: keyword || "No keyword provided",
    limit: Number(limit)
  });
});

// ============================================
// 4. Route Params + Query Strings together
// ============================================
// Example: /products/electronics?sort=price&order=asc
app.get("/products/:category", (req, res) => {
  const { category } = req.params;
  const { sort = "name", order = "asc" } = req.query;

  res.json({
    message: "Route param + query example",
    category,
    sort,
    order
  });
});

// ============================================
// Home route
// ============================================
app.get("/", (req, res) => {
  res.json({
    message: "Try these routes:",
    routes: [
      "GET /users/15",
      "GET /users/10/posts/99",
      "GET /search?keyword=react&limit=10",
      "GET /products/electronics?sort=price&order=desc"
    ]
  });
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

---

## 🧠 Explanation

- **Route Parameters** (`:id`) are part of the URL path and are accessed via `req.params`.
- **Query Strings** come after `?` and are accessed via `req.query`.
- Query parameters are always received as strings.
- You can set default values while destructuring: `const { limit = 5 } = req.query`.

---

## 💡 Key Interview Points

- Difference between `req.params` and `req.query`
- Difference between `req.params`, `req.query`, and `req.body`
- Are query parameters always strings? (Yes)
- How do you handle optional query parameters?
- Common use cases for route params vs query strings

---

## 🔥 Bonus Challenges

1. Create a route `/books/:id` that returns a book object (hardcoded data)
2. Create a route that filters a list of users based on query parameters (`role`, `age`)
3. Add validation: if `id` is not a number, return 400 error
```
