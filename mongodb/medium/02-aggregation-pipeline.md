# 📊 MongoDB: Aggregation Pipeline Basics

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Aggregation%20Pipeline-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

The **Aggregation Pipeline** is one of the most powerful features of MongoDB.  
It allows you to process and transform documents through multiple stages (like a data processing pipeline).

Each stage transforms the documents and passes the result to the next stage.

### Most Common Stages

| Stage         | Purpose                                      |
|---------------|----------------------------------------------|
| `$match`      | Filters documents (like WHERE)               |
| `$group`      | Groups documents and performs aggregations   |
| `$project`    | Reshapes documents (select/rename fields)    |
| `$sort`       | Sorts documents                              |
| `$limit`      | Limits the number of documents               |
| `$skip`       | Skips documents                              |
| `$count`      | Counts the number of documents               |
| `$lookup`     | Performs a left join with another collection |

---

## 🗄️ Sample Collection: `orders`

```json
[
  { "_id": 1, "customer": "Rahul", "city": "Delhi", "amount": 2500, "status": "delivered" },
  { "_id": 2, "customer": "Priya", "city": "Mumbai", "amount": 1800, "status": "pending" },
  { "_id": 3, "customer": "Amit", "city": "Delhi", "amount": 3200, "status": "delivered" },
  { "_id": 4, "customer": "Sneha", "city": "Bangalore", "amount": 1500, "status": "cancelled" },
  { "_id": 5, "customer": "Vikram", "city": "Delhi", "amount": 4100, "status": "delivered" },
  { "_id": 6, "customer": "Neha", "city": "Mumbai", "amount": 2200, "status": "delivered" },
  { "_id": 7, "customer": "Karan", "city": "Delhi", "amount": 1700, "status": "pending" },
  { "_id": 8, "customer": "Anjali", "city": "Pune", "amount": 2900, "status": "delivered" }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Find total revenue from delivered orders

```js
db.orders.aggregate([
  { $match: { status: "delivered" } },
  { $group: { _id: null, totalRevenue: { $sum: "$amount" } } }
])
```

---

### 🔹 Task 2: Total revenue and number of orders by city

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

### 🔹 Task 3: Average order amount by status

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

### 🔹 Task 4: Top 3 cities by revenue

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$city",
      totalRevenue: { $sum: "$amount" }
    }
  },
  { $sort: { totalRevenue: -1 } },
  { $limit: 3 }
])
```

---

### 🔹 Task 5: Count of orders by status (using $project)

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$status",
      count: { $sum: 1 }
    }
  },
  {
    $project: {
      status: "$_id",
      count: 1,
      _id: 0
    }
  }
])
```

---

### 🔹 Task 6: Find customers who have placed delivered orders worth more than 2000

```js
db.orders.aggregate([
  { $match: { status: "delivered", amount: { $gt: 2000 } } },
  {
    $project: {
      customer: 1,
      amount: 1,
      city: 1,
      _id: 0
    }
  }
])
```

---

## 💡 Key Takeaways

- `$match` should be used as early as possible for better performance.
- `$group` is used for aggregations (`$sum`, `$avg`, `$min`, `$max`, `$count`).
- `_id` in `$group` is mandatory (it defines how documents are grouped).
- `$project` is used to reshape the output.
- Aggregation pipeline is one of the most important topics for MongoDB interviews.

---

## 🔥 Bonus Challenge

Write an aggregation query that shows:
- City
- Total revenue from delivered orders only
- Number of delivered orders
- Average order value of delivered orders
- Sort by total revenue (highest first)
```
