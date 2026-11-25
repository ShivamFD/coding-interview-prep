# 📁 Node.js + Express: Serving Static Files

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Static%20Files-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

In most web applications we need to serve static files such as:

- HTML
- CSS
- JavaScript
- Images
- Fonts

Express provides a built-in middleware for this:

```js
express.static()
```

---

## 🎯 Problem Statement

Create an Express server that:

1. Serves static files from a folder named `public`
2. Has a simple API route as well
3. Shows how to create the folder structure

---

## 💻 Solution

### 1. Folder Structure

```
my-app/
├── public/
│   ├── index.html
│   ├── style.css
│   └── script.js
├── app.js
└── package.json
```

---

### 2. public/index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Static Files Demo</title>
  <link rel="stylesheet" href="/style.css">
</head>
<body>
  <h1>Welcome to Express Static Files</h1>
  <p>This page is served by Express.</p>
  <button onclick="showMessage()">Click Me</button>

  <script src="/script.js"></script>
</body>
</html>
```

---

### 3. public/style.css

```css
body {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: 50px auto;
  text-align: center;
  background-color: #f5f5f5;
}

h1 {
  color: #333;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}
```

---

### 4. public/script.js

```js
function showMessage() {
  alert("Static JavaScript file is working!");
}
```

---

### 5. app.js

```js
const express = require("express");
const path = require("path");

const app = express();

// Serve static files from the "public" folder
app.use(express.static(path.join(__dirname, "public")));

// Simple API route
app.get("/api/message", (req, res) => {
  res.json({
    success: true,
    message: "This is an API response"
  });
});

// Optional: fallback for SPA (if needed later)
// app.get("*", (req, res) => {
//   res.sendFile(path.join(__dirname, "public", "index.html"));
// });

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

---

## 🧠 Explanation

- `express.static()` is a built-in middleware.
- When a request comes for `/style.css`, Express looks for the file inside the `public` folder.
- We use `path.join(__dirname, "public")` so the path works correctly on every operating system.
- API routes and static files can work together in the same server.
- Order matters: put `express.static()` before your custom routes if you want static files to take priority.

---

## 💡 Key Interview Points

- What does `express.static()` do?
- Why do we use `path.join(__dirname, ...)`?
- Can we serve multiple static folders?
- Difference between static files and API routes
- How would you serve files from multiple directories?

---

## 🔥 Bonus Challenges

1. Serve static files from two different folders (`public` and `assets`)
2. Add a custom prefix: `/static` for all static files
3. Create a simple 404 page for unknown routes
```
