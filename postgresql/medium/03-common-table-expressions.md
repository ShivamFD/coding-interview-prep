# 🧠 PostgreSQL: Common Table Expressions (CTEs)

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-CTE%20%2F%20WITH-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

A **Common Table Expression (CTE)** is a temporary result set that you can reference within a `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement.

It is defined using the `WITH` clause and makes complex queries much more readable.

### Why use CTEs?

- Makes long queries easier to read and maintain
- Allows you to break a complex problem into smaller steps
- Can be referenced multiple times in the same query
- Supports recursive queries (advanced use case)

---

## 🛠️ Basic Syntax

```sql
WITH cte_name AS (
    -- your query here
)
SELECT * FROM cte_name;
```

You can also define multiple CTEs:

```sql
WITH cte1 AS (...),
     cte2 AS (...)
SELECT * FROM cte1
JOIN cte2 ON ...
```

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
| 8  | Anjali   | IT         | 75000  |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Find employees who earn more than the average salary (using CTE)

```sql
WITH avg_salary AS (
    SELECT AVG(salary) AS average_salary
    FROM employees
)
SELECT e.name, e.department, e.salary
FROM employees e, avg_salary a
WHERE e.salary > a.average_salary;
```

---

### 🔹 Task 2: Find the highest salary in each department using CTE

```sql
WITH dept_max AS (
    SELECT 
        department,
        MAX(salary) AS max_salary
    FROM employees
    GROUP BY department
)
SELECT e.name, e.department, e.salary
FROM employees e
JOIN dept_max d ON e.department = d.department 
               AND e.salary = d.max_salary;
```

---

### 🔹 Task 3: Calculate total salary and average salary of the company using multiple CTEs

```sql
WITH total AS (
    SELECT SUM(salary) AS total_salary FROM employees
),
average AS (
    SELECT AVG(salary) AS avg_salary FROM employees
)
SELECT 
    t.total_salary,
    a.avg_salary
FROM total t, average a;
```

---

### 🔹 Task 4: Find departments that have more than 2 employees (using CTE)

```sql
WITH dept_count AS (
    SELECT 
        department,
        COUNT(*) AS emp_count
    FROM employees
    GROUP BY department
)
SELECT department, emp_count
FROM dept_count
WHERE emp_count > 2;
```

---

### 🔹 Task 5: Show each employee with their department’s average salary (using CTE)

```sql
WITH dept_avg AS (
    SELECT 
        department,
        AVG(salary) AS dept_avg_salary
    FROM employees
    GROUP BY department
)
SELECT 
    e.name,
    e.department,
    e.salary,
    d.dept_avg_salary
FROM employees e
JOIN dept_avg d ON e.department = d.department;
```

---

## 💡 Key Takeaways

- CTEs improve readability of complex queries.
- A CTE exists only for the duration of the query.
- You can use multiple CTEs in a single statement.
- CTEs are often preferred over subqueries for better clarity.
- Recursive CTEs are used for hierarchical data (advanced topic).

---

## 🔥 Bonus Challenge

Write a query using CTE to find the **second highest salary** in the entire company.
```
