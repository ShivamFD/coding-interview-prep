# 📊 MongoDB: Advanced $group

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Advanced%20%24group-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

The `$group` stage is one of the most important stages in the Aggregation Pipeline.

It allows us to:
- Group documents by one or more fields
- Perform calculations (sum, avg, min, max, count)
- Push values into arrays
- Create complex reports

---

## 🗄️ Sample Collection: `orders`

```json
[
  { "_id": 1, "customer": "Rahul", "city": "Delhi", "amount": 2500, "status": "delivered", "category": "Electronics" },
  { "_id": 2, "customer": "Priya", "city": "Mumbai", "amount": 1800, "status": "pending", "category": "Fashion" },
  { "_id": 3, "customer": "Amit", "city": "Delhi", "amount": 3200, "status": "delivered", "category": "Electronics" },
  { "_id": 4, "customer": "Sneha", "city": "Bangalore", "amount": 1500, "status": "cancelled", "category": "Fashion" },
  { "_id": 5, "customer": "Vikram", "city": "Delhi", "amount": 4100, "status": "delivered", "category": "Electronics" },
  { "_id": 6, "customer": "Neha", "city": "Mumbai", "amount": 2200, "status": "delivered", "category": "Home" },
  { "_id": 7, "customer": "Karan", "city": "Delhi", "amount": 1700, "status": "pending", "category": "Fashion" },
  { "_id": 8, "customer": "Anjali", "city": "Pune", "amount": 2900, "status": "delivered", "category": "Electronics" },
  { "_id": 9, "customer": "Rohan", "city": "Mumbai", "amount": 1200, "status": "cancelled", "category": "Home" },
  { "_id": 10, "customer": "Meera", "city": "Bangalore", "amount": 3500, "status": "delivered", "category": "Electronics" }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Total revenue and number of orders by city

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$city",
      totalRevenue: { $sum: "$amount" },
      orderCount: { $sum: 1 }
    }
  },
  { $sort: { totalRevenue: -1 } }
])
```

---

### 🔹 Task 2: Average order value by status

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$status",
      averageAmount: { $avg: "$amount" },
      count: { $sum: 1 }
    }
  }
])
```

---

### 🔹 Task 3: Highest and lowest order amount by category

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$category",
      maxAmount: { $max: "$amount" },
      minAmount: { $min: "$amount" },
      totalOrders: { $sum: 1 }
    }
  }
])
```

---

### 🔹 Task 4: List of customers per city (using $push)

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$city",
      customers: { $push: "$customer" },
      totalRevenue: { $sum: "$amount" }
    }
  }
])
```

---

### 🔹 Task 5: Unique customers per city (using $addToSet)

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$city",
      uniqueCustomers: { $addToSet: "$customer" }
    }
  }
])
```

---

### 🔹 Task 6: Group by multiple fields (city + status)

```js
db.orders.aggregate([
  {
    $group: {
      _id: {
        city: "$city",
        status: "$status"
      },
      totalAmount: { $sum: "$amount" },
      count: { $sum: 1 }
    }
  },
  { $sort: { "_id.city": 1, "_id.status": 1 } }
])
```

---

## 💡 Key Takeaways

- `_id` is mandatory in `$group` — it defines how documents are grouped.
- `$sum: 1` is the standard way to count documents.
- `$push` keeps duplicates, `$addToSet` keeps only unique values.
- You can group by multiple fields using an object as `_id`.
- `$group` is usually followed by `$sort`, `$project`, or `$match`.

---

## 🔥 Bonus Challenge

Write an aggregation that shows for each category:
- Total revenue
- Number of orders
- Average order value
- List of unique cities where that category was ordered
```
