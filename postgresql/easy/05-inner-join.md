# INNER JOIN in PostgreSQL

**Difficulty:** Easy  
**Topic:** INNER JOIN, Table Relationships  
**Tags:** #postgresql #sql #join #innerjoin #interview

## Overview

`INNER JOIN` is used to combine rows from two or more tables based on a related column between them.  
It returns only the rows that have matching values in both tables.

## Why do we use it?

In real databases, data is usually split into multiple tables to avoid duplication (normalization).  
For example:
- One table stores employee details
- Another table stores department details

We use `JOIN` to bring related information together in a single result.

## How it works

1. We specify two tables.
2. We define a condition that links them (usually primary key = foreign key).
3. Only rows that satisfy the condition are returned.

## Problem Statement

We have two tables:

**employees**

| id | name     | department_id | salary |
|----|----------|---------------|--------|
| 1  | Rahul    | 101           | 60000  |
| 2  | Priya    | 102           | 45000  |
| 3  | Amit     | 101           | 75000  |
| 4  | Sneha    | 103           | 52000  |
| 5  | Vikram   | 101           | 68000  |

**departments**

| id  | department_name |
|-----|-----------------|
| 101 | IT              |
| 102 | HR              |
| 103 | Finance         |

Write SQL queries for the following:

1. Show employee name along with their department name.
2. Show employee name, department name, and salary.
3. Show only employees who work in the **IT** department (using JOIN).
4. Count how many employees are there in each department (using JOIN + GROUP BY).

## Solution

```sql
-- 1. Employee name + Department name
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;

-- 2. Employee name + Department name + Salary
SELECT e.name, d.department_name, e.salary
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;

-- 3. Only IT department employees
SELECT e.name, d.department_name, e.salary
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
WHERE d.department_name = 'IT';

-- 4. Number of employees in each department
SELECT d.department_name, COUNT(e.id) AS total_employees
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
GROUP BY d.department_name;

```

Explanation
We give short aliases to tables (e for employees, d for departments) to make the query cleaner.
ON e.department_id = d.id is the joining condition.
INNER JOIN only returns matching records. If an employee has a department_id that does not exist in the departments table, that employee will not appear.

Key Points
INNER JOIN returns only matching rows from both tables.
Always use table aliases when joining multiple tables.
The ON clause defines how the tables are related.
You can combine JOIN with WHERE, GROUP BY, ORDER BY, etc.

Bonus Challenge
Write a query to display:
Department name
Average salary of that department
Number of employees in that department
…using INNER JOIN + GROUP BY.
