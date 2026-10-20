# 🎯 PostgreSQL: FILTER Clause

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-FILTER%20Clause-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

The `FILTER` clause is a clean and readable way to apply conditions directly inside aggregate functions.

Instead of writing:

```sql
COUNT(CASE WHEN condition THEN 1 END)
```

You can write:

```sql
COUNT(*) FILTER (WHERE condition)
```

It makes the query much more readable and is pure PostgreSQL syntax.

### Benefits of FILTER
- Cleaner syntax than CASE
- Better readability
- Can be used with any aggregate function (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`)

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
    COUNT(*) FILTER (WHERE gender = 'Male') AS male_count,
    COUNT(*) FILTER (WHERE gender = 'Female') AS female_count,
    COUNT(*) AS total
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 2: Total salary of Male and Female employees per department

```sql
SELECT 
    department,
    SUM(salary) FILTER (WHERE gender = 'Male') AS male_salary,
    SUM(salary) FILTER (WHERE gender = 'Female') AS female_salary,
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
    ROUND(AVG(salary) FILTER (WHERE gender = 'Male'), 2) AS male_avg,
    ROUND(AVG(salary) FILTER (WHERE gender = 'Female'), 2) AS female_avg
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 4: Count of employees who joined in different years

```sql
SELECT 
    department,
    COUNT(*) FILTER (WHERE joining_year = 2020) AS joined_2020,
    COUNT(*) FILTER (WHERE joining_year = 2021) AS joined_2021,
    COUNT(*) FILTER (WHERE joining_year = 2022) AS joined_2022,
    COUNT(*) FILTER (WHERE joining_year = 2023) AS joined_2023
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 5: High salary vs Low salary count

```sql
SELECT 
    department,
    COUNT(*) FILTER (WHERE salary >= 70000) AS high_salary,
    COUNT(*) FILTER (WHERE salary < 70000) AS low_salary
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 6: Percentage of high salary employees

```sql
SELECT 
    department,
    COUNT(*) AS total,
    COUNT(*) FILTER (WHERE salary >= 70000) AS high_salary_count,
    ROUND(
        COUNT(*) FILTER (WHERE salary >= 70000) * 100.0 / COUNT(*),
        2
    ) AS high_salary_percentage
FROM employees
GROUP BY department
ORDER BY department;
```

---

## 💡 Key Takeaways

- `FILTER (WHERE condition)` is cleaner than `CASE WHEN`.
- It works with all aggregate functions.
- It is pure PostgreSQL (not available in MySQL).
- Very useful for writing readable pivot-style queries.
- Preferred way in modern PostgreSQL code.

---

## 🔥 Bonus Challenge

Write a query using `FILTER` that shows for each department:
- Total employees
- Male count & Female count
- Average male salary & Average female salary
- Number of employees who joined after 2021
```
