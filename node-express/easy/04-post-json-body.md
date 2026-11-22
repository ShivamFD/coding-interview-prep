# 📨 Node.js + Express: Handling POST Requests & JSON Body

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-POST%20%26%20JSON%20Body-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

In real applications we mostly send data to the server using **POST** requests.

The data is usually sent in **JSON** format in the request body.

To read JSON data in Express, we need the built-in middleware:

```js
app.use(express.json());
```

---

## 🎯 Problem Statement

Create an Express server that:

1. Accepts a POST request at `/users`
2. Reads `name` and `email` from the request body
3. Validates that both fields are present
4. Returns the created user with a `201` status code
5. Has a GET route to show all users (in-memory)

---

## 💻 Solution

```js
const express = require("express");
const app = express();

// Important: This middleware parses incoming JSON
app.use(express.json());

// In-memory storage
let users = [];
let nextId = 1;

// ============================================
// CREATE User (POST)
// ============================================
app.post("/users", (req, res) => {
  const { name, email } = req.body;

  // Simple validation
  if (!name || !email) {
    return res.status(400).json({
      success: false,
      message: "Name and email are required"
    });
  }

  const newUser = {
    id: nextId++,
    name,
    email
  };

  users.push(newUser);

  res.status(201).json({
    success: true,
    message: "User created successfully",
    data: newUser
  });
});

// ============================================
// GET All Users
// ============================================
app.get("/users", (req, res) => {
  res.json({
    success: true,
    count: users.length,
    data: users
  });
});

// ============================================
// Home Route
// ============================================
app.get("/", (req, res) => {
  res.json({
    message: "User API is running",
    endpoints: {
      "POST /users": "Create a new user",
      "GET /users": "Get all users"
    }
  });
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

---

### How to Test

You can test with **Postman**, **Thunder Client**, or `curl`:

```bash
curl -X POST http://localhost:3000/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Rahul","email":"rahul@example.com"}'
```

---

## 🧠 Explanation

- `express.json()` is a built-in middleware that parses the JSON body and makes it available on `req.body`.
- Without this middleware, `req.body` will be `undefined`.
- We return status `201` for successful creation (standard practice).
- We return status `400` when required fields are missing.
- Data is stored in an in-memory array (for learning purposes only).

---

## 💡 Key Interview Points

- What does `express.json()` do?
- Difference between `req.body`, `req.params`, and `req.query`
- Why do we need `Content-Type: application/json` header?
- What status code should we use when creating a resource? (`201`)
- What status code for validation errors? (`400`)

---

## 🔥 Bonus Challenges

1. Add a GET `/users/:id` route to find a single user
2. Add simple email format validation
3. Prevent creating a user with a duplicate email
```
