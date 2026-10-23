# 📊 MongoDB: $facet Stage

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-%24facet-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

The `$facet` stage allows you to process multiple **aggregation pipelines** within a single stage on the same set of input documents.

Each sub-pipeline has its own independent output, and all of them run in parallel.

### Why use `$facet`?
- Generate multiple reports in a single query
- Create dashboards with different metrics
- Avoid multiple round-trips to the database
- Very useful for analytics and admin panels

### Basic Syntax

```js
{
  $facet: {
    "outputField1": [ /* pipeline 1 */ ],
    "outputField2": [ /* pipeline 2 */ ],
    "outputField3": [ /* pipeline 3 */ ]
  }
}
```

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

### 🔹 Task 1: Basic $facet – Multiple reports in one query

```js
db.orders.aggregate([
  {
    $facet: {
      "totalRevenue": [
        { $group: { _id: null, total: { $sum: "$amount" } } }
      ],
      "statusBreakdown": [
        { $group: { _id: "$status", count: { $sum: 1 } } }
      ],
      "topCities": [
        { $group: { _id: "$city", revenue: { $sum: "$amount" } } },
        { $sort: { revenue: -1 } },
        { $limit: 3 }
      ]
    }
  }
])
```

---

### 🔹 Task 2: Sales Dashboard with $facet

```js
db.orders.aggregate([
  {
    $facet: {
      "overview": [
        {
          $group: {
            _id: null,
            totalOrders: { $sum: 1 },
            totalRevenue: { $sum: "$amount" },
            averageOrderValue: { $avg: "$amount" }
          }
        }
      ],
      "byStatus": [
        {
          $group: {
            _id: "$status",
            count: { $sum: 1 },
            revenue: { $sum: "$amount" }
          }
        }
      ],
      "byCategory": [
        {
          $group: {
            _id: "$category",
            count: { $sum: 1 },
            revenue: { $sum: "$amount" }
          }
        },
        { $sort: { revenue: -1 } }
      ],
      "topCustomers": [
        {
          $group: {
            _id: "$customer",
            totalSpent: { $sum: "$amount" },
            orderCount: { $sum: 1 }
          }
        },
        { $sort: { totalSpent: -1 } },
        { $limit: 5 }
      ]
    }
  }
])
```

---

### 🔹 Task 3: Delivered vs Cancelled comparison

```js
db.orders.aggregate([
  {
    $facet: {
      "delivered": [
        { $match: { status: "delivered" } },
        {
          $group: {
            _id: null,
            count: { $sum: 1 },
            revenue: { $sum: "$amount" }
          }
        }
      ],
      "cancelled": [
        { $match: { status: "cancelled" } },
        {
          $group: {
            _id: null,
            count: { $sum: 1 },
            revenue: { $sum: "$amount" }
          }
        }
      ]
    }
  }
])
```

---

## 🧠 Detailed Explanation

- `$facet` runs multiple pipelines on the **same input documents**.
- Each pipeline is independent.
- The output is a single document containing multiple arrays (one for each facet).
- Extremely useful when building admin dashboards or analytics pages.
- It is more efficient than running multiple separate aggregation queries.

---

## 💡 Key Takeaways for Interviews

- `$facet` allows multiple aggregations in a single query.
- Each sub-pipeline has its own stages.
- Output is always a single document with multiple fields.
- Very powerful for reporting and dashboards.
- Can be combined with `$match`, `$group`, `$sort`, `$limit`, etc. inside each facet.
- One of the most advanced and useful stages in MongoDB aggregation.

---

## 🔥 Bonus Challenge

Write a `$facet` query that returns:

1. Total revenue and total orders
2. Revenue breakdown by city (top 3)
3. Count of orders by status
4. Average order value by category
```
