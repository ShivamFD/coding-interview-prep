# 📁 Node.js + Express: File Upload with Multer

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-File%20Upload%20%28Multer%29-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Handling file uploads is a very common requirement in real-world applications (profile pictures, documents, images, etc.).

In Node.js + Express, the most popular middleware for handling `multipart/form-data` is **Multer**.

---

## 🎯 Problem Statement

Create an Express server that:

1. Accepts a single image upload
2. Accepts multiple image uploads
3. Stores files in an `uploads/` folder
4. Validates file type (only images)
5. Limits file size

---

## 💻 Solution

### 1. Install Multer

```bash
npm install multer
```

### 2. Complete Code

```js
const express = require("express");
const multer = require("multer");
const path = require("path");
const fs = require("fs");

const app = express();

// Create uploads folder if it doesn't exist
const uploadDir = path.join(__dirname, "uploads");
if (!fs.existsSync(uploadDir)) {
  fs.mkdirSync(uploadDir);
}

// ============================================
// Multer Storage Configuration
// ============================================
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, "uploads/");
  },
  filename: function (req, file, cb) {
    // Unique filename: timestamp + original name
    const uniqueSuffix = Date.now() + "-" + Math.round(Math.random() * 1e9);
    cb(null, uniqueSuffix + path.extname(file.originalname));
  }
});

// ============================================
// File Filter (Only images allowed)
// ============================================
const fileFilter = (req, file, cb) => {
  const allowedTypes = /jpeg|jpg|png|gif|webp/;
  const extname = allowedTypes.test(path.extname(file.originalname).toLowerCase());
  const mimetype = allowedTypes.test(file.mimetype);

  if (extname && mimetype) {
    cb(null, true);
  } else {
    cb(new Error("Only image files are allowed!"));
  }
};

// ============================================
// Multer Upload Instance
// ============================================
const upload = multer({
  storage: storage,
  limits: {
    fileSize: 5 * 1024 * 1024 // 5MB limit
  },
  fileFilter: fileFilter
});

// ============================================
// Routes
// ============================================

// Single file upload
app.post("/upload/single", upload.single("image"), (req, res) => {
  if (!req.file) {
    return res.status(400).json({ message: "Please upload a file" });
  }

  res.json({
    message: "File uploaded successfully",
    file: {
      filename: req.file.filename,
      path: req.file.path,
      size: req.file.size
    }
  });
});

// Multiple files upload (max 5)
app.post("/upload/multiple", upload.array("images", 5), (req, res) => {
  if (!req.files || req.files.length === 0) {
    return res.status(400).json({ message: "Please upload files" });
  }

  const filesInfo = req.files.map((file) => ({
    filename: file.filename,
    path: file.path,
    size: file.size
  }));

  res.json({
    message: "Files uploaded successfully",
    files: filesInfo
  });
});

// Error handling middleware for Multer
app.use((err, req, res, next) => {
  if (err instanceof multer.MulterError) {
    if (err.code === "LIMIT_FILE_SIZE") {
      return res.status(400).json({ message: "File too large. Max size is 5MB." });
    }
    return res.status(400).json({ message: err.message });
  } else if (err) {
    return res.status(400).json({ message: err.message });
  }
  next();
});

app.get("/", (req, res) => {
  res.json({ message: "File Upload API is running" });
});

const PORT = 5000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

---

## 🧠 Explanation

1. **diskStorage** → tells Multer where and how to store files.
2. **fileFilter** → validates the file type before saving.
3. **limits.fileSize** → prevents users from uploading very large files.
4. `upload.single("image")` → accepts one file with field name `"image"`.
5. `upload.array("images", 5)` → accepts up to 5 files with field name `"images"`.
6. We also added a custom error handler for Multer-specific errors.

---

## 💡 Key Interview Points

- Difference between `diskStorage` and `memoryStorage`
- How to validate file types securely
- How to handle file size limits
- What is `multipart/form-data`?
- How would you store files on cloud (AWS S3, Cloudinary)?
- Security risks with file uploads (and how to prevent them)

---

## 🔥 Bonus Challenges

1. Add support for uploading PDFs as well
2. Resize images before saving (using sharp library)
3. Upload files directly to Cloudinary or AWS S3
4. Create a route to delete an uploaded file
```
