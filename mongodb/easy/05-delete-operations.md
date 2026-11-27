# 🗑️ MongoDB: Delete Operations

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Delete%20Operations-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

MongoDB provides two main methods to delete documents:

| Method          | Description                                      |
|-----------------|--------------------------------------------------|
| `deleteOne()`   | Deletes the **first** document that matches the filter |
| `deleteMany()`  | Deletes **all** documents that match the filter  |

Both methods return a result object that tells you how many documents were deleted.

---

## 🗄️ Sample Collection: `users`

```json
[
  { "_id": 1, "name": "Rahul", "age": 28, "city": "Delhi", "active": true },
  { "_id": 2, "name": "Priya", "age": 25, "city": "Mumbai", "active": true },
  { "_id": 3, "name": "Amit", "age": 32, "city": "Delhi", "active": false },
  { "_id": 4, "name": "Sneha", "age": 27, "city": "Bangalore", "active": true },
  { "_id": 5, "name": "Vikram", "age": 35, "city": "Delhi", "active": false },
  { "_id": 6, "name": "Neha", "age": 24, "city": "Mumbai", "active": true }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Delete a single user by name

```js
db.users.deleteOne({ name: "Amit" })
```

---

### 🔹 Task 2: Delete the first inactive user

```js
db.users.deleteOne({ active: false })
```

---

### 🔹 Task 3: Delete all inactive users

```js
db.users.deleteMany({ active: false })
```

---

### 🔹 Task 4: Delete all users from Delhi

```js
db.users.deleteMany({ city: "Delhi" })
```

---

### 🔹 Task 5: Delete users older than 30

```js
db.users.deleteMany({ age: { $gt: 30 } })
```

---

### 🔹 Task 6: Delete all documents in the collection

```js
db.users.deleteMany({})
```

> Note: This removes every document but keeps the collection.

---

### 🔹 Task 7: Check how many documents were deleted

```js
const result = db.users.deleteMany({ city: "Mumbai" });
print(result.deletedCount);   // shows number of deleted documents
```

---

## 💡 Key Takeaways

- `deleteOne()` → stops after deleting the first match.
- `deleteMany()` → deletes every document that matches the filter.
- Always be careful with `deleteMany({})` — it deletes everything.
- The result object contains `deletedCount`.
- It is good practice to first run a `find()` with the same filter to see what will be deleted.

---

## 🔥 Bonus Challenge

Write queries for the following:

1. Delete all users who are inactive **and** older than 30
2. Delete only one user from Mumbai
3. Delete all users whose name starts with "N" (hint: use regex)
```
