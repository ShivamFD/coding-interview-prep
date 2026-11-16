# ✅ JavaScript: Array some() & every()

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Array%20some%20%26%20every-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`some()` and `every()` are two very useful array methods for checking conditions.

| Method     | Returns `true` when...                          | Stops early? |
|------------|--------------------------------------------------|--------------|
| `some()`   | **At least one** element passes the test         | Yes          |
| `every()`  | **All** elements pass the test                   | Yes          |

Both methods return a **boolean**.

---

## 🎯 Problem Statement

Given the following array of students, perform the tasks below.

```js
const students = [
  { name: "Rahul", score: 85, passed: true },
  { name: "Priya", score: 42, passed: false },
  { name: "Amit", score: 78, passed: true },
  { name: "Sneha", score: 91, passed: true },
  { name: "Vikram", score: 35, passed: false }
];
```

---

## 💻 Solutions

### 🔹 Task 1: Check if at least one student has scored more than 90

```js
const hasHighScorer = students.some((student) => student.score > 90);
console.log(hasHighScorer); // true
```

---

### 🔹 Task 2: Check if all students have passed

```js
const allPassed = students.every((student) => student.passed === true);
console.log(allPassed); // false
```

---

### 🔹 Task 3: Check if any student has failed

```js
const hasFailed = students.some((student) => student.passed === false);
console.log(hasFailed); // true
```

---

### 🔹 Task 4: Check if all students scored above 30

```js
const allAbove30 = students.every((student) => student.score > 30);
console.log(allAbove30); // true
```

---

### 🔹 Task 5: Check if there is any student named "Amit"

```js
const hasAmit = students.some((student) => student.name === "Amit");
console.log(hasAmit); // true
```

---

### 🔹 Task 6: Practical Example – Form Validation

```js
const formFields = [
  { name: "email", value: "rahul@example.com", isValid: true },
  { name: "password", value: "12345", isValid: false },
  { name: "name", value: "Rahul", isValid: true }
];

const isFormValid = formFields.every((field) => field.isValid);
console.log(isFormValid); // false
```

---

## 🧠 Explanation

- `some()` → Returns `true` as soon as it finds **one** matching element.
- `every()` → Returns `true` only if **every** element matches the condition.
- Both methods stop early (short-circuit) for better performance.
- They are cleaner than using a `for` loop with a flag.

---

## 💡 Key Interview Points

- Difference between `some()`, `every()`, `find()`, and `filter()`
- Do these methods mutate the original array? (No)
- What do they return? (Boolean)
- Real-world use cases (form validation, permission checks, etc.)

---

## 🔥 Bonus Challenges

1. Check if all products in a cart have price greater than 0
2. Check if at least one user is an admin
3. Write a function `isAllEven(arr)` using `every()`
```
