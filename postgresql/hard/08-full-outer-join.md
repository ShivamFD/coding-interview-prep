# 🔄 Advanced PostgreSQL: FULL OUTER JOIN

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-FULL%20OUTER%20JOIN-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

A **FULL OUTER JOIN** returns all rows from both tables.  
When there is no match, it fills the missing side with `NULL`.

### When to use FULL OUTER JOIN?
- Comparing two datasets
- Finding missing records in either table
- Data reconciliation / auditing
- Finding unmatched records from both sides

### Visual Understanding

```
Table A          Table B
--------         --------
1                1
2                3
4                5

FULL OUTER JOIN result will contain:
1 (matched)
2 (only in A)
3 (only in B)
4 (only in A)
5 (only in B)
```

---

## 🗄️ Datasets

**employees_2023**

| id | name     | department | salary |
|:--:|:---------|:-----------|-------:|
| 1  | Rahul    | IT         | 60000  |
| 2  | Priya    | HR         | 45000  |
| 3  | Amit     | IT         | 75000  |
| 4  | Sneha    | Finance    | 52000  |
| 5  | Vikram   | IT         | 68000  |

**employees_2024**

| id | name     | department | salary |
|:--:|:---------|:-----------|-------:|
| 1  | Rahul    | IT         | 65000  |
| 2  | Priya    | HR         | 48000  |
| 3  | Amit     | IT         | 78000  |
| 6  | Neha     | HR         | 50000  |
| 7  | Karan    | Finance    | 82000  |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Basic FULL OUTER JOIN

```sql
SELECT 
    COALESCE(e23.id, e24.id) AS id,
    e23.name AS name_2023,
    e24.name AS name_2024,
    e23.salary AS salary_2023,
    e24.salary AS salary_2024
FROM employees_2023 e23
FULL OUTER JOIN employees_2024 e24 ON e23.id = e24.id
ORDER BY id;
```

---

### 🔹 Task 2: Find employees who exist only in 2023 (left only)

```sql
SELECT 
    e23.id,
    e23.name,
    e23.department,
    e23.salary
FROM employees_2023 e23
FULL OUTER JOIN employees_2024 e24 ON e23.id = e24.id
WHERE e24.id IS NULL;
```

---

### 🔹 Task 3: Find employees who exist only in 2024 (right only)

```sql
SELECT 
    e24.id,
    e24.name,
    e24.department,
    e24.salary
FROM employees_2023 e23
FULL OUTER JOIN employees_2024 e24 ON e23.id = e24.id
WHERE e23.id IS NULL;
```

---

### 🔹 Task 4: Find employees present in both years with salary change

```sql
SELECT 
    e23.id,
    e23.name,
    e23.salary AS salary_2023,
    e24.salary AS salary_2024,
    e24.salary - e23.salary AS salary_difference
FROM employees_2023 e23
FULL OUTER JOIN employees_2024 e24 ON e23.id = e24.id
WHERE e23.id IS NOT NULL AND e24.id IS NOT NULL
ORDER BY salary_difference DESC;
```

---

### 🔹 Task 5: Complete Data Comparison Report

```sql
SELECT 
    COALESCE(e23.id, e24.id) AS id,
    COALESCE(e23.name, e24.name) AS name,
    CASE 
        WHEN e23.id IS NOT NULL AND e24.id IS NOT NULL THEN 'Present in Both'
        WHEN e23.id IS NOT NULL AND e24.id IS NULL THEN 'Only in 2023'
        WHEN e23.id IS NULL AND e24.id IS NOT NULL THEN 'Only in 2024'
    END AS status,
    e23.salary AS salary_2023,
    e24.salary AS salary_2024,
    COALESCE(e24.salary, 0) - COALESCE(e23.salary, 0) AS difference
FROM employees_2023 e23
FULL OUTER JOIN employees_2024 e24 ON e23.id = e24.id
ORDER BY status, id;
```

---

### 🔹 Task 6: Find employees whose salary increased by more than 5000

```sql
SELECT 
    e23.name,
    e23.salary AS old_salary,
    e24.salary AS new_salary,
    e24.salary - e23.salary AS increment
FROM employees_2023 e23
INNER JOIN employees_2024 e24 ON e23.id = e24.id
WHERE e24.salary - e23.salary > 5000
ORDER BY increment DESC;
```

---

## 🧠 Detailed Explanation

### How FULL OUTER JOIN works:

- It returns **matched rows** + **unmatched rows from left** + **unmatched rows from right**.
- Unmatched rows will have `NULL` on the opposite side.
- We use `WHERE e24.id IS NULL` to find records only in the left table.
- We use `WHERE e23.id IS NULL` to find records only in the right table.
- `COALESCE` is very commonly used with FULL OUTER JOIN to pick the non-null value.

---

## 💡 Key Takeaways for Interviews

- FULL OUTER JOIN is perfect for data comparison and reconciliation.
- Always use `COALESCE` when selecting columns that can be NULL.
- Filtering with `IS NULL` after a FULL OUTER JOIN is a classic pattern to find missing records.
- FULL OUTER JOIN is less commonly used than INNER/LEFT, but very important for auditing and migration tasks.
- Many databases (like MySQL) do not support FULL OUTER JOIN directly (you have to emulate it with UNION).

---

## 🔥 Bonus Challenge

Write a query that shows a complete report with these columns:
- Employee Name
- Status (`Only in 2023` / `Only in 2024` / `Present in Both`)
- Salary 2023
- Salary 2024
- Increment (or NULL if not present in both)
- And sort the result by Status and then by Name
```
