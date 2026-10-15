# 🗄️ PostgreSQL: Working with NULL Values

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-NULL%20Handling-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

In SQL, `NULL` represents a missing or unknown value.  
It is **not** the same as zero or an empty string.

Handling `NULL` correctly is very important in real-world queries.

### Important Operators & Functions

| Keyword / Function     | Purpose                                      |
|------------------------|----------------------------------------------|
| `IS NULL`              | Checks if a value is NULL                    |
| `IS NOT NULL`          | Checks if a value is not NULL                |
| `COALESCE(a, b, ...)`  | Returns the first non-NULL value             |
| `NULLIF(a, b)`         | Returns NULL if a equals b, otherwise returns a |

---

## 🗄️ Dataset: `employees` Table

| id | name     | department | salary | manager_id |
|:--:|:---------|:-----------|-------:|:----------:|
| 1  | Rahul    | IT         | 60000  | NULL       |
| 2  | Priya    | HR         | 45000  | 1          |
| 3  | Amit     | IT         | 75000  | 1          |
| 4  | Sneha    | Finance    | NULL   | 2          |
| 5  | Vikram   | IT         | 68000  | NULL       |
| 6  | Neha     | HR         | 48000  | 2          |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Find employees who have no manager

```sql
SELECT name, department, salary
FROM employees
WHERE manager_id IS NULL;
```

---

### 🔹 Task 2: Find employees who have a salary (salary is not missing)

```sql
SELECT name, department, salary
FROM employees
WHERE salary IS NOT NULL;
```

---

### 🔹 Task 3: Replace NULL salary with 0 using COALESCE

```sql
SELECT 
    name,
    department,
    COALESCE(salary, 0) AS salary
FROM employees;
```

---

### 🔹 Task 4: Show "No Manager" when manager_id is NULL

```sql
SELECT 
    name,
    department,
    COALESCE(manager_id::TEXT, 'No Manager') AS manager
FROM employees;
```

---

### 🔹 Task 5: Count how many employees have missing salary

```sql
SELECT COUNT(*) AS employees_with_null_salary
FROM employees
WHERE salary IS NULL;
```

---

## 💡 Key Takeaways

- Never use `= NULL` or `!= NULL`. Always use `IS NULL` / `IS NOT NULL`.
- `COALESCE` is the most common way to replace NULL values.
- `NULL` is ignored by most aggregate functions (`SUM`, `AVG`, `COUNT(column)`).
- `COUNT(*)` counts all rows, including those with NULL values.

---

## 🔥 Bonus Challenge

Write a query that shows:
- Employee name
- Salary (replace NULL with 0)
- A new column `salary_status`:
  - 'Missing' if salary is NULL
  - 'Available' if salary is not NULL
```
