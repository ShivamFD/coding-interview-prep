# LEFT JOIN in PostgreSQL

**Difficulty:** Easy  
**Topic:** LEFT JOIN, Outer Joins  
**Tags:** #postgresql #sql #join #leftjoin #interview

## Overview

`LEFT JOIN` (also called `LEFT OUTER JOIN`) returns **all rows** from the left table and the matched rows from the right table.  
If there is no match, the result will contain `NULL` values for columns of the right table.

## Why do we use it?

We use `LEFT JOIN` when we want to keep all records from one table even if there is no matching record in the other table.

**Example use cases:**
- Show all employees even if they don’t belong to any department
- Show all departments even if they have no employees
- Find records that are missing in another table

## How it works

1. All rows from the **left** table are returned.
2. Matching rows from the **right** table are added.
3. If no match is found, `NULL` is shown for the right table’s columns.

## Problem Statement

We have two tables:

**employees**

| id | name     | department_id | salary |
|----|----------|---------------|--------|
| 1  | Rahul    | 101           | 60000  |
| 2  | Priya    | 102           | 45000  |
| 3  | Amit     | 101           | 75000  |
| 4  | Sneha    | NULL          | 52000  |
| 5  | Vikram   | 104           | 68000  |

**departments**

| id  | department_name |
|-----|-----------------|
| 101 | IT              |
| 102 | HR              |
| 103 | Finance         |

Write SQL queries for the following:

1. Show all employees with their department name (include employees who have no department).
2. Show all employees and their department. Display “No Department” if department is missing.
3. Find employees who do **not** belong to any department.
4. Count how many employees are there in each department (including departments with zero employees).

## Solution

```sql
-- 1. All employees + department name (including those without department)
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;

-- 2. Show “No Department” when department is missing
SELECT 
  e.name, 
  COALESCE(d.department_name, 'No Department') AS department
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;

-- 3. Employees who do not belong to any department
SELECT e.name, e.salary
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id
WHERE d.id IS NULL;

-- 4. Count employees in each department (including zero)
SELECT 
  d.department_name, 
  COUNT(e.id) AS total_employees
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
GROUP BY d.department_name;
```

Explanation
In query 1, employee “Sneha” and “Vikram” will still appear even though they have no matching department.
COALESCE() is used to replace NULL with a custom message.
WHERE d.id IS NULL is the classic way to find non-matching rows (anti-join pattern).
When we put departments on the left side, we can also see departments that have zero employees.

Key Points
LEFT JOIN keeps all rows from the left table.
Use IS NULL on the right table’s key to find unmatched records.
COALESCE(column, 'default') is very useful for replacing NULLs.
The order of tables matters in LEFT JOIN.

Bonus Challenge
Write a query that shows:
All departments
Number of employees in each department
Average salary of each department
(Include departments that have no employees)

