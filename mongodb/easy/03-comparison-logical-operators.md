

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Comparison%20%26%20Logical%20Operators-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

MongoDB provides powerful operators for filtering documents.

### Comparison Operators

| Operator | Meaning                    |
|----------|----------------------------|
| `$eq`    | Equal to                   |
| `$ne`    | Not equal to               |
| `$gt`    | Greater than               |
| `$gte`   | Greater than or equal to   |
| `$lt`    | Less than                  |
| `$lte`    | Less than or equal to      |
| `$in`    | Matches any value in array |
| `$nin`   | Matches none of the values |

### Logical Operators

| Operator | Meaning                          |
|----------|----------------------------------|
| `$and`   | All conditions must be true      |
| `$or`    | At least one condition is true   |
| `$not`   | Inverts the effect of a query    |
| `$nor`   | None of the conditions are true  |

---

## 🗄️ Sample Collection: `products`

```json
[
  { "_id": 1, "name": "Laptop", "price": 55000, "category": "Electronics", "rating": 4.5, "inStock": true },
  { "_id": 2, "name": "Mobile", "price": 25000, "category": "Electronics", "rating": 4.2, "inStock": true },
  { "_id": 3, "name": "Headphones", "price": 2000, "category": "Electronics", "rating": 3.8, "inStock": false },
  { "_id": 4, "name": "Shirt", "price": 1200, "category": "Fashion", "rating": 4.0, "inStock": true },
  { "_id": 5, "name": "Jeans", "price": 2500, "category": "Fashion", "rating": 4.3, "inStock": true },
  { "_id": 6, "name": "Book", "price": 500, "category": "Education", "rating": 4.7, "inStock": true },
  { "_id": 7, "name": "Watch", "price": 8000, "category": "Electronics", "rating": 4.1, "inStock": false }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Products with price greater than 5000

```js
db.products.find({ price: { $gt: 5000 } })
```

---

### 🔹 Task 2: Products with price between 2000 and 10000

```js
db.products.find({ 
  price: { $gte: 2000, $lte: 10000 } 
})
```

---

### 🔹 Task 3: Products that are not in the Electronics category

```js
db.products.find({ category: { $ne: "Electronics" } })
```

---

### 🔹 Task 4: Products in Fashion or Education category

```js
db.products.find({ 
  category: { $in: ["Fashion", "Education"] } 
})
```

---

### 🔹 Task 5: In-stock products with rating >= 4.2

```js
db.products.find({
  $and: [
    { inStock: true },
    { rating: { $gte: 4.2 } }
  ]
})
```

---

### 🔹 Task 6: Products that are either Electronics or have price < 1000

```js
db.products.find({
  $or: [
    { category: "Electronics" },
    { price: { $lt: 1000 } }
  ]
})
```

---

### 🔹 Task 7: Out of stock products that are not in Fashion category

```js
db.products.find({
  $and: [
    { inStock: false },
    { category: { $ne: "Fashion" } }
  ]
})
```

---

## 💡 Key Takeaways

- Comparison operators are used inside the field object.
- `$and` is often optional (you can just list conditions), but useful for clarity.
- `$or` is very commonly used for multiple alternative conditions.
- `$in` is cleaner than writing multiple `$or` conditions for the same field.
- These operators are the foundation of almost every MongoDB query.

---

## 🔥 Bonus Challenge

Write a query that finds products which:
- Are in stock
- Belong to Electronics or Fashion
- Have price less than or equal to 30000
- And rating greater than 4.0
```
