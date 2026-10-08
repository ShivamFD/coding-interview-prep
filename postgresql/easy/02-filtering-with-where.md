# Filtering Data with WHERE Clause

**Difficulty:** Easy  
**Topic:** SQL Filtering, AND, OR, IN, BETWEEN  
**Tags:** #postgresql #sql #where #filtering #interview

## Problem Statement

We have the same `employees` table:

| id | name     | department | salary | joining_year |
|----|----------|------------|--------|--------------|
| 1  | Rahul    | IT         | 60000  | 2021         |
| 2  | Priya    | HR         | 45000  | 2022         |
| 3  | Amit     | IT         | 75000  | 2020         |
| 4  | Sneha    | Finance    | 52000  | 2023         |
| 5  | Vikram   | IT         | 68000  | 2021         |
| 6  | Neha     | HR         | 48000  | 2022         |

Write SQL queries for the following:

1. Employees who work in **IT** department **and** have salary greater than 65000.
2. Employees who work in **HR** or **Finance** department.
3. Employees whose salary is between 50000 and 70000 (inclusive).
4. Employees who joined in the year 2021 or 2022.
5. Employees whose name starts with the letter **A**.

## Solution

```sql
-- 1. IT department AND salary > 65000
SELECT * FROM employees
WHERE department = 'IT' AND salary > 65000;

-- 2. HR or Finance department
SELECT * FROM employees
WHERE department = 'HR' OR department = 'Finance';

-- Better way using IN
SELECT * FROM employees
WHERE department IN ('HR', 'Finance');

-- 3. Salary between 50000 and 70000
SELECT * FROM employees
WHERE salary BETWEEN 50000 AND 70000;

-- 4. Joined in 2021 or 2022
SELECT * FROM employees
WHERE joining_year IN (2021, 2022);

-- 5. Name starts with 'A'
SELECT * FROM employees
WHERE name LIKE 'A%';
```
Explanation
AND → both conditions must be true
OR → at least one condition must be true
IN → checks if a value matches any value in a list (cleaner than multiple ORs)
BETWEEN ... AND ... → inclusive range (includes both starting and ending values)
LIKE 'A%' → pattern matching (% means any number of characters)

Key Points
Always use single quotes for string and date values.
BETWEEN is inclusive (includes both ends).
IN is preferred over multiple OR conditions.
LIKE is case-sensitive in PostgreSQL by default (use ILIKE for case-insensitive).

Bonus Challenge
Write a query to find employees who:
Work in IT department
Have salary greater than 60000
And joined in 2021
