# 📊 MongoDB: Sorting, Limiting & Projection

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Sort%20Limit%20Projection-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

These are three of the most commonly used features when reading data from MongoDB:

| Feature       | Method           | Purpose                              |
|---------------|------------------|--------------------------------------|
| Sorting       | `.sort()`        | Order the results                    |
| Limiting      | `.limit()`       | Restrict number of documents         |
| Skipping      | `.skip()`        | Skip documents (useful for pagination) |
| Projection    | Second argument of `find()` | Select which fields to return |

---

## 🗄️ Sample Collection: `products`

```json
[
  { "_id": 1, "name": "Laptop", "price": 55000, "category": "Electronics", "rating": 4.5 },
  { "_id": 2, "name": "Mobile", "price": 25000, "category": "Electronics", "rating": 4.2 },
  { "_id": 3, "name": "Headphones", "price": 2000, "category": "Electronics", "rating": 3.8 },
  { "_id": 4, "name": "Shirt", "price": 1200, "category": "Fashion", "rating": 4.0 },
  { "_id": 5, "name": "Jeans", "price": 2500, "category": "Fashion", "rating": 4.3 },
  { "_id": 6, "name": "Book", "price": 500, "category": "Education", "rating": 4.7 },
  { "_id": 7, "name": "Watch", "price": 8000, "category": "Electronics", "rating": 4.1 },
  { "_id": 8, "name": "Shoes", "price": 3500, "category": "Fashion", "rating": 4.4 }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Sort products by price (ascending)

```js
db.products.find().sort({ price: 1 })
```

---

### 🔹 Task 2: Sort products by price (descending)

```js
db.products.find().sort({ price: -1 })
```

---

### 🔹 Task 3: Get the 3 most expensive products

```js
db.products.find().sort({ price: -1 }).limit(3)
```

---

### 🔹 Task 4: Skip the first 2 products and get the next 3

```js
db.products.find().sort({ price: 1 }).skip(2).limit(3)
```

---

### 🔹 Task 5: Get only name and price of all products (Projection)

```js
db.products.find({}, { name: 1, price: 1, _id: 0 })
```

---

### 🔹 Task 6: Get the highest rated product (name and rating only)

```js
db.products.find({}, { name: 1, rating: 1, _id: 0 }).sort({ rating: -1 }).limit(1)
```

---

### 🔹 Task 7: Pagination Example (Page 2, page size = 3)

```js
const page = 2;
const pageSize = 3;

db.products.find()
  .sort({ price: 1 })
  .skip((page - 1) * pageSize)
  .limit(pageSize)
```

---

## 💡 Key Takeaways

- `1` means ascending, `-1` means descending in `.sort()`.
- `.limit()` is very useful for Top-N queries.
- `.skip()` + `.limit()` is the classic way to do pagination.
- Projection helps reduce the amount of data transferred.
- `_id` is included by default — set `_id: 0` to exclude it.
- Chaining order: `find → sort → skip → limit`

---

## 🔥 Bonus Challenge

Write a query that returns the top 3 highest rated Electronics products, showing only `name`, `price`, and `rating`.
```
