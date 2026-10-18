# 📊 Advanced PostgreSQL: Conditional Aggregation (Pivoting Data)

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Conditional%20Aggregation-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

**Conditional Aggregation** is a powerful technique where we use aggregate functions (`COUNT`, `SUM`, `AVG`, etc.) together with `CASE` statements to transform rows into columns.

This is the standard way to create **Pivot Tables** in pure SQL (without using the `crosstab` extension).

### Why is this important?

In real-world reporting, data is usually stored in a normalized (long) format, but we often need to display it in a wide format (like Excel pivot tables).

**Example of what we want to achieve:**

Instead of this (normalized data):

| department | gender | count |
|------------|--------|-------|
| IT         | Male   | 5     |
| IT         | Female | 3     |
| HR         | Male   | 2     |
| HR         | Female | 4     |

We want this (pivoted result):

| department | male_count | female_count | total |
|------------|------------|--------------|-------|
| IT         | 5          | 3            | 8     |
| HR         | 2          | 4            | 6     |

This technique is extremely common in interviews and real projects.

---

## 🧠 How Conditional Aggregation Works

We use this pattern:

```sql
COUNT(CASE WHEN condition THEN 1 END)
SUM(CASE WHEN condition THEN value END)
AVG(CASE WHEN condition THEN value END)
```

- The `CASE` statement returns a value only when the condition is true.
- When the condition is false, it returns `NULL`.
- Aggregate functions ignore `NULL` values.
- This allows us to count/sum/average only specific groups inside one query.

---

## 🗄️ Dataset: `employees` Table

| id | name     | department | gender | salary | joining_year |
|:--:|:---------|:-----------|:-------|-------:|:------------:|
| 1  | Rahul    | IT         | Male   | 60000  | 2021         |
| 2  | Priya    | HR         | Female | 45000  | 2022         |
| 3  | Amit     | IT         | Male   | 75000  | 2020         |
| 4  | Sneha    | Finance    | Female | 52000  | 2023         |
| 5  | Vikram   | IT         | Male   | 68000  | 2021         |
| 6  | Neha     | HR         | Female | 48000  | 2022         |
| 7  | Karan    | Finance    | Male   | 81000  | 2021         |
| 8  | Anjali   | IT         | Female | 72000  | 2022         |
| 9  | Rohan    | HR         | Male   | 51000  | 2023         |
| 10 | Meera    | Finance    | Female | 59000  | 2020         |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Count of Male and Female employees in each department

```sql
SELECT 
    department,
    COUNT(CASE WHEN gender = 'Male' THEN 1 END) AS male_count,
    COUNT(CASE WHEN gender = 'Female' THEN 1 END) AS female_count,
    COUNT(*) AS total_employees
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 2: Total salary of Male and Female employees in each department

```sql
SELECT 
    department,
    SUM(CASE WHEN gender = 'Male' THEN salary ELSE 0 END) AS male_salary,
    SUM(CASE WHEN gender = 'Female' THEN salary ELSE 0 END) AS female_salary,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 3: Average salary by gender in each department

```sql
SELECT 
    department,
    ROUND(AVG(CASE WHEN gender = 'Male' THEN salary END), 2) AS male_avg_salary,
    ROUND(AVG(CASE WHEN gender = 'Female' THEN salary END), 2) AS female_avg_salary
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 4: Count of employees who joined in different years (Pivot by Year)

```sql
SELECT 
    department,
    COUNT(CASE WHEN joining_year = 2020 THEN 1 END) AS joined_2020,
    COUNT(CASE WHEN joining_year = 2021 THEN 1 END) AS joined_2021,
    COUNT(CASE WHEN joining_year = 2022 THEN 1 END) AS joined_2022,
    COUNT(CASE WHEN joining_year = 2023 THEN 1 END) AS joined_2023
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 5: Create salary range categories for each department

```sql
SELECT 
    department,
    COUNT(CASE WHEN salary < 50000 THEN 1 END) AS low_salary,
    COUNT(CASE WHEN salary BETWEEN 50000 AND 70000 THEN 1 END) AS medium_salary,
    COUNT(CASE WHEN salary > 70000 THEN 1 END) AS high_salary
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 6: Percentage of Male employees in each department

```sql
SELECT 
    department,
    COUNT(*) AS total,
    COUNT(CASE WHEN gender = 'Male' THEN 1 END) AS male_count,
    ROUND(
        COUNT(CASE WHEN gender = 'Male' THEN 1 END) * 100.0 / COUNT(*), 
        2
    ) AS male_percentage
FROM employees
GROUP BY department
ORDER BY department;
```

---

## 🧠 Detailed Explanation

### Why do we use `CASE` inside aggregate functions?

Because we want to **conditionally include** rows in the aggregation.

Example:

```sql
COUNT(CASE WHEN gender = 'Male' THEN 1 END)
```

- When `gender = 'Male'` → returns `1` → counted
- When `gender = 'Female'` → returns `NULL` → ignored by `COUNT`

This is much cleaner and faster than writing multiple subqueries.

### Difference between `COUNT(CASE ...)` and `SUM(CASE ...)`

- Use `COUNT(CASE WHEN ... THEN 1 END)` when you want to count rows.
- Use `SUM(CASE WHEN ... THEN value ELSE 0 END)` when you want to sum a numeric column.

---

## 💡 Key Takeaways for Interviews

- This is the standard way to pivot data in PostgreSQL.
- Always use `ELSE 0` with `SUM` to avoid `NULL` results.
- `COUNT` automatically ignores `NULL`, so `ELSE` is optional with `COUNT`.
- You can pivot by any categorical column (gender, year, status, etc.).
- Conditional aggregation is usually preferred over multiple self-joins or subqueries for this use case.
- Very common in reporting dashboards and business intelligence questions.

---

## 🔥 Bonus Challenge

Write a single query that shows for each department:

- Total employees
- Male count & Female count
- Male total salary & Female total salary
- Male average salary & Female average salary
- Percentage of high salary employees (salary > 70000)
```
