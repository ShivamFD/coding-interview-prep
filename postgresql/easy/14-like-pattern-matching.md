# 🔍 PostgreSQL: LIKE & Pattern Matching

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-LIKE%20%26%20Pattern%20Matching-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

The `LIKE` operator is used for pattern matching in SQL.

### Wildcards

| Symbol | Meaning                          |
|--------|----------------------------------|
| `%`    | Matches any sequence of characters |
| `_`    | Matches exactly one character    |

### Important Points
- `LIKE` is case-sensitive in PostgreSQL
- Use `ILIKE` for case-insensitive matching
- Very useful for search functionality

---

## 🗄️ Dataset: `employees` Table

| id | name         | email                  | department |
|:--:|:-------------|:-----------------------|:-----------|
| 1  | Rahul Sharma | rahul.s@company.com    | IT         |
| 2  | Priya Patel  | priya.p@company.com    | HR         |
| 3  | Amit Kumar   | amit.k@company.com     | IT         |
| 4  | Sneha Reddy  | sneha.r@company.com    | Finance    |
| 5  | Vikram Singh | vikram.s@company.com   | IT         |
| 6  | Neha Gupta   | neha.g@company.com     | HR         |
| 7  | Karan Mehta  | karan.m@company.com    | Finance    |
| 8  | Anjali Verma | anjali.v@company.com   | IT         |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Find employees whose name starts with "A"

```sql
SELECT name, department
FROM employees
WHERE name LIKE 'A%';
```

---

### 🔹 Task 2: Find employees whose name ends with "a"

```sql
SELECT name, department
FROM employees
WHERE name LIKE '%a';
```

---

### 🔹 Task 3: Find employees whose name contains "ra"

```sql
SELECT name, department
FROM employees
WHERE name LIKE '%ra%';
```

---

### 🔹 Task 4: Find employees with exactly 5-letter first names (using _)

```sql
SELECT name
FROM employees
WHERE name LIKE '_____ %';   -- 5 characters + space + last name
```

---

### 🔹 Task 5: Case-insensitive search using ILIKE

```sql
SELECT name, email
FROM employees
WHERE email ILIKE '%COMPANY.COM%';
```

---

### 🔹 Task 6: Find employees whose email starts with "rahul" or "amit"

```sql
SELECT name, email
FROM employees
WHERE email LIKE 'rahul%' OR email LIKE 'amit%';
```

---

### 🔹 Task 7: Find names that have "h" as the second letter

```sql
SELECT name
FROM employees
WHERE name LIKE '_h%';
```

---

## 💡 Key Takeaways

- `%` → matches zero or more characters
- `_` → matches exactly one character
- `LIKE` is case-sensitive
- `ILIKE` is case-insensitive (PostgreSQL specific)
- Very commonly used in search features
- Can be combined with `AND` / `OR`

---

## 🔥 Bonus Challenge

Write a query that finds employees whose:
- Name contains "a" (case-insensitive)
- And email ends with "company.com"
```
