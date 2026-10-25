# ⚙️ Node.js + Express: dotenv & CORS

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-dotenv%20%26%20CORS-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

### 1. dotenv
`dotenv` is used to load environment variables from a `.env` file into `process.env`.  
This is the standard way to manage configuration (ports, database URLs, secrets, etc.).

### 2. CORS
CORS (Cross-Origin Resource Sharing) is a security mechanism that allows (or restricts) requests from different origins (domains).

In real applications, your frontend (React) and backend (Express) usually run on different ports/domains, so CORS must be configured.

---

## 🎯 Problem Statement

Create an Express server that:

1. Loads environment variables using `dotenv`
2. Uses a proper `PORT` from environment variables
3. Enables CORS so that a frontend running on a different origin can access the API
4. Has a simple test route

---

## 💻 Solution

### 1. Install required packages

```bash
npm install express dotenv cors
```

---

### 2. Create `.env` file

```env
PORT=5000
NODE_ENV=development
CLIENT_URL=http://localhost:3000
```

---

### 3. Complete Server Code (`app.js` or `index.js`)

```js
require("dotenv").config(); // Load environment variables

const express = require("express");
const cors = require("cors");

const app = express();

// ============================================
// Middleware
// ============================================

// Enable CORS
app.use(cors({
  origin: process.env.CLIENT_URL || "http://localhost:3000",
  credentials: true
}));

// Parse JSON bodies
app.use(express.json());

// ============================================
// Routes
// ============================================

app.get("/", (req, res) => {
  res.json({
    message: "API is running",
    environment: process.env.NODE_ENV,
    port: process.env.PORT
  });
});

app.get("/api/hello", (req, res) => {
  res.json({
    message: "Hello from the backend!",
    success: true
  });
});

// ============================================
// Start Server
// ============================================

const PORT = process.env.PORT || 5000;

app.listen(PORT, () => {
  console.log(`Server running in ${process.env.NODE_ENV} mode on port ${PORT}`);
});
```

---

## 🧠 Explanation

### dotenv
- `require("dotenv").config()` loads all variables from `.env` into `process.env`.
- Never commit the `.env` file to Git (add it to `.gitignore`).
- Always provide fallback values: `process.env.PORT || 5000`.

### CORS
- By default, browsers block requests from different origins.
- `cors` middleware tells the browser that it is allowed to access the API.
- We can restrict it to specific origins for better security.

---

## 💡 Key Interview Points

- Why do we use environment variables?
- What should never be stored in `.env` and committed to Git? (secrets, API keys)
- What is CORS and why do we need it?
- Difference between simple CORS and CORS with credentials
- How to allow multiple frontend origins
- Best practices for managing configuration in Node.js applications

---

## 🔥 Bonus Challenges

1. Allow multiple frontend origins (array of URLs)
2. Create different `.env.development` and `.env.production` files
3. Add a simple API key authentication middleware using an environment variable
4. Write a proper `.gitignore` that excludes `.env`
```
