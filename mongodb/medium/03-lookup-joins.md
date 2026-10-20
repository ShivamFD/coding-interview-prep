# 🔗 MongoDB: $lookup (Joins)

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-%24lookup%20%2F%20Joins-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

MongoDB is a NoSQL database, but it still supports joins using the **`$lookup`** stage in the Aggregation Pipeline.

`$lookup` performs a **left outer join** between two collections.

### When to use `$lookup`?
- When you have related data in different collections
- Example: Orders + Customers, Posts + Users, Products + Categories

---

## 🗄️ Sample Collections

**customers**

```json
[
  { "_id": 1, "name": "Rahul", "city": "Delhi" },
  { "_id": 2, "name": "Priya", "city": "Mumbai" },
  { "_id": 3, "name": "Amit", "city": "Delhi" },
  { "_id": 4, "name": "Sneha", "city": "Bangalore" }
]
```

**orders**

```json
[
  { "_id": 101, "customer_id": 1, "product": "Laptop", "amount": 55000 },
  { "_id": 102, "customer_id": 1, "product": "Mouse", "amount": 800 },
  { "_id": 103, "customer_id": 2, "product": "Mobile", "amount": 25000 },
  { "_id": 104, "customer_id": 3, "product": "Keyboard", "amount": 1500 },
  { "_id": 105, "customer_id": 1, "product": "Monitor", "amount": 12000 }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Basic $lookup – Join orders with customer details

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",          // target collection
      localField: "customer_id",  // field from orders
      foreignField: "_id",        // field from customers
      as: "customer_info"         // output array field
    }
  }
])
```

---

### 🔹 Task 2: Unwind the joined data (convert array to object)

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customer_id",
      foreignField: "_id",
      as: "customer_info"
    }
  },
  { $unwind: "$customer_info" }
])
```

---

### 🔹 Task 3: Show only required fields after join

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customer_id",
      foreignField: "_id",
      as: "customer_info"
    }
  },
  { $unwind: "$customer_info" },
  {
    $project: {
      _id: 0,
      order_id: "$_id",
      product: 1,
      amount: 1,
      customer_name: "$customer_info.name",
      city: "$customer_info.city"
    }
  }
])
```

---

### 🔹 Task 4: Find total amount spent by each customer

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customer_id",
      foreignField: "_id",
      as: "customer_info"
    }
  },
  { $unwind: "$customer_info" },
  {
    $group: {
      _id: "$customer_info.name",
      totalSpent: { $sum: "$amount" },
      orderCount: { $sum: 1 }
    }
  },
  { $sort: { totalSpent: -1 } }
])
```

---

### 🔹 Task 5: Find customers who have not placed any order (Left join + filter)

```js
db.customers.aggregate([
  {
    $lookup: {
      from: "orders",
      localField: "_id",
      foreignField: "customer_id",
      as: "orders"
    }
  },
  {
    $match: {
      orders: { $size: 0 }
    }
  },
  {
    $project: {
      name: 1,
      city: 1,
      _id: 0
    }
  }
])
```

---

## 💡 Key Takeaways

- `$lookup` is the MongoDB way of doing joins.
- It always returns an **array** in the `as` field.
- Use `$unwind` when you want to convert the array into individual documents.
- `$lookup` + `$group` is a very common interview pattern.
- For better performance, create indexes on the join fields.

---

## 🔥 Bonus Challenge

Write an aggregation that shows:
- Customer name
- City
- List of all products they bought (as an array)
- Total amount spent
```
