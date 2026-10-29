# 🔗 JavaScript: Function Composition & Pipe

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Function%20Composition-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

**Function Composition** is the process of combining two or more functions to produce a new function.

Instead of writing:

```js
const result = f(g(h(x)));
```

We can write:

```js
const result = compose(f, g, h)(x);
```

### Two common styles:

| Style       | Order of execution          | Name     |
|-------------|-----------------------------|----------|
| Right → Left | `f(g(h(x)))`               | compose  |
| Left → Right | `h(g(f(x)))`               | pipe     |

---

## 🎯 Problem Statement

Implement two higher-order functions:

1. `compose` → executes functions from **right to left**
2. `pipe` → executes functions from **left to right**

---

## 💻 Solution

### 1. Compose (Right to Left)

```js
function compose(...fns) {
  return function (value) {
    return fns.reduceRight((acc, fn) => fn(acc), value);
  };
}
```

### 2. Pipe (Left to Right)

```js
function pipe(...fns) {
  return function (value) {
    return fns.reduce((acc, fn) => fn(acc), value);
  };
}
```

---

### Example Usage

```js
const add5 = (x) => x + 5;
const multiply3 = (x) => x * 3;
const subtract2 = (x) => x - 2;

// Using compose (right to left)
const calculateCompose = compose(subtract2, multiply3, add5);
console.log(calculateCompose(10)); 
// Step by step: 10 → +5 = 15 → *3 = 45 → -2 = 43

// Using pipe (left to right)
const calculatePipe = pipe(add5, multiply3, subtract2);
console.log(calculatePipe(10)); 
// Step by step: 10 → +5 = 15 → *3 = 45 → -2 = 43
```

---

### Real-world Example

```js
const trim = (str) => str.trim();
const toLower = (str) => str.toLowerCase();
const addExclamation = (str) => str + "!";

const formatText = pipe(trim, toLower, addExclamation);

console.log(formatText("  Hello World  ")); // "hello world!"
```

---

## 🧠 Explanation

- Both `compose` and `pipe` take multiple functions and return a new function.
- `compose` uses `reduceRight` so the right-most function runs first.
- `pipe` uses `reduce` so the left-most function runs first.
- This pattern is very common in functional programming and libraries like Lodash, Ramda, and Redux.

---

## 💡 Key Interview Points

- Difference between `compose` and `pipe`
- Why is function composition useful?
- How does `reduce` help in composition?
- Can we compose async functions?
- Real-world libraries that use this pattern heavily

---

## 🔥 Bonus Challenges

1. Implement a version that supports asynchronous functions
2. Add error handling inside compose/pipe
3. Create a `compose` function that works with any number of arguments (not just unary functions)
```
