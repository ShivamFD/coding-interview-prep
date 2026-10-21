# 📄 PostgreSQL: LIMIT, OFFSET & Pagination

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-LIMIT%20%26%20OFFSET-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`LIMIT` and `OFFSET` are used to control how many rows are returned from a query.

They are the foundation of **pagination** in applications.

| Clause     | Purpose                                      |
|------------|----------------------------------------------|
| `LIMIT n`  | Returns only the first n rows                |
| `OFFSET n` | Skips the first n rows                       |

### Common Use Case
- Page 1 → `LIMIT 10 OFFSET 0`
- Page 2 → `LIMIT 10 OFFSET 10`
- Page 3 → `LIMIT 10 OFFSET 20`

---

## 🗄️ Dataset: `employees` Table

| id | name     | department | salary |
|:--:|:---------|:-----------|-------:|
| 1  | Rahul    | IT         | 60000  |
| 2  | Priya    | HR         | 45000  |
| 3  | Amit     | IT         | 75000  |
| 4  | Sneha    | Finance    | 52000  |
| 5  | Vikram   | IT         | 68000  |
| 6  | Neha     | HR         | 48000  |
| 7  | Karan    | Finance    | 81000  |
| 8  | Anjali   | IT         | 72000  |
| 9  | Rohan    | HR         | 51000  |
| 10 | Meera    | Finance    | 59000  |
| 11 | Suresh   | IT         | 64000  |
| 12 | Kavita   | HR         | 47000  |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Get the first 5 employees

```sql
SELECT * FROM employees
LIMIT 5;
```

---

### 🔹 Task 2: Get employees 6 to 10 (skip first 5)

```sql
SELECT * FROM employees
OFFSET 5
LIMIT 5;
```

---

### 🔹 Task 3: Get top 3 highest paid employees

```sql
SELECT name, department, salary
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

---

### 🔹 Task 4: Get the 4th to 6th highest paid employees

```sql
SELECT name, department, salary
FROM employees
ORDER BY salary DESC
OFFSET 3
LIMIT 3;
```

---

### 🔹 Task 5: Simple Pagination Example (Page Size = 4)

**Page 1:**
```sql
SELECT * FROM employees
ORDER BY id
LIMIT 4 OFFSET 0;
```

**Page 2:**
```sql
SELECT * FROM employees
ORDER BY id
LIMIT 4 OFFSET 4;
```

**Page 3:**
```sql
SELECT * FROM employees
ORDER BY id
LIMIT 4 OFFSET 8;
```

---

### 🔹 Task 6: Get the second highest salary employee

```sql
SELECT name, salary
FROM employees
ORDER BY salary DESC
OFFSET 1
LIMIT 1;
```

---

## 💡 Key Takeaways

- Always use `ORDER BY` with `LIMIT` and `OFFSET` for consistent results.
- `OFFSET` can become slow on large tables (because the database still has to scan the skipped rows).
- For better performance on deep pagination, consider keyset pagination (using `WHERE id > last_id`).
- `LIMIT` without `OFFSET` is very fast.
- This is one of the most commonly used features in real applications.

---

## 🔥 Bonus Challenge

Write a query that returns the 5th highest paid employee in the IT department.
```
