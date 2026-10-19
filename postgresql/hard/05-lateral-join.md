# 🔀 Advanced PostgreSQL: LATERAL Joins

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-LATERAL%20Join-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

A **LATERAL join** allows a subquery in the `FROM` clause to reference columns from previous tables in the `FROM` list.

In simple words:

> LATERAL makes it possible for a subquery to depend on the current row of the preceding table.

This is extremely powerful when you want to:
- Get the top N records per group
- Perform correlated calculations in the FROM clause
- Call set-returning functions that depend on row values
- Write cleaner alternatives to complex window function queries

### Why LATERAL is important?

Without LATERAL, a subquery in the `FROM` clause cannot see columns from other tables.  
With LATERAL, it can.

It is one of the most advanced and useful features of PostgreSQL.

---

## 🧠 Basic Syntax

```sql
SELECT *
FROM table1 t1
LEFT JOIN LATERAL (
    SELECT *
    FROM table2 t2
    WHERE t2.some_column = t1.id
    ORDER BY t2.score DESC
    LIMIT 2
) sub ON true;
```

---

## 🗄️ Dataset

**employees**

| id | name     | department |
|:--:|:---------|:-----------|
| 1  | Rahul    | IT         |
| 2  | Priya    | HR         |
| 3  | Amit     | IT         |
| 4  | Sneha    | Finance    |
| 5  | Vikram   | IT         |

**projects**

| id | employee_id | project_name       | score |
|:--:|:-----------:|:-------------------|------:|
| 1  | 1           | Website Redesign   | 85    |
| 2  | 1           | API Development    | 92    |
| 3  | 1           | Mobile App         | 78    |
| 4  | 3           | Database Migration | 88    |
| 5  | 3           | Cloud Setup        | 95    |
| 6  | 5           | Security Audit     | 80    |
| 7  | 5           | Performance Tuning | 91    |
| 8  | 2           | HR Portal          | 87    |
| 9  | 4           | Finance Dashboard  | 83    |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Get the highest scored project for each employee

```sql
SELECT 
    e.name,
    e.department,
    p.project_name,
    p.score
FROM employees e
LEFT JOIN LATERAL (
    SELECT project_name, score
    FROM projects
    WHERE employee_id = e.id
    ORDER BY score DESC
    LIMIT 1
) p ON true
ORDER BY e.name;
```

---

### 🔹 Task 2: Get top 2 highest scored projects for each employee

```sql
SELECT 
    e.name,
    e.department,
    p.project_name,
    p.score
FROM employees e
LEFT JOIN LATERAL (
    SELECT project_name, score
    FROM projects
    WHERE employee_id = e.id
    ORDER BY score DESC
    LIMIT 2
) p ON true
ORDER BY e.name, p.score DESC;
```

---

### 🔹 Task 3: Show employee name with their average project score (using LATERAL)

```sql
SELECT 
    e.name,
    e.department,
    p.avg_score
FROM employees e
LEFT JOIN LATERAL (
    SELECT ROUND(AVG(score), 2) AS avg_score
    FROM projects
    WHERE employee_id = e.id
) p ON true
ORDER BY p.avg_score DESC NULLS LAST;
```

---

### 🔹 Task 4: Find employees who have at least one project with score > 90

```sql
SELECT e.name, e.department
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM projects p
    WHERE p.employee_id = e.id AND p.score > 90
);
```

**Alternative using LATERAL:**

```sql
SELECT e.name, e.department, p.project_name, p.score
FROM employees e
JOIN LATERAL (
    SELECT project_name, score
    FROM projects
    WHERE employee_id = e.id AND score > 90
) p ON true;
```

---

### 🔹 Task 5: Get the latest project (highest id) for each employee

```sql
SELECT 
    e.name,
    p.project_name,
    p.score
FROM employees e
LEFT JOIN LATERAL (
    SELECT project_name, score
    FROM projects
    WHERE employee_id = e.id
    ORDER BY id DESC
    LIMIT 1
) p ON true;
```

---

## 🧠 Detailed Explanation

### Normal JOIN vs LATERAL JOIN

- A normal subquery in `FROM` is independent — it cannot see columns from other tables.
- `LATERAL` makes the subquery **correlated** — it can reference columns from preceding tables.

### When to use LATERAL?

- Top-N per group queries (very common interview pattern)
- When you want to call a function that returns a set of rows based on current row values
- When Window Functions become too complex or less readable

### Performance Note

LATERAL joins can be very efficient when combined with proper indexes, especially for Top-N queries, because PostgreSQL can stop after finding the required number of rows (`LIMIT`).

---

## 💡 Key Takeaways for Interviews

- `LATERAL` allows correlated subqueries in the `FROM` clause.
- It is one of the best ways to solve **“Top N records per group”** problems.
- Always use `LEFT JOIN LATERAL ... ON true` when you want to keep rows even if the subquery returns nothing.
- Use `JOIN LATERAL` (INNER) when you only want rows that have matching results.
- LATERAL is more readable than complex window function solutions in many cases.

---

## 🔥 Bonus Challenge

Write a query using `LATERAL` that shows:
- Employee name
- Their highest scored project
- Their second highest scored project
- The difference between the two scores
```
