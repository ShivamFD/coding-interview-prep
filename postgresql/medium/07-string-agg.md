# 🧵 PostgreSQL: STRING_AGG & Array Aggregation

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-STRING_AGG-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`STRING_AGG` is a powerful aggregate function in PostgreSQL that concatenates values from multiple rows into a single string.

It is extremely useful when you want to:
- List all employees in a department as a comma-separated string
- Create a summary of tags, skills, or categories
- Generate readable reports from one-to-many relationships

### Related Functions

| Function              | Description                                      |
|-----------------------|--------------------------------------------------|
| `STRING_AGG(expr, delimiter)` | Concatenates values with a delimiter     |
| `ARRAY_AGG(expr)`     | Aggregates values into an array                  |
| `ARRAY_TO_STRING()`   | Converts an array into a string                  |

---

## 🗄️ Dataset: `employees` Table

| id | name     | department | skill        |
|:--:|:---------|:-----------|:-------------|
| 1  | Rahul    | IT         | JavaScript   |
| 2  | Rahul    | IT         | React        |
| 3  | Priya    | HR         | Communication|
| 4  | Amit     | IT         | Node.js      |
| 5  | Amit     | IT         | PostgreSQL   |
| 6  | Sneha    | Finance    | Excel        |
| 7  | Vikram   | IT         | JavaScript   |
| 8  | Neha     | HR         | Recruitment  |
| 9  | Karan    | Finance    | Accounting   |
| 10 | Anjali   | IT         | React        |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: List all unique skills in each department

```sql
SELECT 
    department,
    STRING_AGG(DISTINCT skill, ', ' ORDER BY skill) AS skills
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 2: List all employees in each department

```sql
SELECT 
    department,
    STRING_AGG(name, ', ' ORDER BY name) AS employees
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 3: Show number of skills and the list of skills for each employee

```sql
SELECT 
    name,
    COUNT(skill) AS total_skills,
    STRING_AGG(skill, ', ' ORDER BY skill) AS skill_list
FROM employees
GROUP BY name
ORDER BY name;
```

---

### 🔹 Task 4: Using ARRAY_AGG instead of STRING_AGG

```sql
SELECT 
    department,
    ARRAY_AGG(DISTINCT skill ORDER BY skill) AS skills_array
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 5: Convert array back to string using ARRAY_TO_STRING

```sql
SELECT 
    department,
    ARRAY_TO_STRING(
        ARRAY_AGG(DISTINCT skill ORDER BY skill), 
        ' | '
    ) AS skills
FROM employees
GROUP BY department
ORDER BY department;
```

---

### 🔹 Task 6: Show employees and their skills only for IT department

```sql
SELECT 
    name,
    STRING_AGG(skill, ', ' ORDER BY skill) AS skills
FROM employees
WHERE department = 'IT'
GROUP BY name
ORDER BY name;
```

---

## 💡 Key Takeaways

- `STRING_AGG(expression, delimiter)` is the most commonly used function for this purpose.
- You can use `DISTINCT` inside `STRING_AGG` to remove duplicates.
- You can add `ORDER BY` inside `STRING_AGG` to control the order of values.
- `ARRAY_AGG` is useful when you want to keep the result as an array for further processing.
- `STRING_AGG` is one of the most frequently asked aggregate functions in PostgreSQL interviews.

---

## 🔥 Bonus Challenge

Write a query that shows:
- Department
- Total number of unique skills
- List of unique skills (comma separated)
- List of employees in that department
```
