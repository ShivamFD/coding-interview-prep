# 🎯 PostgreSQL: HAVING Clause

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-HAVING%20Clause-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

The `HAVING` clause is used to filter **groups** after aggregation has been performed.

### Key Difference:

| Clause   | Filters          | When it runs          |
|----------|------------------|-----------------------|
| `WHERE`  | Individual rows  | Before grouping       |
| `HAVING` | Groups           | After grouping        |

You **cannot** use aggregate functions in the `WHERE` clause.  
That’s why we need `HAVING`.

---

## 🗄️ Dataset: `employees` Table

| id | name     | department | salary | joining_year |
|:--:|:---------|:-----------|-------:|:------------:|
| 1  | Rahul    | IT         | 60000  | 2021         |
| 2  | Priya    | HR         | 45000  | 2022         |
| 3  | Amit     | IT         | 75000  | 2020         |
| 4  | Sneha    | Finance    | 52000  | 2023         |
| 5  | Vikram   | IT         | 68000  | 2021         |
| 6  | Neha     | HR         | 48000  | 2022         |
| 7  | Karan    | Finance    | 81000  | 2021         |
| 8  | Anjali   | IT         | 72000  | 2022         |
| 9  | Rohan    | HR         | 51000  | 2023         |
| 10 | Meera    | Finance    | 59000  | 2020         |
| 11 | Suresh   | IT         | 64000  | 2023         |
| 12 | Kavita   | Marketing  | 47000  | 2022         |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Departments that have more than 2 employees

```sql
SELECT 
    department,
    COUNT(*) AS total_employees
FROM employees
GROUP BY department
HAVING COUNT(*) > 2
ORDER BY total_employees DESC;
```

---

### 🔹 Task 2: Departments where average salary is greater than 60000

```sql
SELECT 
    department,
    ROUND(AVG(salary), 2) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 60000
ORDER BY avg_salary DESC;
```

---

### 🔹 Task 3: Departments where total salary is more than 200000

```sql
SELECT 
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department
HAVING SUM(salary) > 200000
ORDER BY total_salary DESC;
```

---

### 🔹 Task 4: Departments that have at least one employee with salary > 70000

```sql
SELECT 
    department,
    COUNT(*) AS total_employees,
    MAX(salary) AS highest_salary
FROM employees
GROUP BY department
HAVING MAX(salary) > 70000;
```

---

### 🔹 Task 5: Combine WHERE + GROUP BY + HAVING

Find departments where:
- Employees joined after 2020
- And the department has more than 2 such employees

```sql
SELECT 
    department,
    COUNT(*) AS employees_after_2020
FROM employees
WHERE joining_year > 2020
GROUP BY department
HAVING COUNT(*) > 2
ORDER BY employees_after_2020 DESC;
```

---

### 🔹 Task 6: Departments whose average salary is higher than the overall company average

```sql
SELECT 
    department,
    ROUND(AVG(salary), 2) AS dept_avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > (SELECT AVG(salary) FROM employees)
ORDER BY dept_avg_salary DESC;
```

---

## 💡 Key Takeaways

- `WHERE` filters rows **before** aggregation.
- `HAVING` filters groups **after** aggregation.
- You can use aggregate functions only in `HAVING` (not in `WHERE`).
- You can combine `WHERE` + `GROUP BY` + `HAVING` in the same query.
- `HAVING` can also contain subqueries.

---

## 🔥 Bonus Challenge

Write a query that shows departments which:
- Have more than 2 employees
- Have average salary greater than 55000
- And the highest salary in that department is at least 70000
```
