# 🔍 MongoDB: Text Search ($text)

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Text%20Search-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

MongoDB provides full-text search capabilities using **Text Indexes** and the `$text` operator.

This is useful when you want to search for words inside string fields (like product descriptions, blog posts, titles, etc.).

### Steps to use Text Search:
1. Create a **text index** on the field(s) you want to search
2. Use the `$text` operator in queries
3. Optionally sort by relevance using `{ $meta: "textScore" }`

---

## 🗄️ Sample Collection: `articles`

```json
[
  {
    "_id": 1,
    "title": "Introduction to JavaScript",
    "content": "JavaScript is a versatile programming language used for web development.",
    "tags": ["javascript", "web"]
  },
  {
    "_id": 2,
    "title": "Mastering React Hooks",
    "content": "React Hooks allow you to use state and lifecycle features in functional components.",
    "tags": ["react", "javascript"]
  },
  {
    "_id": 3,
    "title": "Node.js Performance Tips",
    "content": "Learn how to optimize your Node.js applications for better performance.",
    "tags": ["nodejs", "backend"]
  },
  {
    "_id": 4,
    "title": "Understanding MongoDB Indexes",
    "content": "Indexes are crucial for improving query performance in MongoDB.",
    "tags": ["mongodb", "database"]
  },
  {
    "_id": 5,
    "title": "Async JavaScript Patterns",
    "content": "Explore callbacks, promises, and async/await in JavaScript.",
    "tags": ["javascript", "async"]
  }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Create a text index

```js
db.articles.createIndex({ title: "text", content: "text" })
```

---

### 🔹 Task 2: Basic text search

```js
db.articles.find({ $text: { $search: "javascript" } })
```

---

### 🔹 Task 3: Search for multiple words (OR behavior)

```js
db.articles.find({ $text: { $search: "javascript react" } })
```

---

### 🔹 Task 4: Search for an exact phrase

```js
db.articles.find({ $text: { $search: "\"React Hooks\"" } })
```

---

### 🔹 Task 5: Exclude a word from search

```js
// Find documents that contain "javascript" but not "react"
db.articles.find({ $text: { $search: "javascript -react" } })
```

---

### 🔹 Task 6: Sort by relevance (textScore)

```js
db.articles.find(
  { $text: { $search: "javascript" } },
  { score: { $meta: "textScore" } }
).sort({ score: { $meta: "textScore" } })
```

---

### 🔹 Task 7: Projection with text score

```js
db.articles.find(
  { $text: { $search: "performance" } },
  {
    title: 1,
    score: { $meta: "textScore" },
    _id: 0
  }
).sort({ score: { $meta: "textScore" } })
```

---

## 💡 Key Takeaways

- You must create a **text index** before using `$text`.
- `$search: "word1 word2"` → matches documents containing any of the words.
- Use double quotes for exact phrase matching.
- Use `-word` to exclude a term.
- `{ $meta: "textScore" }` gives relevance score and can be used for sorting.
- Only one text index is allowed per collection (can cover multiple fields).

---

## 🔥 Bonus Challenge

1. Create a text index on `title`, `content`, and `tags`.
2. Write a query that searches for "javascript" and sorts the results by relevance.
3. Write a query that finds articles containing "Node.js" but not "performance".
```
