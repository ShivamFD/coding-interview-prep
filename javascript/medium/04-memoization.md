# 🧠 JavaScript: Memoization

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Memoization-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

**Memoization** is an optimization technique where we cache the results of expensive function calls and return the cached result when the same inputs occur again.

### Why use Memoization?
- Avoid repeated expensive calculations
- Improve performance of pure functions
- Commonly used in dynamic programming and recursive algorithms

### Real-world examples:
- Fibonacci sequence
- API response caching
- Expensive mathematical calculations

---

## 🎯 Problem Statement

Write a `memoize` higher-order function that:

1. Takes a function as input
2. Returns a new memoized version of that function
3. Caches results based on the arguments
4. Returns the cached result if the same arguments are passed again

---

## 💻 Solution

```js
function memoize(fn) {
  const cache = {};

  return function (...args) {
    const key = JSON.stringify(args);

    if (key in cache) {
      console.log("Returning from cache");
      return cache[key];
    }

    console.log("Calculating result");
    const result = fn.apply(this, args);
    cache[key] = result;
    return result;
  };
}
```

---

### Example 1: Memoized Addition

```js
function add(a, b) {
  return a + b;
}

const memoizedAdd = memoize(add);

console.log(memoizedAdd(2, 3)); // Calculating result → 5
console.log(memoizedAdd(2, 3)); // Returning from cache → 5
console.log(memoizedAdd(4, 5)); // Calculating result → 9
```

---

### Example 2: Memoized Fibonacci (Very Common Interview Question)

```js
function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

const memoizedFib = memoize(function fib(n) {
  if (n <= 1) return n;
  return memoizedFib(n - 1) + memoizedFib(n - 2);
});

console.log(memoizedFib(40)); // Much faster than normal fibonacci(40)
```

---

## 🧠 Explanation

1. We create an empty `cache` object (closed over by the returned function → **Closure**).
2. When the memoized function is called, we create a unique key from the arguments using `JSON.stringify`.
3. If the key already exists in the cache → return the cached value.
4. If not → call the original function, store the result in cache, and return it.
5. We use `fn.apply(this, args)` to preserve the correct context.

---

## 💡 Key Interview Points

- Memoization only works correctly with **pure functions** (same input → same output).
- Why do we use `JSON.stringify` for the cache key?
- Limitation of using objects as cache (key order, object references).
- Difference between Memoization and Caching.
- How would you implement a memoize function with a maximum cache size (LRU)?
- Time and space complexity trade-off.

---

## 🔥 Bonus Challenges

1. Implement a memoize function that works correctly with object arguments.
2. Add a `.clear()` method to clear the cache.
3. Implement an LRU (Least Recently Used) cache version of memoize.
4. Write a memoized version of a recursive factorial function.
```
