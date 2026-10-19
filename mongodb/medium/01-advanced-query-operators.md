# 🔍 MongoDB: Advanced Query Operators

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Query%20Operators-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

In MongoDB, we use **query operators** to perform advanced filtering.

These operators allow us to write powerful conditions similar to SQL’s `WHERE` clause.

### Most Important Query Operators

| Operator        | Description                              |
|-----------------|------------------------------------------|
| `$eq`           | Equal to                                 |
| `$ne`           | Not equal to                             |
| `$gt`           | Greater than                             |
| `$gte`          | Greater than or equal to                 |
| `$lt`           | Less than                                |
| `$lte`           | Less than or equal to                    |
| `$in`           | Matches any value in an array            |
| `$nin`          | Matches none of the values in an array   |
| `$and`          | Logical AND                              |
| `$or`           | Logical OR                               |
| `$regex`        | Pattern matching (like LIKE in SQL)      |
| `$exists`       | Checks if a field exists                 |

---

## 🗄️ Sample Collection: `products`

```json
[
  { "_id": 1, "name": "Laptop", "price": 55000, "category": "Electronics", "inStock": true, "rating": 4.5 },
  { "_id": 2, "name": "Mobile", "price": 25000, "category": "Electronics", "inStock": true, "rating": 4.2 },
  { "_id": 3, "name": "Headphones", "price": 1999, "category": "Electronics", "inStock": false, "rating": 3.8 },
  { "_id": 4, "name": "Shirt", "price": 999, "category": "Fashion", "inStock": true, "rating": 4.0 },
  { "_id": 5, "name": "Jeans", "price": 1499, "category": "Fashion", "inStock": true, "rating": 3.5 },
  { "_id": 6, "name": "Book", "price": 499, "category": "Stationery", "inStock": true, "rating": 4.7 },
  { "_id": 7, "name": "Watch", "price": 8000, "category": "Electronics", "inStock": false, "rating": 4.1 },
  { "_id": 8, "name": "Shoes", "price": 2999, "category": "Fashion", "inStock": true, "rating": 4.3 }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Find products with price greater than 5000

```js
db.products.find({ price: { $gt: 5000 } })
```

---

### 🔹 Task 2: Find products with price between 1000 and 10000

```js
db.products.find({ 
  price: { $gte: 1000, $lte: 10000 } 
})
```

---

### 🔹 Task 3: Find products that are in stock and belong to Electronics

```js
db.products.find({
  $and: [
    { inStock: true },
    { category: "Electronics" }
  ]
})
```

---

### 🔹 Task 4: Find products that are either Fashion or have rating >= 4.5

```js
db.products.find({
  $or: [
    { category: "Fashion" },
    { rating: { $gte: 4.5 } }
  ]
})
```

---

### 🔹 Task 5: Find products whose name starts with “S”

```js
db.products.find({
  name: { $regex: /^S/i }
})
```

---

### 🔹 Task 6: Find products that are NOT in the Electronics category

```js
db.products.find({
  category: { $ne: "Electronics" }
})
```

---

### 🔹 Task 7: Find products whose category is either Electronics or Fashion

```js
db.products.find({
  category: { $in: ["Electronics", "Fashion"] }
})
```

---

### 🔹 Task 8: Find products that have the field `rating`

```js
db.products.find({
  rating: { $exists: true }
})
```

---

## 💡 Key Takeaways

- `$gt`, `$gte`, `$lt`, `$lte` are used for range queries.
- `$and` and `$or` are used for combining multiple conditions.
- `$in` is very useful when matching against a list of values.
- `$regex` is used for pattern matching (case-insensitive with `i` flag).
- `$exists` checks whether a field is present in the document.
- These operators are extremely important for writing real-world MongoDB queries.

---

## 🔥 Bonus Challenge

Write a query that finds products which:
- Are in stock
- Belong to either Electronics or Fashion
- Have price less than 10000
- And rating greater than or equal to 4.0
```
