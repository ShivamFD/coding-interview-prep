# 🪟 PostgreSQL: Introduction to Window Functions

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Window%20Functions%20Intro-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Window Functions perform calculations across a set of rows that are related to the current row.

Unlike regular aggregate functions (`GROUP BY`), **Window Functions do not collapse rows**.  
They return a value for every row.

### Basic Syntax

```sql
FUNCTION_NAME() OVER (
  PARTITION BY column_name
  ORDER BY column_name
)
```

### Most Common Window Functions

| Function         | Purpose                              |
|------------------|--------------------------------------|
| `ROW_NUMBER()`   | Unique sequential number             |
| `RANK()`         | Ranking with gaps                    |
| `DENSE_RANK()`   | Ranking without gaps                 |
| `LAG()`          | Access previous row value            |
| `LEAD()`         | Access next row value                |
| `SUM() OVER()`   | Running total                        |
| `AVG() OVER()`   | Moving average                       |

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

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Assign a unique row number to each employee (by salary)

```sql
SELECT 
    name,
    department,
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num
FROM employees;
```

---

### 🔹 Task 2: Rank employees within each department by salary

```sql
SELECT 
    name,
    department,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank_in_dept
FROM employees;
```

---

### 🔹 Task 3: Dense Rank within each department

```sql
SELECT 
    name,
    department,
    salary,
    DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rank
FROM employees;
```

---

### 🔹 Task 4: Show previous employee’s salary (LAG)

```sql
SELECT 
    name,
    department,
    salary,
    LAG(salary) OVER (PARTITION BY department ORDER BY salary DESC) AS previous_salary
FROM employees;
```

---

### 🔹 Task 5: Running total of salary by department

```sql
SELECT 
    name,
    department,
    salary,
    SUM(salary) OVER (PARTITION BY department ORDER BY salary) AS running_total
FROM employees;
```

---

### 🔹 Task 6: Average salary of the department (without collapsing rows)

```sql
SELECT 
    name,
    department,
    salary,
    ROUND(AVG(salary) OVER (PARTITION BY department), 2) AS dept_avg_salary
FROM employees;
```

---

## 💡 Key Takeaways

- Window Functions do **not** reduce the number of rows.
- `PARTITION BY` works like `GROUP BY` but keeps all rows.
- `ORDER BY` inside `OVER()` is required for ranking and running calculations.
- `ROW_NUMBER()` → always unique
- `RANK()` → gaps on ties
- `DENSE_RANK()` → no gaps on ties
- This is one of the most important topics for SQL interviews.

---

## 🔥 Bonus Challenge

Write a query that shows each employee with:
- Their salary
- Department average salary
- Difference between their salary and the department average
```
