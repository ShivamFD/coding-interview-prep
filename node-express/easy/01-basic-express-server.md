# 🚀 Node.js + Express: Basic Server with Routes

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Express%20Basics-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

In this problem, we will create a basic **Express.js** server with common HTTP routes.

This is the foundation of almost every Node.js backend interview question.

### What you will learn:
- How to create an Express server
- How to define GET and POST routes
- How to use route parameters (`:id`)
- How to read query parameters
- How to send JSON responses

---

## 🎯 Problem Statement

Create a simple Express server that supports the following routes:

1. `GET /` → Returns a welcome message
2. `GET /users` → Returns a list of users
3. `GET /users/:id` → Returns a single user by ID
4. `POST /users` → Accepts a new user and returns it
5. `GET /search` → Reads a query parameter `name` and returns a message

---

## 💻 Solution

```js
const express = require("express");
const app = express();

// Middleware to parse JSON body
app.use(express.json());

// Sample data
let users = [
  { id: 1, name: "Rahul", role: "Developer" },
  { id: 2, name: "Priya", role: "Designer" },
  { id: 3, name: "Amit", role: "Manager" }
];

// 1. Home route
app.get("/", (req, res) => {
  res.json({ message: "Welcome to the Express API" });
});

// 2. Get all users
app.get("/users", (req, res) => {
  res.json(users);
});

// 3. Get user by ID
app.get("/users/:id", (req, res) => {
  const id = parseInt(req.params.id);
  const user = users.find((u) => u.id === id);

  if (!user) {
    return res.status(404).json({ message: "User not found" });
  }

  res.json(user);
});

// 4. Create a new user
app.post("/users", (req, res) => {
  const { name, role } = req.body;

  if (!name || !role) {
    return res.status(400).json({ message: "Name and role are required" });
  }

  const newUser = {
    id: users.length + 1,
    name,
    role
  };

  users.push(newUser);
  res.status(201).json(newUser);
});

// 5. Search using query parameter
app.get("/search", (req, res) => {
  const { name } = req.query;

  if (!name) {
    return res.status(400).json({ message: "Please provide a name query parameter" });
  }

  res.json({ message: `You searched for: ${name}` });
});

// Start the server
const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

---

## 🧠 Explanation

- `express.json()` is middleware that allows us to read JSON data from the request body.
- `req.params` is used for route parameters (`/users/:id`).
- `req.query` is used for query strings (`/search?name=Rahul`).
- `req.body` contains the data sent in POST requests.
- We always send proper status codes (`200`, `201`, `400`, `404`).

---

## 💡 Key Interview Points

- Difference between `req.params` and `req.query`
- Why do we need `express.json()` middleware?
- How to send proper HTTP status codes
- How to handle missing data (validation)
- Difference between `res.send()` and `res.json()`

---

## 🔥 Bonus Challenges

1. Add a `PUT /users/:id` route to update a user
2. Add a `DELETE /users/:id` route to delete a user
3. Add basic validation (name should be at least 3 characters)
```
