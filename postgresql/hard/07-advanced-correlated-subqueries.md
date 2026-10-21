# 🧠 Advanced PostgreSQL: Correlated Subqueries

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Correlated%20Subqueries-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

A **Correlated Subquery** is a subquery that depends on the outer query.  
It is executed once for every row processed by the outer query.

### Difference between Normal and Correlated Subquery

| Type                | Executes                          | Depends on outer query? |
|---------------------|-----------------------------------|-------------------------|
| Normal Subquery     | Only once                         | No                      |
| Correlated Subquery | Once for every outer row          | Yes                     |

Correlated subqueries are powerful but can be slower on large datasets.  
They are still very commonly asked in interviews.

---

## 🗄️ Dataset: `employees` Table

| id | name     | department | salary | manager_id |
|:--:|:---------|:-----------|-------:|:----------:|
| 1  | Rahul    | IT         | 90000  | NULL       |
| 2  | Priya    | HR         | 65000  | 1          |
| 3  | Amit     | IT         | 75000  | 1          |
| 4  | Sneha    | Finance    | 52000  | 2          |
| 5  | Vikram   | IT         | 68000  | 3          |
| 6  | Neha     | HR         | 48000  | 2          |
| 7  | Karan    | Finance    | 81000  | 1          |
| 8  | Anjali   | IT         | 72000  | 3          |
| 9  | Rohan    | HR         | 55000  | 2          |
| 10 | Meera    | Finance    | 59000  | 7          |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Find employees who earn more than the average salary of their department

```sql
SELECT 
    name,
    department,
    salary
FROM employees e1
WHERE salary > (
    SELECT AVG(salary)
    FROM employees e2
    WHERE e2.department = e1.department
)
ORDER BY department, salary DESC;
```

---

### 🔹 Task 2: Find employees who earn more than their manager

```sql
SELECT 
    e.name AS employee,
    e.salary AS employee_salary,
    m.name AS manager,
    m.salary AS manager_salary
FROM employees e
JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

**Using Correlated Subquery version:**

```sql
SELECT 
    name,
    salary,
    department
FROM employees e
WHERE salary > (
    SELECT salary
    FROM employees
    WHERE id = e.manager_id
);
```

---

### 🔹 Task 3: Find the highest paid employee in each department (using correlated subquery)

```sql
SELECT 
    name,
    department,
    salary
FROM employees e1
WHERE salary = (
    SELECT MAX(salary)
    FROM employees e2
    WHERE e2.department = e1.department
)
ORDER BY department;
```

---

### 🔹 Task 4: Find employees who have the same salary as someone in another department

```sql
SELECT 
    name,
    department,
    salary
FROM employees e1
WHERE EXISTS (
    SELECT 1
    FROM employees e2
    WHERE e2.salary = e1.salary
      AND e2.department <> e1.department
);
```

---

### 🔹 Task 5: Count how many employees earn less than the current employee (in the same department)

```sql
SELECT 
    name,
    department,
    salary,
    (
        SELECT COUNT(*)
        FROM employees e2
        WHERE e2.department = e1.department
          AND e2.salary < e1.salary
    ) AS people_earning_less
FROM employees e1
ORDER BY department, salary DESC;
```

---

### 🔹 Task 6: Find departments where the average salary is higher than the overall company average

```sql
SELECT 
    department,
    ROUND(AVG(salary), 2) AS dept_avg
FROM employees
GROUP BY department
HAVING AVG(salary) > (
    SELECT AVG(salary) FROM employees
);
```

---

## 🧠 Detailed Explanation

### How a Correlated Subquery works:

1. The outer query picks one row.
2. The inner query runs using values from that specific row.
3. The result of the inner query is used to decide whether to keep the outer row.
4. This process repeats for every row.

### Performance Note:
Correlated subqueries can be slow because they run many times.  
In many cases they can be rewritten using `JOIN` or Window Functions for better performance.

---

## 💡 Key Takeaways for Interviews

- Correlated subqueries depend on the outer query.
- They are very useful for “compared to its own group” type questions.
- `EXISTS` + correlated subquery is a classic pattern.
- Always think whether the same logic can be written with `JOIN` or Window Functions.
- Interviewers love asking the difference between correlated and non-correlated subqueries.

---

## 🔥 Bonus Challenge

Write a query that finds employees who earn more than **at least 2 other employees** in their own department using a correlated subquery.
```
