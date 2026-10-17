# 🔍 PostgreSQL: EXISTS and NOT EXISTS

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-EXISTS%20%2F%20NOT%20EXISTS-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`EXISTS` and `NOT EXISTS` are used to check whether a subquery returns any rows.

- `EXISTS` → returns `TRUE` if the subquery returns at least one row
- `NOT EXISTS` → returns `TRUE` if the subquery returns zero rows

They are very efficient and commonly used in interviews.

### Why use EXISTS instead of IN?

- `EXISTS` stops as soon as it finds the first matching row (faster)
- Handles `NULL` values better than `IN`
- More readable for correlated subqueries

---

## 🗄️ Datasets

**employees**

| id | name     | department_id |
|:--:|:---------|:-------------:|
| 1  | Rahul    | 101           |
| 2  | Priya    | 102           |
| 3  | Amit     | 101           |
| 4  | Sneha    | 103           |
| 5  | Vikram   | 101           |
| 6  | Neha     | 102           |

**departments**

| id  | department_name |
|:---:|:----------------|
| 101 | IT              |
| 102 | HR              |
| 103 | Finance         |
| 104 | Marketing       |

**projects**

| id | project_name     | employee_id |
|:--:|:-----------------|:-----------:|
| 1  | Website Redesign | 1           |
| 2  | Mobile App       | 3           |
| 3  | HR Portal        | 2           |
| 4  | API Development  | 1           |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Find employees who are assigned to at least one project

```sql
SELECT name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM projects p
    WHERE p.employee_id = e.id
);
```

---

### 🔹 Task 2: Find employees who are NOT assigned to any project

```sql
SELECT name
FROM employees e
WHERE NOT EXISTS (
    SELECT 1
    FROM projects p
    WHERE p.employee_id = e.id
);
```

---

### 🔹 Task 3: Find departments that have at least one employee

```sql
SELECT department_name
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.id
);
```

---

### 🔹 Task 4: Find departments that have NO employees

```sql
SELECT department_name
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.id
);
```

---

### 🔹 Task 5: Find employees who work in a department that has more than 2 employees (using EXISTS)

```sql
SELECT name
FROM employees e1
WHERE EXISTS (
    SELECT 1
    FROM employees e2
    WHERE e2.department_id = e1.department_id
    GROUP BY e2.department_id
    HAVING COUNT(*) > 2
);
```

---

## 💡 Key Takeaways

- `EXISTS` only cares whether any row is returned, not what the row contains.
- Writing `SELECT 1` inside EXISTS is a common practice (we don’t need actual data).
- `NOT EXISTS` is the preferred way to find missing related records.
- Correlated subqueries with EXISTS are very powerful and frequently asked in interviews.
- `EXISTS` is usually faster than `IN` for large datasets.

---

## 🔥 Bonus Challenge

Write a query using `NOT EXISTS` to find all employees who do not belong to the IT department and are also not assigned to any project.
```
