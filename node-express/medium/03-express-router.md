# 🗂️ Node.js + Express: Express Router (Modular Routes)

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Express%20Router-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

As your application grows, putting all routes in `app.js` becomes messy.  
**Express Router** allows you to create modular, mountable route handlers.

### Benefits of using Router:
- Better code organization
- Separation of concerns
- Easier to maintain and scale
- Cleaner main file

---

## 🎯 Problem Statement

Create a simple Express application with modular routes using `express.Router()`.

**Structure:**

```
project/
├── app.js
├── routes/
│   ├── users.js
│   └── products.js
```

**Requirements:**
1. Create a `users` router with basic CRUD routes
2. Create a `products` router with basic routes
3. Mount both routers in the main `app.js`

---

## 💻 Solution

### 1. routes/users.js

```js
const express = require("express");
const router = express.Router();

// Sample data
let users = [
  { id: 1, name: "Rahul", email: "rahul@example.com" },
  { id: 2, name: "Priya", email: "priya@example.com" }
];

// GET all users
router.get("/", (req, res) => {
  res.json(users);
});

// GET user by ID
router.get("/:id", (req, res) => {
  const user = users.find((u) => u.id === parseInt(req.params.id));
  if (!user) {
    return res.status(404).json({ message: "User not found" });
  }
  res.json(user);
});

// CREATE a new user
router.post("/", (req, res) => {
  const { name, email } = req.body;

  if (!name || !email) {
    return res.status(400).json({ message: "Name and email are required" });
  }

  const newUser = {
    id: users.length + 1,
    name,
    email
  };

  users.push(newUser);
  res.status(201).json(newUser);
});

// UPDATE a user
router.put("/:id", (req, res) => {
  const user = users.find((u) => u.id === parseInt(req.params.id));
  if (!user) {
    return res.status(404).json({ message: "User not found" });
  }

  user.name = req.body.name || user.name;
  user.email = req.body.email || user.email;

  res.json(user);
});

// DELETE a user
router.delete("/:id", (req, res) => {
  const index = users.findIndex((u) => u.id === parseInt(req.params.id));
  if (index === -1) {
    return res.status(404).json({ message: "User not found" });
  }

  users.splice(index, 1);
  res.json({ message: "User deleted successfully" });
});

module.exports = router;
```

---

### 2. routes/products.js

```js
const express = require("express");
const router = express.Router();

let products = [
  { id: 1, name: "Laptop", price: 55000 },
  { id: 2, name: "Mobile", price: 25000 }
];

// GET all products
router.get("/", (req, res) => {
  res.json(products);
});

// GET product by ID
router.get("/:id", (req, res) => {
  const product = products.find((p) => p.id === parseInt(req.params.id));
  if (!product) {
    return res.status(404).json({ message: "Product not found" });
  }
  res.json(product);
});

// CREATE a product
router.post("/", (req, res) => {
  const { name, price } = req.body;

  if (!name || !price) {
    return res.status(400).json({ message: "Name and price are required" });
  }

  const newProduct = {
    id: products.length + 1,
    name,
    price
  };

  products.push(newProduct);
  res.status(201).json(newProduct);
});

module.exports = router;
```

---

### 3. app.js (Main file)

```js
const express = require("express");
const app = express();

// Middleware
app.use(express.json());

// Import routers
const userRoutes = require("./routes/users");
const productRoutes = require("./routes/products");

// Mount routers
app.use("/api/users", userRoutes);
app.use("/api/products", productRoutes);

// Home route
app.get("/", (req, res) => {
  res.json({ message: "Welcome to the API" });
});

// Start server
const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

---

## 🧠 Explanation

- `express.Router()` creates a mini application that can handle routes.
- We define routes inside separate files.
- In `app.js`, we mount them using `app.use("/api/users", userRoutes)`.
- Now all user routes are available under `/api/users`.
- This structure is standard in professional Express applications.

---

## 💡 Key Interview Points

- Why do we use Express Router?
- Difference between `app.get` and `router.get`
- How does mounting work with `app.use()`?
- How to organize a large Express project?
- Best practices for folder structure in Express

---

## 🔥 Bonus Challenges

1. Add a middleware that logs every request to the users router only
2. Create a separate `routes/index.js` that exports all routers
3. Add proper error handling middleware
```
