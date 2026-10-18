# 🍃 MongoDB: Basic CRUD Operations

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-CRUD-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

CRUD stands for:

- **C**reate
- **R**ead
- **U**pdate
- **D**elete

These are the four basic operations you perform on any database.  
In MongoDB, we work with **documents** (similar to JSON objects) stored in **collections**.

---

## 🗄️ Sample Collection: `users`

```json
[
  { "_id": 1, "name": "Rahul", "age": 28, "city": "Delhi", "role": "Developer" },
  { "_id": 2, "name": "Priya", "age": 25, "city": "Mumbai", "role": "Designer" },
  { "_id": 3, "name": "Amit", "age": 32, "city": "Delhi", "role": "Manager" },
  { "_id": 4, "name": "Sneha", "age": 27, "city": "Bangalore", "role": "Developer" }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 1. Create (Insert)

**Insert one document:**

```js
db.users.insertOne({
  name: "Vikram",
  age: 30,
  city: "Pune",
  role: "Developer"
})
```

**Insert multiple documents:**

```js
db.users.insertMany([
  { name: "Neha", age: 26, city: "Mumbai", role: "HR" },
  { name: "Karan", age: 29, city: "Delhi", role: "Developer" }
])
```

---

### 🔹 2. Read (Find)

**Find all documents:**

```js
db.users.find()
```

**Find with condition:**

```js
db.users.find({ city: "Delhi" })
```

**Find one document:**

```js
db.users.findOne({ name: "Rahul" })
```

**Find with multiple conditions (AND):**

```js
db.users.find({ city: "Delhi", role: "Developer" })
```

**Find with projection (only specific fields):**

```js
db.users.find(
  { city: "Delhi" },
  { name: 1, age: 1, _id: 0 }
)
```

---

### 🔹 3. Update

**Update one document:**

```js
db.users.updateOne(
  { name: "Rahul" },
  { $set: { age: 29 } }
)
```

**Update multiple documents:**

```js
db.users.updateMany(
  { city: "Delhi" },
  { $set: { country: "India" } }
)
```

**Increment a value:**

```js
db.users.updateOne(
  { name: "Priya" },
  { $inc: { age: 1 } }
)
```

---

### 🔹 4. Delete

**Delete one document:**

```js
db.users.deleteOne({ name: "Sneha" })
```

**Delete multiple documents:**

```js
db.users.deleteMany({ role: "Developer" })
```

**Delete all documents in a collection:**

```js
db.users.deleteMany({})
```

---

## 💡 Key Takeaways

- `insertOne` / `insertMany` → Create
- `find` / `findOne` → Read
- `updateOne` / `updateMany` → Update
- `deleteOne` / `deleteMany` → Delete
- Always use `$set` when updating fields (otherwise it replaces the whole document)
- `_id` is automatically created by MongoDB if you don’t provide it
- MongoDB queries use JSON-like syntax

---

## 🔥 Bonus Challenge

Write queries for the following:

1. Find all users who are older than 27
2. Update the city of “Amit” to “Hyderabad”
3. Delete all users from “Mumbai”
```
