# 🌳 Advanced PostgreSQL: Recursive CTE (Hierarchy + Path)

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Advanced%20Recursive%20CTE-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Recursive CTEs are used to query hierarchical or graph-structured data.

In this advanced problem, we will:

- Build a complete organization hierarchy
- Generate the full path from root to each employee
- Calculate the level/depth of each employee
- Count the number of subordinates

This is a very common hard-level SQL interview question.

---

## 🗄️ Dataset: `employees` Table

| id | name     | manager_id |
|:--:|:---------|:----------:|
| 1  | Rahul    | NULL       |
| 2  | Priya    | 1          |
| 3  | Amit     | 1          |
| 4  | Sneha    | 2          |
| 5  | Vikram   | 2          |
| 6  | Neha     | 3          |
| 7  | Karan    | 3          |
| 8  | Anjali   | 4          |
| 9  | Rohan    | 4          |
| 10 | Meera    | 6          |

**Hierarchy Visualization:**

```
Rahul
├── Priya
│   ├── Sneha
│   │   ├── Anjali
│   │   └── Rohan
│   └── Vikram
└── Amit
    ├── Neha
    │   └── Meera
    └── Karan
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Get the complete hierarchy with level

```sql
WITH RECURSIVE hierarchy AS (
    -- Base case: top-level employees (no manager)
    SELECT 
        id,
        name,
        manager_id,
        1 AS level
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    -- Recursive case
    SELECT 
        e.id,
        e.name,
        e.manager_id,
        h.level + 1
    FROM employees e
    INNER JOIN hierarchy h ON e.manager_id = h.id
)
SELECT * FROM hierarchy
ORDER BY level, name;
```

---

### 🔹 Task 2: Generate the full path from root to each employee

```sql
WITH RECURSIVE hierarchy AS (
    SELECT 
        id,
        name,
        manager_id,
        1 AS level,
        name::TEXT AS path
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    SELECT 
        e.id,
        e.name,
        e.manager_id,
        h.level + 1,
        h.path || ' → ' || e.name
    FROM employees e
    INNER JOIN hierarchy h ON e.manager_id = h.id
)
SELECT 
    id,
    name,
    level,
    path
FROM hierarchy
ORDER BY path;
```

---

### 🔹 Task 3: Count the number of direct and total subordinates

```sql
WITH RECURSIVE hierarchy AS (
    SELECT 
        id,
        name,
        manager_id,
        id AS root_id
    FROM employees

    UNION ALL

    SELECT 
        e.id,
        e.name,
        e.manager_id,
        h.root_id
    FROM employees e
    INNER JOIN hierarchy h ON e.manager_id = h.id
)
SELECT 
    e.id,
    e.name,
    COUNT(h.id) FILTER (WHERE h.id != e.id) AS total_subordinates
FROM employees e
LEFT JOIN hierarchy h ON e.id = h.root_id
GROUP BY e.id, e.name
ORDER BY total_subordinates DESC;
```

---

### 🔹 Task 4: Find all employees under a specific manager (e.g., under Priya)

```sql
WITH RECURSIVE subordinates AS (
    SELECT id, name, manager_id
    FROM employees
    WHERE name = 'Priya'

    UNION ALL

    SELECT e.id, e.name, e.manager_id
    FROM employees e
    INNER JOIN subordinates s ON e.manager_id = s.id
)
SELECT * FROM subordinates
WHERE name != 'Priya';
```

---

### 🔹 Task 5: Find the management chain of a specific employee (e.g., Meera)

```sql
WITH RECURSIVE chain AS (
    SELECT id, name, manager_id
    FROM employees
    WHERE name = 'Meera'

    UNION ALL

    SELECT e.id, e.name, e.manager_id
    FROM employees e
    INNER JOIN chain c ON e.id = c.manager_id
)
SELECT * FROM chain;
```

---

## 🧠 Detailed Explanation

### Structure of a Recursive CTE:

1. **Base Case (Anchor Member)**  
   Selects the starting point (usually root nodes).

2. **Recursive Member**  
   Joins the CTE with the original table to find the next level.

3. **Termination**  
   Recursion stops when the recursive member returns no more rows.

### Why Path is useful:
- Easy to understand the full hierarchy at a glance
- Can be used for breadcrumbs in applications
- Helpful for authorization (checking if someone is in the hierarchy)

---

## 💡 Key Takeaways for Interviews

- Recursive CTEs are the standard way to handle hierarchical data in PostgreSQL.
- Always define a clear base case.
- Be careful with infinite recursion (add depth limit if needed).
- Generating a path is a very common follow-up question.
- This pattern is widely used for org charts, category trees, nested comments, etc.

---

## 🔥 Bonus Challenge

Write a query that returns every employee with:
- Their level in the hierarchy
- Full path from CEO to themselves
- Number of direct reports
- Total number of subordinates under them
```
