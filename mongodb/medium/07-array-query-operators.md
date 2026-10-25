# 🔍 MongoDB: Array Query Operators

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Array%20Query%20Operators-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

When working with arrays in MongoDB, we need special operators to query them effectively.

### Most Important Array Query Operators

| Operator       | Description                                              |
|----------------|----------------------------------------------------------|
| `$elemMatch`   | Matches documents that contain an array element matching all specified conditions |
| `$size`        | Matches arrays with the exact number of elements         |
| `$all`         | Matches arrays that contain all the specified elements   |
| `$in`          | Matches any value in the given array                     |
| `$nin`         | Matches none of the values in the given array            |

---

## 🗄️ Sample Collection: `products`

```json
[
  {
    "_id": 1,
    "name": "Laptop",
    "tags": ["electronics", "computers"],
    "ratings": [
      { "user": "Rahul", "score": 5 },
      { "user": "Priya", "score": 4 }
    ],
    "sizes": [14, 15, 16]
  },
  {
    "_id": 2,
    "name": "Mobile",
    "tags": ["electronics", "mobile"],
    "ratings": [
      { "user": "Amit", "score": 3 },
      { "user": "Sneha", "score": 5 },
      { "user": "Vikram", "score": 4 }
    ],
    "sizes": [6]
  },
  {
    "_id": 3,
    "name": "Headphones",
    "tags": ["electronics", "audio"],
    "ratings": [
      { "user": "Neha", "score": 5 }
    ],
    "sizes": []
  },
  {
    "_id": 4,
    "name": "Book",
    "tags": ["education", "reading"],
    "ratings": [
      { "user": "Karan", "score": 4 },
      { "user": "Anjali", "score": 5 }
    ],
    "sizes": null
  }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Find products that have exactly 2 tags

```js
db.products.find({ tags: { $size: 2 } })
```

---

### 🔹 Task 2: Find products that have both "electronics" and "computers" tags

```js
db.products.find({ tags: { $all: ["electronics", "computers"] } })
```

---

### 🔹 Task 3: Find products that have a rating of 5 by any user

```js
db.products.find({ "ratings.score": 5 })
```

---

### 🔹 Task 4: Find products where a user named "Priya" gave a score of 4 or more (using $elemMatch)

```js
db.products.find({
  ratings: {
    $elemMatch: {
      user: "Priya",
      score: { $gte: 4 }
    }
  }
})
```

---

### 🔹 Task 5: Find products that have at least one size greater than 10

```js
db.products.find({ sizes: { $elemMatch: { $gt: 10 } } })
```

---

### 🔹 Task 6: Find products whose tags include either "mobile" or "audio"

```js
db.products.find({ tags: { $in: ["mobile", "audio"] } })
```

---

### 🔹 Task 7: Find products that do not have the tag "electronics"

```js
db.products.find({ tags: { $nin: ["electronics"] } })
```

---

### 🔹 Task 8: Find products that have exactly 3 ratings

```js
db.products.find({ ratings: { $size: 3 } })
```

---

## 💡 Key Takeaways

- `$size` → exact length of array
- `$all` → must contain all specified values
- `$elemMatch` → used when multiple conditions must match on the **same** array element
- Without `$elemMatch`, conditions on array fields can match different elements
- `$in` and `$nin` are very useful for matching against lists
- These operators are extremely common in real-world MongoDB queries

---

## 🔥 Bonus Challenge

Write a query that finds products which:
- Have the tag "electronics"
- Have at least one rating with score ≥ 5
- And have more than 1 rating
```
