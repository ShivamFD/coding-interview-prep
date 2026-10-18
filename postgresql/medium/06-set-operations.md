# 🔀 PostgreSQL: Set Operations (UNION, INTERSECT, EXCEPT)

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Set%20Operations-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Set operations allow you to combine the results of two or more `SELECT` queries.

| Operator       | Description                                      | Removes Duplicates? |
|----------------|--------------------------------------------------|---------------------|
| `UNION`        | Combines results from both queries               | Yes                 |
| `UNION ALL`    | Combines results from both queries               | No                  |
| `INTERSECT`    | Returns only common rows                         | Yes                 |
| `EXCEPT`       | Returns rows from first query that are not in second | Yes             |

**Important Rules:**
- Both queries must return the same number of columns
- Corresponding columns must have compatible data types
- Column names are taken from the first query

---

## 🗄️ Datasets

**developers**

| id | name     | city     |
|:--:|:---------|:---------|
| 1  | Rahul    | Delhi    |
| 2  | Priya    | Mumbai   |
| 3  | Amit     | Delhi    |
| 4  | Sneha    | Bangalore|

**designers**

| id | name     | city     |
|:--:|:---------|:---------|
| 1  | Vikram   | Delhi    |
| 2  | Neha     | Mumbai   |
| 3  | Karan    | Pune     |
| 4  | Anjali   | Delhi    |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Get all unique names from both tables (UNION)

```sql
SELECT name FROM developers
UNION
SELECT name FROM designers;
```

---

### 🔹 Task 2: Get all names including duplicates (UNION ALL)

```sql
SELECT name FROM developers
UNION ALL
SELECT name FROM designers;
```

---

### 🔹 Task 3: Find common cities between developers and designers (INTERSECT)

```sql
SELECT city FROM developers
INTERSECT
SELECT city FROM designers;
```

---

### 🔹 Task 4: Find cities where only developers live (EXCEPT)

```sql
SELECT city FROM developers
EXCEPT
SELECT city FROM designers;
```

---

### 🔹 Task 5: Find cities where only designers live

```sql
SELECT city FROM designers
EXCEPT
SELECT city FROM developers;
```

---

### 🔹 Task 6: Combine name and city from both tables with a label

```sql
SELECT name, city, 'Developer' AS role
FROM developers

UNION ALL

SELECT name, city, 'Designer' AS role
FROM designers

ORDER BY role, name;
```

---

## 💡 Key Takeaways

- `UNION` removes duplicates, `UNION ALL` keeps them (faster).
- Prefer `UNION ALL` when you are sure there are no duplicates or when you want to keep them.
- `INTERSECT` returns only the common rows.
- `EXCEPT` is very useful for finding differences between two result sets.
- You can add extra columns (like a label) to distinguish the source of data.
- `ORDER BY` should be placed only at the end of the final query.

---

## 🔥 Bonus Challenge

Write a query that shows all unique cities from both tables and also displays how many people (developers + designers) live in each city.
```
