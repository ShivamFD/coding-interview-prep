# 🍛 JavaScript: Currying

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Currying-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

**Currying** is a technique of converting a function that takes multiple arguments into a sequence of functions, each taking a single argument.

Instead of:
```js
sum(1, 2, 3)
```

We can write:
```js
sum(1)(2)(3)
```

### Why is Currying useful?
- Function composition
- Partial application
- Reusability
- Cleaner functional code

---

## 🎯 Problem Statement

Implement a `curry` function that:

1. Takes any function as input
2. Returns a curried version of that function
3. The curried function can be called with one argument at a time or multiple arguments
4. It should work until all required arguments are provided

**Example:**

```js
function sum(a, b, c) {
  return a + b + c;
}

const curriedSum = curry(sum);

console.log(curriedSum(1)(2)(3));     // 6
console.log(curriedSum(1, 2)(3));     // 6
console.log(curriedSum(1)(2, 3));     // 6
console.log(curriedSum(1, 2, 3));     // 6
```

---

## 💻 Solution

```js
function curry(fn) {
  return function curried(...args) {
    // If enough arguments are provided, execute the original function
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    }

    // Otherwise, return a new function that collects more arguments
    return function (...nextArgs) {
      return curried.apply(this, args.concat(nextArgs));
    };
  };
}
```

---

### Example Usage

```js
function multiply(a, b, c) {
  return a * b * c;
}

const curriedMultiply = curry(multiply);

console.log(curriedMultiply(2)(3)(4));     // 24
console.log(curriedMultiply(2, 3)(4));     // 24
console.log(curriedMultiply(2)(3, 4));     // 24
```

---

## 🧠 Explanation

1. `fn.length` tells us how many parameters the original function expects.
2. We collect arguments using rest parameters (`...args`).
3. If we have enough arguments → call the original function.
4. If not → return a new function that keeps collecting arguments.
5. We use `.apply()` to preserve the `this` context and pass arguments correctly.
6. This implementation supports both single and multiple argument calls at each step.

---

## 💡 Key Interview Points

- Difference between Currying and Partial Application
- How does `fn.length` work?
- Can you implement infinite currying (like `sum(1)(2)(3)(4)...()`)?
- Real-world use cases of currying
- How closures help in currying

---

## 🔥 Bonus Challenges

1. Implement **infinite currying** so that this works:

```js
sum(1)(2)(3)(4)(5)()  // 15
```

2. Implement a version that supports placeholder values (like lodash’s `_.curry`).
```
