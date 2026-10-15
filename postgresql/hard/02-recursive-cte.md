# 🔁 Advanced PostgreSQL: Recursive CTEs

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Recursive%20CTE-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

A **Recursive CTE** is a special type of Common Table Expression that can reference itself.  
It is mainly used to query **hierarchical** or **tree-structured** data.

Common real-world use cases:
- Employee → Manager hierarchy
- Category → Subcategory tree
- Bill of Materials (BOM)
- Graph traversal
- Generating sequences

### Structure of a Recursive CTE

```sql
WITH RECURSIVE cte_name AS (
    -- 1. Anchor Member (starting point)
    SELECT ...
    
    UNION ALL
    
    -- 2. Recursive Member (references the CTE itself)
    SELECT ...
    FROM cte_name
    JOIN ...
)
SELECT * FROM cte_name;
```

---

## 🗄️ Dataset: `employees` Table (Hierarchy)

| id | name     | manager_id | department | salary |
|:--:|:---------|:----------:|:-----------|-------:|
| 1  | Rahul    | NULL       | IT         | 90000  |
| 2  | Priya    | 1          | HR         | 65000  |
| 3  | Amit     | 1          | IT         | 75000  |
| 4  | Sneha    | 2          | Finance    | 52000  |
| 5  | Vikram   | 3          | IT         | 68000  |
| 6  | Neha     | 2          | HR         | 48000  |
| 7  | Karan    | 3          | IT         | 72000  |
| 8  | Anjali   | 5          | IT         | 61000  |

> Rahul is the top-level manager (CEO).  
> Priya and Amit report to Rahul.  
> And so on...

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Show the complete employee hierarchy (all levels)

```sql
WITH RECURSIVE emp_hierarchy AS (
    -- Anchor: Top-level employees (no manager)
    SELECT 
        id,
        name,
        manager_id,
        department,
        salary,
        1 AS level,
        name AS path
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    -- Recursive: Employees who report to someone already in the hierarchy
    SELECT 
        e.id,
        e.name,
        e.manager_id,
        e.department,
        e.salary,
        eh.level + 1,
        eh.path || ' → ' || e.name
    FROM employees e
    INNER JOIN emp_hierarchy eh ON e.manager_id = eh.id
)
SELECT 
    id,
    name,
    level,
    path,
    department,
    salary
FROM emp_hierarchy
ORDER BY path;
```

---

### 🔹 Task 2: Find all subordinates under a specific manager (e.g. under Amit)

```sql
WITH RECURSIVE subordinates AS (
    -- Anchor: Start with Amit
    SELECT id, name, manager_id, department, salary
    FROM employees
    WHERE name = 'Amit'

    UNION ALL

    -- Recursive: Find people who report to anyone in the current result
    SELECT e.id, e.name, e.manager_id, e.department, e.salary
    FROM employees e
    INNER JOIN subordinates s ON e.manager_id = s.id
)
SELECT * FROM subordinates
WHERE name != 'Amit';   -- Exclude the manager himself
```

---

### 🔹 Task 3: Count total number of people under each manager (direct + indirect)

```sql
WITH RECURSIVE hierarchy AS (
    SELECT id, name, manager_id, 0 AS depth
    FROM employees

    UNION ALL

    SELECT e.id, e.name, h.manager_id, h.depth + 1
    FROM employees e
    INNER JOIN hierarchy h ON e.manager_id = h.id
)
SELECT 
    m.name AS manager,
    COUNT(h.id) AS total_subordinates
FROM hierarchy h
JOIN employees m ON h.manager_id = m.id
GROUP BY m.name
ORDER BY total_subordinates DESC;
```

---

### 🔹 Task 4: Find the management chain of a specific employee (e.g. Anjali)

```sql
WITH RECURSIVE management_chain AS (
    -- Anchor: Start with Anjali
    SELECT id, name, manager_id, 1 AS level
    FROM employees
    WHERE name = 'Anjali'

    UNION ALL

    -- Go upwards to the manager
    SELECT e.id, e.name, e.manager_id, mc.level + 1
    FROM employees e
    INNER JOIN management_chain mc ON e.id = mc.manager_id
)
SELECT name, level
FROM management_chain
ORDER BY level;
```

---

## 💡 Key Takeaways for Interviews

- Recursive CTEs have two parts: **Anchor Member** + **Recursive Member**.
- Always use `UNION ALL` (not `UNION`) for better performance.
- Be careful of infinite loops — make sure the recursion eventually stops.
- Very useful for any hierarchical / tree / graph data.
- `level` or `depth` column is commonly added to track hierarchy depth.
- Path concatenation (`||`) is a popular way to show the full chain.

---

## 🔥 Bonus Challenge

Write a recursive query that shows the full hierarchy and also calculates the **total salary cost** under each manager (including their own salary + all subordinates).
```
