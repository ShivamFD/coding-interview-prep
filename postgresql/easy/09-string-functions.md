# 🔤 PostgreSQL: String Functions

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-String%20Functions-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

PostgreSQL provides many useful built-in functions to work with text (strings).

These are very commonly used in real projects and interviews.

### Most Important String Functions

| Function              | Description                          |
|-----------------------|--------------------------------------|
| `UPPER(str)`          | Converts to uppercase                |
| `LOWER(str)`          | Converts to lowercase                |
| `LENGTH(str)`         | Returns number of characters         |
| `CONCAT(a, b, ...)`   | Joins strings together               |
| `SUBSTRING(str, s, l)`| Extracts part of a string            |
| `TRIM(str)`           | Removes spaces from both sides       |
| `REPLACE(str, a, b)`  | Replaces occurrences of a with b     |

---

## 🗄️ Dataset: `employees` Table

| id | name       | email                  | department |
|:--:|:-----------|:-----------------------|:-----------|
| 1  | Rahul      | rahul@company.com      | IT         |
| 2  | Priya      | PRIYA@company.com      | HR         |
| 3  | Amit Kumar | amit.kumar@company.com | IT         |
| 4  | Sneha      | sneha@company.com      | Finance    |
| 5  | Vikram     |  vikram@company.com    | IT         |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Convert all names to uppercase

```sql
SELECT id, UPPER(name) AS name_upper
FROM employees;
```

---

### 🔹 Task 2: Convert all emails to lowercase

```sql
SELECT id, LOWER(email) AS email_lower
FROM employees;
```

---

### 🔹 Task 3: Find the length of each employee’s name

```sql
SELECT name, LENGTH(name) AS name_length
FROM employees;
```

---

### 🔹 Task 4: Create a full display name using CONCAT

```sql
SELECT 
    CONCAT(name, ' (', department, ')') AS display_name
FROM employees;
```

---

### 🔹 Task 5: Extract the username part from email (before @)

```sql
SELECT 
    name,
    email,
    SUBSTRING(email FROM 1 FOR POSITION('@' IN email) - 1) AS username
FROM employees;
```

---

### 🔹 Task 6: Remove extra spaces from names using TRIM

```sql
SELECT id, TRIM(name) AS clean_name
FROM employees;
```

---

### 🔹 Task 7: Replace “company.com” with “newcompany.com” in emails

```sql
SELECT 
    name,
    REPLACE(email, 'company.com', 'newcompany.com') AS updated_email
FROM employees;
```

---

## 💡 Key Takeaways

- `UPPER` / `LOWER` are case conversion functions.
- `LENGTH` counts characters (not bytes).
- `CONCAT` can take multiple arguments.
- `SUBSTRING` + `POSITION` is commonly used to extract parts of a string.
- `TRIM` removes leading and trailing spaces.
- `REPLACE` is useful for data cleaning.

---

## 🔥 Bonus Challenge

Write a query that shows:
- Original name
- Name in uppercase
- Length of the name
- Email username (part before @)
```
