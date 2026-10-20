# 🧩 PostgreSQL: COALESCE and NULLIF

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-COALESCE%20%26%20NULLIF-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

These two functions are very useful when working with `NULL` values.

| Function              | Description                                              |
|-----------------------|----------------------------------------------------------|
| `COALESCE(a, b, c...)`| Returns the first non-NULL value from the list           |
| `NULLIF(a, b)`        | Returns `NULL` if a equals b, otherwise returns a        |

### Why are they important?
- Replacing NULL values with default values
- Avoiding division by zero
- Cleaning data
- Writing safer queries

---

## 🗄️ Dataset: `employees` Table

| id | name     | department | salary | bonus  | manager_id |
|:--:|:---------|:-----------|-------:|-------:|:----------:|
| 1  | Rahul    | IT         | 60000  | 5000   | NULL       |
| 2  | Priya    | HR         | 45000  | NULL   | 1          |
| 3  | Amit     | IT         | 75000  | 8000   | 1          |
| 4  | Sneha    | Finance    | NULL   | 3000   | 2          |
| 5  | Vikram   | IT         | 68000  | NULL   | NULL       |
| 6  | Neha     | HR         | 48000  | 2000   | 2          |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Replace NULL salary with 0

```sql
SELECT 
    name,
    COALESCE(salary, 0) AS salary
FROM employees;
```

---

### 🔹 Task 2: Replace NULL bonus with 0 and calculate total income

```sql
SELECT 
    name,
    salary,
    COALESCE(bonus, 0) AS bonus,
    COALESCE(salary, 0) + COALESCE(bonus, 0) AS total_income
FROM employees;
```

---

### 🔹 Task 3: Show "No Manager" when manager_id is NULL

```sql
SELECT 
    name,
    COALESCE(manager_id::TEXT, 'No Manager') AS manager
FROM employees;
```

---

### 🔹 Task 4: Use COALESCE with multiple fallback values

```sql
SELECT 
    name,
    COALESCE(bonus, salary, 0) AS amount
FROM employees;
```

---

### 🔹 Task 5: Prevent division by zero using NULLIF

```sql
SELECT 
    name,
    salary,
    bonus,
    ROUND(salary / NULLIF(bonus, 0), 2) AS salary_to_bonus_ratio
FROM employees;
```

> If bonus is 0 or NULL, `NULLIF` will return NULL and avoid the division by zero error.

---

### 🔹 Task 6: Combine COALESCE and NULLIF

```sql
SELECT 
    name,
    COALESCE(ROUND(salary / NULLIF(bonus, 0), 2), 0) AS safe_ratio
FROM employees;
```

---

## 💡 Key Takeaways

- `COALESCE` is the most common way to handle NULL values.
- You can pass multiple arguments to `COALESCE` — it returns the first non-NULL one.
- `NULLIF(a, b)` is very useful to avoid division by zero.
- `NULLIF` returns NULL when both values are equal.
- These two functions are frequently used together in real-world queries.

---

## 🔥 Bonus Challenge

Write a query that shows:
- Employee name
- Salary (replace NULL with 0)
- Bonus (replace NULL with 0)
- Total income
- A column `status`:
  - 'Has Manager' if manager_id is not NULL
  - 'No Manager' if manager_id is NULL
```
