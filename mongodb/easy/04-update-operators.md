# ✏️ MongoDB: Update Operators ($set, $inc, $unset)

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Update%20Operators-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

When updating documents in MongoDB, we use **update operators** instead of replacing the whole document.

### Most Important Update Operators

| Operator   | Purpose                                      |
|------------|----------------------------------------------|
| `$set`     | Sets the value of a field                    |
| `$inc`     | Increments (or decrements) a numeric field   |
| `$unset`   | Removes a field from the document            |
| `$rename`  | Renames a field                              |
| `$mul`     | Multiplies the value of a field              |

---

## 🗄️ Sample Collection: `users`

```json
[
  { "_id": 1, "name": "Rahul", "age": 28, "city": "Delhi", "score": 80 },
  { "_id": 2, "name": "Priya", "age": 25, "city": "Mumbai", "score": 92 },
  { "_id": 3, "name": "Amit", "age": 32, "city": "Delhi", "score": 75 },
  { "_id": 4, "name": "Sneha", "age": 27, "city": "Bangalore", "score": 88 }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Update the city of Rahul using `$set`

```js
db.users.updateOne(
  { name: "Rahul" },
  { $set: { city: "Pune" } }
)
```

---

### 🔹 Task 2: Increase Amit’s score by 10 using `$inc`

```js
db.users.updateOne(
  { name: "Amit" },
  { $inc: { score: 10 } }
)
```

---

### 🔹 Task 3: Decrease Priya’s age by 1

```js
db.users.updateOne(
  { name: "Priya" },
  { $inc: { age: -1 } }
)
```

---

### 🔹 Task 4: Add a new field `isActive` to all users

```js
db.users.updateMany(
  {},
  { $set: { isActive: true } }
)
```

---

### 🔹 Task 5: Remove the `score` field from Sneha’s document

```js
db.users.updateOne(
  { name: "Sneha" },
  { $unset: { score: "" } }
)
```

---

### 🔹 Task 6: Update multiple fields at once

```js
db.users.updateOne(
  { name: "Rahul" },
  {
    $set: {
      city: "Hyderabad",
      role: "Developer"
    },
    $inc: {
      score: 5
    }
  }
)
```

---

### 🔹 Task 7: Rename a field

```js
db.users.updateMany(
  {},
  { $rename: { "city": "location" } }
)
```

---

## 💡 Key Takeaways

- Always use update operators (`$set`, `$inc`, etc.) instead of replacing the whole document.
- `$set` is the most commonly used operator.
- `$inc` can take positive or negative numbers.
- `$unset` removes a field completely.
- You can combine multiple operators in a single update.
- `updateOne` → updates only the first matching document.
- `updateMany` → updates all matching documents.

---

## 🔥 Bonus Challenge

Write update queries for the following:

1. Increase the score of all users from Delhi by 15
2. Add a field `lastUpdated` with the current date to all documents
3. Remove the `age` field from users who are older than 30
```
