# 🎯 PostgreSQL: BETWEEN & IN Operators

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-BETWEEN%20%26%20IN-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`BETWEEN` and `IN` are two very commonly used operators for filtering data.

| Operator     | Purpose                                      |
|--------------|----------------------------------------------|
| `BETWEEN`    | Checks if a value is within a range (inclusive) |
| `IN`         | Checks if a value matches any value in a list |
| `NOT BETWEEN`| Opposite of BETWEEN                          |
| `NOT IN`     | Opposite of IN                               |

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
| 9  | Rohan    | Marketing  | 51000  | 2023         |
| 10 | Meera    | Finance    | 59000  | 2020         |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Employees with salary between 50000 and 70000

```sql
SELECT name, department, salary
FROM employees
WHERE salary BETWEEN 50000 AND 70000
ORDER BY salary;
```

---

### 🔹 Task 2: Employees who joined between 2021 and 2022

```sql
SELECT name, department, joining_year
FROM employees
WHERE joining_year BETWEEN 2021 AND 2022
ORDER BY joining_year;
```

---

### 🔹 Task 3: Employees who work in IT or HR

```sql
SELECT name, department, salary
FROM employees
WHERE department IN ('IT', 'HR')
ORDER BY department, name;
```

---

### 🔹 Task 4: Employees who do NOT work in Finance or Marketing

```sql
SELECT name, department, salary
FROM employees
WHERE department NOT IN ('Finance', 'Marketing')
ORDER BY department;
```

---

### 🔹 Task 5: Employees with salary NOT between 50000 and 70000

```sql
SELECT name, department, salary
FROM employees
WHERE salary NOT BETWEEN 50000 AND 70000
ORDER BY salary;
```

---

### 🔹 Task 6: Combine BETWEEN and IN

```sql
SELECT name, department, salary, joining_year
FROM employees
WHERE department IN ('IT', 'Finance')
  AND salary BETWEEN 60000 AND 80000
ORDER BY salary DESC;
```

---

### 🔹 Task 7: Employees who joined in specific years

```sql
SELECT name, joining_year
FROM employees
WHERE joining_year IN (2020, 2023)
ORDER BY joining_year, name;
```

---

## 💡 Key Takeaways

- `BETWEEN` is inclusive (includes both boundary values).
- `BETWEEN a AND b` is the same as `value >= a AND value <= b`.
- `IN` is cleaner than writing multiple `OR` conditions.
- `NOT IN` and `NOT BETWEEN` are useful for exclusion.
- These operators are extremely common in real-world filtering.

---

## 🔥 Bonus Challenge

Write a query that finds employees who:
- Work in IT or Finance
- Have salary between 55000 and 75000
- And joined after 2020
```
