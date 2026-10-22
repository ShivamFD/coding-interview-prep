# ⚡ MongoDB: Indexing Basics

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Indexing-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Indexes in MongoDB improve the speed of search queries.  
Without indexes, MongoDB must scan every document in a collection (collection scan), which is slow.

### Why Indexing is important?
- Makes queries significantly faster
- Essential for production applications
- Supports efficient sorting and filtering

### Types of Indexes (Common ones)
- Single Field Index
- Compound Index
- Text Index
- Unique Index

---

## 🗄️ Sample Collection: `users`

```json
[
  { "_id": 1, "name": "Rahul", "email": "rahul@example.com", "age": 28, "city": "Delhi" },
  { "_id": 2, "name": "Priya", "email": "priya@example.com", "age": 25, "city": "Mumbai" },
  { "_id": 3, "name": "Amit", "email": "amit@example.com", "age": 32, "city": "Delhi" },
  { "_id": 4, "name": "Sneha", "email": "sneha@example.com", "age": 27, "city": "Bangalore" },
  { "_id": 5, "name": "Vikram", "email": "vikram@example.com", "age": 30, "city": "Delhi" }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Create a single field index on `email`

```js
db.users.createIndex({ email: 1 })
```

> `1` means ascending order, `-1` means descending order.

---

### 🔹 Task 2: Create a unique index on `email`

```js
db.users.createIndex({ email: 1 }, { unique: true })
```

This prevents duplicate email addresses.

---

### 🔹 Task 3: Create a compound index on `city` and `age`

```js
db.users.createIndex({ city: 1, age: -1 })
```

Useful when you frequently query by city and sort/filter by age.

---

### 🔹 Task 4: View all indexes on a collection

```js
db.users.getIndexes()
```

---

### 🔹 Task 5: Check if a query is using an index (Explain)

```js
db.users.find({ email: "rahul@example.com" }).explain("executionStats")
```

Look for `"stage": "IXSCAN"` → means index is being used.  
If you see `"COLLSCAN"` → full collection scan (no index used).

---

### 🔹 Task 6: Drop an index

```js
db.users.dropIndex({ email: 1 })
```

Or drop by name:

```js
db.users.dropIndex("email_1")
```

---

### 🔹 Task 7: Create a text index (for text search)

```js
db.users.createIndex({ name: "text", email: "text" })
```

**Usage:**

```js
db.users.find({ $text: { $search: "Rahul" } })
```

---

## 💡 Key Takeaways

- Indexes make read queries faster but slightly slow down write operations.
- Create indexes on fields that are frequently used in `find()`, `sort()`, and `match` stages.
- Use **compound indexes** when filtering on multiple fields.
- Always use `.explain("executionStats")` to verify if an index is being used.
- Unique indexes are perfect for fields like email, username, etc.
- Don’t over-index — each index consumes memory and slows down writes.

---

## 🔥 Bonus Challenge

1. Create a compound index on `city` (ascending) and `name` (ascending).
2. Write a query that uses this index and verify it with `.explain()`.
3. Create a unique index on the `email` field and try inserting a duplicate email (observe the error).
```
