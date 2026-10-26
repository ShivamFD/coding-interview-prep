# 🔐 Node.js + Express: JWT Authentication

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-JWT%20Authentication-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

**JWT (JSON Web Token)** is the most popular way to implement stateless authentication in modern web applications.

### How JWT works:
1. User logs in with email/password
2. Server verifies credentials and creates a signed token
3. Client stores the token (usually in localStorage or cookies)
4. Client sends the token in the `Authorization` header on every request
5. Server verifies the token and allows access

### Structure of a JWT:
```
header.payload.signature
```

---

## 🎯 Problem Statement

Implement a basic JWT authentication system with:

1. User registration (simple in-memory)
2. User login → returns JWT
3. Protected route that only works with a valid token
4. Middleware to verify the token

---

## 💻 Solution

### 1. Install required packages

```bash
npm install express jsonwebtoken bcryptjs dotenv
```

---

### 2. Complete Code

```js
require("dotenv").config();
const express = require("express");
const jwt = require("jsonwebtoken");
const bcrypt = require("bcryptjs");

const app = express();
app.use(express.json());

// ============================================
// In-memory user store (for demo only)
// ============================================
const users = [];

// ============================================
// Middleware: Verify JWT
// ============================================
const authenticateToken = (req, res, next) => {
  const authHeader = req.headers["authorization"];
  const token = authHeader && authHeader.split(" ")[1]; // Bearer TOKEN

  if (!token) {
    return res.status(401).json({ message: "Access denied. No token provided." });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET || "mysecretkey");
    req.user = decoded; // attach user payload to request
    next();
  } catch (error) {
    return res.status(403).json({ message: "Invalid or expired token." });
  }
};

// ============================================
// Routes
// ============================================

// Register
app.post("/register", async (req, res) => {
  const { username, password } = req.body;

  if (!username || !password) {
    return res.status(400).json({ message: "Username and password are required" });
  }

  const existingUser = users.find((u) => u.username === username);
  if (existingUser) {
    return res.status(400).json({ message: "Username already exists" });
  }

  const hashedPassword = await bcrypt.hash(password, 10);

  const user = {
    id: users.length + 1,
    username,
    password: hashedPassword
  };

  users.push(user);

  res.status(201).json({ message: "User registered successfully" });
});

// Login
app.post("/login", async (req, res) => {
  const { username, password } = req.body;

  const user = users.find((u) => u.username === username);
  if (!user) {
    return res.status(400).json({ message: "Invalid credentials" });
  }

  const isPasswordValid = await bcrypt.compare(password, user.password);
  if (!isPasswordValid) {
    return res.status(400).json({ message: "Invalid credentials" });
  }

  // Create JWT
  const token = jwt.sign(
    { id: user.id, username: user.username },
    process.env.JWT_SECRET || "mysecretkey",
    { expiresIn: "1h" }
  );

  res.json({
    message: "Login successful",
    token
  });
});

// Protected Route
app.get("/profile", authenticateToken, (req, res) => {
  res.json({
    message: "This is a protected route",
    user: req.user
  });
});

// Public Route
app.get("/", (req, res) => {
  res.json({ message: "Welcome to the API" });
});

// ============================================
// Start Server
// ============================================
const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

---

### 3. Example `.env`

```env
PORT=5000
JWT_SECRET=supersecretkey123
```

---

## 🧠 Explanation

1. **Registration**: We hash the password using `bcrypt` before storing it.
2. **Login**: We compare the password and generate a JWT using `jwt.sign()`.
3. **authenticateToken middleware**:
   - Extracts the token from the `Authorization` header
   - Verifies it using `jwt.verify()`
   - Attaches the decoded payload to `req.user`
4. Protected routes simply use the middleware.

---

## 💡 Key Interview Points

- What is the structure of a JWT?
- Difference between JWT and Session-based authentication
- Where should we store the JWT on the client? (httpOnly cookies vs localStorage)
- What is the purpose of the secret key?
- How do you handle token expiration?
- What is refresh token pattern?
- Why do we hash passwords?

---

## 🔥 Bonus Challenges

1. Add role-based access control (admin vs user)
2. Implement a refresh token system
3. Add token blacklisting on logout
4. Move users to MongoDB instead of in-memory array
```
