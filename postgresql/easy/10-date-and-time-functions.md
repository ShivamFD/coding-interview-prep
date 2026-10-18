# 📅 PostgreSQL: Date and Time Functions

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Date%20%26%20Time-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

PostgreSQL has powerful built-in functions to work with dates and timestamps.

These functions are very commonly used in real applications (filtering by date, calculating age, finding difference between dates, etc.).

### Most Important Date/Time Functions

| Function                    | Description                              |
|-----------------------------|------------------------------------------|
| `CURRENT_DATE`              | Returns today’s date                     |
| `CURRENT_TIMESTAMP`         | Returns current date and time            |
| `NOW()`                     | Same as CURRENT_TIMESTAMP                |
| `AGE(date)`                 | Calculates age from a date               |
| `DATE_PART(field, date)`    | Extracts a part (year, month, day...)    |
| `EXTRACT(field FROM date)`  | Same as DATE_PART                        |
| `DATE_TRUNC(field, date)`   | Truncates date to a certain precision    |
| `+` / `-` INTERVAL          | Add or subtract time intervals           |

---

## 🗄️ Dataset: `employees` Table

| id | name     | join_date  | birth_date  |
|:--:|:---------|:-----------|:------------|
| 1  | Rahul    | 2021-03-15 | 1995-06-20  |
| 2  | Priya    | 2022-07-01 | 1998-11-12  |
| 3  | Amit     | 2020-01-10 | 1992-04-05  |
| 4  | Sneha    | 2023-05-22 | 1997-09-30  |
| 5  | Vikram   | 2021-11-08 | 1994-02-14  |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Show today’s date and current timestamp

```sql
SELECT 
    CURRENT_DATE AS today,
    CURRENT_TIMESTAMP AS right_now;
```

---

### 🔹 Task 2: Calculate age of each employee

```sql
SELECT 
    name,
    birth_date,
    AGE(birth_date) AS age
FROM employees;
```

---

### 🔹 Task 3: Extract year, month, and day from join_date

```sql
SELECT 
    name,
    join_date,
    EXTRACT(YEAR FROM join_date) AS join_year,
    EXTRACT(MONTH FROM join_date) AS join_month,
    EXTRACT(DAY FROM join_date) AS join_day
FROM employees;
```

---

### 🔹 Task 4: Find employees who joined in the year 2021

```sql
SELECT name, join_date
FROM employees
WHERE EXTRACT(YEAR FROM join_date) = 2021;
```

---

### 🔹 Task 5: Find employees who joined in the last 2 years

```sql
SELECT name, join_date
FROM employees
WHERE join_date >= CURRENT_DATE - INTERVAL '2 years';
```

---

### 🔹 Task 6: Add 6 months to each employee’s join date

```sql
SELECT 
    name,
    join_date,
    join_date + INTERVAL '6 months' AS probation_end_date
FROM employees;
```

---

### 🔹 Task 7: Find the number of days each employee has been working

```sql
SELECT 
    name,
    join_date,
    CURRENT_DATE - join_date AS days_worked
FROM employees;
```

---

## 💡 Key Takeaways

- `CURRENT_DATE` gives only the date (no time).
- `AGE()` is perfect for calculating age or duration.
- `EXTRACT` / `DATE_PART` is used to pull year, month, day, etc.
- You can add or subtract intervals using `+ INTERVAL` / `- INTERVAL`.
- Date difference (`date1 - date2`) returns number of days.

---

## 🔥 Bonus Challenge

Write a query that shows:
- Employee name
- Age in years (only the year part)
- How many full years they have been working in the company
```
