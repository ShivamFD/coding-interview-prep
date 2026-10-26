# 🛠️ MongoDB: $project & $addFields

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-%24project%20%26%20%24addFields-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

In the Aggregation Pipeline, we often need to reshape documents.

| Stage          | Purpose                                      |
|----------------|----------------------------------------------|
| `$project`     | Select, rename, exclude fields, or create computed fields |
| `$addFields`   | Add new fields without removing existing ones |

`$addFields` is safer when you only want to add fields.  
`$project` gives you full control over the shape of the output.

---

## 🗄️ Sample Collection: `employees`

```json
[
  { "_id": 1, "firstName": "Rahul", "lastName": "Sharma", "salary": 60000, "department": "IT" },
  { "_id": 2, "firstName": "Priya", "lastName": "Patel", "salary": 45000, "department": "HR" },
  { "_id": 3, "firstName": "Amit", "lastName": "Kumar", "salary": 75000, "department": "IT" },
  { "_id": 4, "firstName": "Sneha", "lastName": "Reddy", "salary": 52000, "department": "Finance" },
  { "_id": 5, "firstName": "Vikram", "lastName": "Singh", "salary": 68000, "department": "IT" }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Select only specific fields using $project

```js
db.employees.aggregate([
  {
    $project: {
      firstName: 1,
      salary: 1,
      _id: 0
    }
  }
])
```

---

### 🔹 Task 2: Create a full name field using $project

```js
db.employees.aggregate([
  {
    $project: {
      fullName: { $concat: ["$firstName", " ", "$lastName"] },
      department: 1,
      salary: 1,
      _id: 0
    }
  }
])
```

---

### 🔹 Task 3: Add a new field without removing others ($addFields)

```js
db.employees.aggregate([
  {
    $addFields: {
      fullName: { $concat: ["$firstName", " ", "$lastName"] },
      annualSalary: { $multiply: ["$salary", 12] }
    }
  }
])
```

---

### 🔹 Task 4: Calculate tax (10% of salary) and net salary

```js
db.employees.aggregate([
  {
    $addFields: {
      tax: { $multiply: ["$salary", 0.1] },
      netSalary: { $subtract: ["$salary", { $multiply: ["$salary", 0.1] }] }
    }
  },
  {
    $project: {
      fullName: { $concat: ["$firstName", " ", "$lastName"] },
      salary: 1,
      tax: 1,
      netSalary: 1,
      _id: 0
    }
  }
])
```

---

### 🔹 Task 5: Rename a field

```js
db.employees.aggregate([
  {
    $project: {
      name: "$firstName",
      dept: "$department",
      monthlySalary: "$salary",
      _id: 0
    }
  }
])
```

---

### 🔹 Task 6: Conditional field using $cond

```js
db.employees.aggregate([
  {
    $addFields: {
      level: {
        $cond: {
          if: { $gte: ["$salary", 65000] },
          then: "Senior",
          else: "Junior"
        }
      }
    }
  }
])
```

---

## 💡 Key Takeaways

- `$project` → controls exactly which fields appear in the output.
- `$addFields` → adds new fields while keeping all existing ones.
- Use `$concat`, `$multiply`, `$subtract`, `$cond` etc. inside these stages.
- `_id` is included by default — exclude it with `_id: 0` when needed.
- These stages are extremely common in reporting and data transformation pipelines.

---

## 🔥 Bonus Challenge

Write an aggregation that returns:
- Full name
- Department
- Monthly salary
- Annual salary
- Level (`Senior` if salary ≥ 60000, otherwise `Junior`)
```
