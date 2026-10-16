# 📊 PostgreSQL: DISTINCT and ORDER BY

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-DISTINCT%20%2B%20ORDER%20BY-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

- `DISTINCT` is used to remove duplicate rows from the result.
- `ORDER BY` is used to sort the result in ascending (`ASC`) or descending (`DESC`) order.

These two are among the most frequently used clauses in SQL.

---

## 🗄️ Dataset: `employees` Table

| id | name     | department | salary | city     |
|:--:|:---------|:-----------|-------:|:---------|
| 1  | Rahul    | IT         | 60000  | Delhi    |
| 2  | Priya    | HR         | 45000  | Mumbai   |
| 3  | Amit     | IT         | 75000  | Delhi    |
| 4  | Sneha    | Finance    | 52000  | Bangalore|
| 5  | Vikram   | IT         | 68000  | Delhi    |
| 6  | Neha     | HR         | 48000  | Mumbai   |
| 7  | Karan    | Finance    | 81000  | Delhi    |
| 8  | Anjali   | IT         | 75000  | Pune     |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Get all unique departments

```sql
SELECT DISTINCT department
FROM employees;
```

---

### 🔹 Task 2: Get all unique cities

```sql
SELECT DISTINCT city
FROM employees
ORDER BY city;
```

---

### 🔹 Task 3: Get unique department + city combinations

```sql
SELECT DISTINCT department, city
FROM employees
ORDER BY department, city;
```

---

### 🔹 Task 4: Show all employees sorted by salary (highest to lowest)

```sql
SELECT name, department, salary
FROM employees
ORDER BY salary DESC;
```

---

### 🔹 Task 5: Sort by department (A-Z) and then by salary (highest to lowest)

```sql
SELECT name, department, salary
FROM employees
ORDER BY department ASC, salary DESC;
```

---

### 🔹 Task 6: Get the top 3 highest paid employees

```sql
SELECT name, department, salary
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

---

## 💡 Key Takeaways

- `DISTINCT` removes duplicate rows based on the selected columns.
- You can use `DISTINCT` on multiple columns.
- `ORDER BY` comes after `WHERE` and before `LIMIT`.
- Default sorting order is `ASC` (ascending).
- You can sort by multiple columns (priority from left to right).
- `LIMIT` is commonly used with `ORDER BY` to get Top-N records.

---

## 🔥 Bonus Challenge

Write a query to display unique cities where IT department employees live, sorted alphabetically.
```
