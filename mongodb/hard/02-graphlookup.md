# 🌳 MongoDB: $graphLookup (Hierarchical Data)

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-%24graphLookup-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`$graphLookup` is a powerful aggregation stage that performs a **recursive search** on a collection.

It is used for hierarchical or graph-like data such as:

- Organization hierarchy (employees → managers)
- Category trees
- Nested comments / replies
- Friend networks
- Folder structures

It is MongoDB’s version of a recursive CTE.

---

## 🗄️ Sample Collection: `employees`

```json
[
  { "_id": 1, "name": "Rahul", "reportsTo": null },
  { "_id": 2, "name": "Priya", "reportsTo": 1 },
  { "_id": 3, "name": "Amit", "reportsTo": 1 },
  { "_id": 4, "name": "Sneha", "reportsTo": 2 },
  { "_id": 5, "name": "Vikram", "reportsTo": 2 },
  { "_id": 6, "name": "Neha", "reportsTo": 3 },
  { "_id": 7, "name": "Karan", "reportsTo": 3 },
  { "_id": 8, "name": "Anjali", "reportsTo": 4 }
]
```

**Hierarchy:**

```
Rahul
├── Priya
│   ├── Sneha
│   │   └── Anjali
│   └── Vikram
└── Amit
    ├── Neha
    └── Karan
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Find all people who report directly or indirectly to Rahul

```js
db.employees.aggregate([
  {
    $match: { name: "Rahul" }
  },
  {
    $graphLookup: {
      from: "employees",
      startWith: "$_id",
      connectFromField: "_id",
      connectToField: "reportsTo",
      as: "subordinates"
    }
  }
])
```

---

### 🔹 Task 2: Find the full reporting chain of Anjali (from her up to the top)

```js
db.employees.aggregate([
  {
    $match: { name: "Anjali" }
  },
  {
    $graphLookup: {
      from: "employees",
      startWith: "$reportsTo",
      connectFromField: "reportsTo",
      connectToField: "_id",
      as: "reportingChain"
    }
  }
])
```

---

### 🔹 Task 3: Limit the depth of recursion (maxDepth)

```js
db.employees.aggregate([
  {
    $match: { name: "Rahul" }
  },
  {
    $graphLookup: {
      from: "employees",
      startWith: "$_id",
      connectFromField: "_id",
      connectToField: "reportsTo",
      as: "subordinates",
      maxDepth: 1   // only direct reports
    }
  }
])
```

---

### 🔹 Task 4: Add depth information using depthField

```js
db.employees.aggregate([
  {
    $match: { name: "Rahul" }
  },
  {
    $graphLookup: {
      from: "employees",
      startWith: "$_id",
      connectFromField: "_id",
      connectToField: "reportsTo",
      as: "subordinates",
      depthField: "level"
    }
  }
])
```

---

### 🔹 Task 5: Restrict recursion with restrictSearchWithMatch

```js
db.employees.aggregate([
  {
    $match: { name: "Rahul" }
  },
  {
    $graphLookup: {
      from: "employees",
      startWith: "$_id",
      connectFromField: "_id",
      connectToField: "reportsTo",
      as: "subordinates",
      restrictSearchWithMatch: { name: { $ne: "Vikram" } }
    }
  }
])
```

---

## 🧠 Detailed Explanation

### Important Parameters:

| Parameter                  | Description                                      |
|----------------------------|--------------------------------------------------|
| `from`                     | Collection to search                             |
| `startWith`                | Starting value for the recursion                 |
| `connectFromField`         | Field whose value is followed                    |
| `connectToField`           | Field to match against                           |
| `as`                       | Output array field                               |
| `maxDepth`                 | Maximum recursion depth                          |
| `depthField`               | Adds a field showing the depth of each document  |
| `restrictSearchWithMatch`  | Filter applied during recursion                  |

---

## 💡 Key Takeaways for Interviews

- `$graphLookup` is used for hierarchical / recursive data.
- It is the MongoDB equivalent of SQL Recursive CTEs.
- Always be careful with depth — unrestricted recursion can be expensive.
- Very useful for organization charts, nested comments, category trees, etc.
- One of the most advanced and powerful stages in MongoDB aggregation.

---

## 🔥 Bonus Challenge

Write a query that returns every employee along with:
- Their complete list of subordinates
- The number of subordinates they have
```
