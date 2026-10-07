# Basic SELECT Queries in PostgreSQL

**Difficulty:** Easy  
**Topic:** SQL Basics, SELECT, WHERE, ORDER BY, LIMIT  
**Tags:** #postgresql #sql #select #basics #interview

## Problem Statement

We have a table called `employees` with the following data:

| id | name     | department | salary |
|----|----------|------------|--------|
| 1  | Rahul    | IT         | 60000  |
| 2  | Priya    | HR         | 45000  |
| 3  | Amit     | IT         | 75000  |
| 4  | Sneha    | Finance    | 52000  |
| 5  | Vikram   | IT         | 68000  |

Write SQL queries for the following:

1. Select all columns from the employees table.
2. Select only the `name` and `salary` of all employees.
3. Select employees who work in the **IT** department.
4. Select employees whose salary is greater than 60000.
5. Select all employees and sort them by salary in **descending** order.
6. Select the top 3 highest paid employees.

## Solution

```sql
-- 1. Select all columns
SELECT * FROM employees;

-- 2. Select only name and salary
SELECT name, salary FROM employees;

-- 3. Employees in IT department
SELECT * FROM employees
WHERE department = 'IT';

-- 4. Salary greater than 60000
SELECT * FROM employees
WHERE salary > 60000;

-- 5. Sort by salary (highest to lowest)
SELECT * FROM employees
ORDER BY salary DESC;

-- 6. Top 3 highest paid employees
SELECT * FROM employees
ORDER BY salary DESC
LIMIT 3;

```

Explanation
SELECT * → selects all columns
SELECT column1, column2 → selects specific columns
WHERE → used for filtering rows
ORDER BY column DESC → sorts in descending order (use ASC for ascending)
LIMIT n → returns only the first n rows
These are the most fundamental building blocks of SQL.

Key Points
SQL keywords are case-insensitive, but we usually write them in UPPERCASE.
String values are written in single quotes: 'IT'
Always use WHERE before ORDER BY and LIMIT.
Bonus Challenge
Write a query to find employees who work in IT department and have salary greater than 65000.
