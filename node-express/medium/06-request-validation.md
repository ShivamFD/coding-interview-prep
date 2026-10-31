# ✅ Node.js + Express: Request Validation

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Request%20Validation-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Validating incoming request data is critical for security and data integrity.

In this problem we will implement **manual validation** (without external libraries like Joi or Zod) so you understand the core logic.

Common things we validate:
- Required fields
- Data types
- String length
- Email format
- Number ranges

---

## 🎯 Problem Statement

Create a registration API that validates the following fields:

- `name` → required, minimum 3 characters
- `email` → required, valid email format
- `password` → required, minimum 6 characters
- `age` → optional, must be a number between 18 and 100

Return proper error messages when validation fails.

---

## 💻 Solution

```js
const express = require("express");
const app = express();

app.use(express.json());

// ============================================
// Validation Helper Function
// ============================================
function validateRegister(data) {
  const errors = [];

  // Name validation
  if (!data.name || data.name.trim() === "") {
    errors.push("Name is required");
  } else if (data.name.trim().length < 3) {
    errors.push("Name must be at least 3 characters long");
  }

  // Email validation
  if (!data.email || data.email.trim() === "") {
    errors.push("Email is required");
  } else {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(data.email)) {
      errors.push("Please provide a valid email address");
    }
  }

  // Password validation
  if (!data.password) {
    errors.push("Password is required");
  } else if (data.password.length < 6) {
    errors.push("Password must be at least 6 characters long");
  }

  // Age validation (optional)
  if (data.age !== undefined) {
    const age = Number(data.age);
    if (isNaN(age)) {
      errors.push("Age must be a number");
    } else if (age < 18 || age > 100) {
      errors.push("Age must be between 18 and 100");
    }
  }

  return errors;
}

// ============================================
// Routes
// ============================================
app.post("/register", (req, res) => {
  const errors = validateRegister(req.body);

  if (errors.length > 0) {
    return res.status(400).json({
      success: false,
      errors: errors
    });
  }

  // If validation passes
  res.status(201).json({
    success: true,
    message: "User registered successfully",
    data: {
      name: req.body.name,
      email: req.body.email,
      age: req.body.age || null
    }
  });
});

app.get("/", (req, res) => {
  res.json({ message: "Validation API is running" });
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

---

## 🧠 Explanation

- We created a separate `validateRegister` function for clean code.
- It returns an array of error messages.
- If the array has any items → we send a `400 Bad Request`.
- This pattern is easy to extend for other routes (login, update profile, etc.).

---

## 💡 Key Interview Points

- Why is validation important?
- Difference between client-side and server-side validation
- Why should we never trust client-side validation alone?
- Common validation libraries (Joi, Zod, express-validator)
- How would you structure validation in a large project?

---

## 🔥 Bonus Challenges

1. Create a validation function for a "Create Product" route (name, price, category)
2. Move validation into a middleware
3. Add password strength validation (must contain number + special character)
```
