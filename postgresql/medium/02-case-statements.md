# CASE Statements in PostgreSQL

**Difficulty:** Medium  
**Topic:** CASE, Conditional Logic  
**Tags:** #postgresql #sql #case #conditional #interview

## Overview

The `CASE` statement is used to add **conditional logic** inside SQL queries.  
It works like `if-else` or `switch` statements in programming languages.

There are two forms:
1. **Simple CASE**
2. **Searched CASE** (most commonly used)

## Why do we use it?

We use `CASE` when we want to:
- Create new categories based on existing data
- Replace values conditionally
- Perform different calculations based on conditions
- Make queries more readable and powerful

## How it works

```sql
CASE
  WHEN condition1 THEN result1
  WHEN condition2 THEN result2
  ELSE default_result
END
```

## 📝 Problem Statement

We have the employees table:

### 🗄️ Input Data: `employees` Table

| id | name | department | salary |
| :---: | :--- | :--- | :---: |
| **1** | Rahul | IT | 60000 |
| **2** | Priya | HR | 45000 |
| **3** | Amit | IT | 75000 |
| **4** | Sneha | Finance | 52000 |
| **5** | Vikram | IT | 68000 |
| **6** | Neha | HR | 48000 |
| **7** | Karan | Finance | 81000 |

---

Write SQL queries for the following:

1.

Create a new column salary_level with these rules:

salary < 50000 → 'Low'

salary between 50000 and 70000 → 'Medium'

salary > 70000 → 'High'

2. Show employee name, department, and a new column bonus where:

• IT department → 10% of salary

• HR department → 7% of salary

Finance department → 12% of salary

3. Show employee name and a message:

If salary ≥ 70000 → 'Eligible for Promotion'

Otherwise → 'Not Eligible'

4. Count how many employees fall into each salary level (Low / Medium / High).

Solution

-- 1. Create salary_level
```sql
SELECT 
  name,
  salary,
  CASE
    WHEN salary < 50000 THEN 'Low'
    WHEN salary BETWEEN 50000 AND 70000 THEN 'Medium'
    ELSE 'High'
  END AS salary_level
FROM employees;
```
-- 2. Calculate bonus based on department
```sql
SELECT 
  name,
  department,
  salary,
  CASE department
    WHEN 'IT' THEN salary * 0.10
    WHEN 'HR' THEN salary * 0.07
    WHEN 'Finance' THEN salary * 0.12
    ELSE 0
  END AS bonus
FROM employees;
```
-- 3. Promotion eligibility
```sql
SELECT 
  name,
  salary,
  CASE
    WHEN salary >= 70000 THEN 'Eligible for Promotion'
    ELSE 'Not Eligible'
  END AS promotion_status
FROM employees;
```
-- 4. Count employees in each salary level
```sql
SELECT 
  CASE
    WHEN salary < 50000 THEN 'Low'
    WHEN salary BETWEEN 50000 AND 70000 THEN 'Medium'
    ELSE 'High'
  END AS salary_level,
  COUNT(*) AS total_employees
FROM employees
GROUP BY 
  CASE
    WHEN salary < 50000 THEN 'Low'
    WHEN salary BETWEEN 50000 AND 70000 THEN 'Medium'
    ELSE 'High'
  END;
```
  Explanation

• In query 1 & 3 we used Searched CASE (with conditions).
In query 2 we used Simple CASE (comparing one column against values).
Query 4 shows that we can use CASE inside GROUP BY as well.
ELSE is optional but recommended to handle unexpected values.

Key Points

CASE must end with END.
You can use CASE in SELECT, WHERE, ORDER BY, and GROUP BY.
Simple CASE is cleaner when checking one column against multiple values.

• Searched CASE is more flexible for complex conditions.
Bonus Challenge
Write a query that shows:
Employee name
• Salary
Salary Level (Low/Medium/High)
And sort the result by salary level in this order: High → Medium → Low


