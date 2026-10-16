# 🔗 PostgreSQL: Self JOIN

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Self%20JOIN-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

A **Self JOIN** is a regular join, but the table is joined with itself.  

It is useful when you want to compare rows within the same table.

### Common Use Cases

- Find employees and their managers (from the same table)
- Find pairs of employees in the same department
- Compare a row with other rows in the same table

---

## 🗄️ Dataset: `employees` Table

| id | name     | manager_id | department | salary |
|:--:|:---------|:----------:|:-----------|-------:|
| 1  | Rahul    | NULL       | IT         | 90000  |
| 2  | Priya    | 1          | HR         | 65000  |
| 3  | Amit     | 1          | IT         | 75000  |
| 4  | Sneha    | 2          | Finance    | 52000  |
| 5  | Vikram   | 3          | IT         | 68000  |
| 6  | Neha     | 2          | HR         | 48000  |
| 7  | Karan    | 3          | IT         | 72000  |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Show each employee with their manager’s name

```sql
SELECT 
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;
```

---

### 🔹 Task 2: Show only employees who have a manager

```sql
SELECT 
    e.name AS employee,
    m.name AS manager
FROM employees e
INNER JOIN employees m ON e.manager_id = m.id;
```

---

### 🔹 Task 3: Find employees who earn more than their manager

```sql
SELECT 
    e.name AS employee,
    e.salary AS employee_salary,
    m.name AS manager,
    m.salary AS manager_salary
FROM employees e
INNER JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

---

### 🔹 Task 4: Find pairs of employees who work in the same department

```sql
SELECT 
    e1.name AS employee1,
    e2.name AS employee2,
    e1.department
FROM employees e1
INNER JOIN employees e2 
    ON e1.department = e2.department 
   AND e1.id < e2.id;   -- avoids duplicate pairs and self-pairing
```

---

### 🔹 Task 5: Show employee name, manager name, and department

```sql
SELECT 
    e.name AS employee,
    COALESCE(m.name, 'No Manager') AS manager,
    e.department
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id
ORDER BY e.department, e.name;
```

---

## 💡 Key Takeaways

- Self JOIN means joining a table with itself.
- We must use different aliases (e.g., `e` and `m`).
- `LEFT JOIN` is useful when some rows may not have a match (e.g., top-level managers).
- Use `e1.id < e2.id` to avoid duplicate pairs when comparing rows.
- Self JOINs are very common in organizational hierarchy questions.

---

## 🔥 Bonus Challenge

Write a query to find the manager who has the highest number of direct reports.
```
