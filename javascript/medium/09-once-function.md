# 🔒 JavaScript: Implement Once Function

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Once%20Function-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

The `once` function is a higher-order function that ensures a given function can be executed **only once**.

After the first call, subsequent calls return the result of the first call (or do nothing).

### Real-world use cases:
- Payment processing (prevent double charge)
- Initialization code that should run only once
- Event handlers that should fire only once
- API calls that should not be repeated

---

## 🎯 Problem Statement

Implement a function `once(fn)` that:

1. Takes a function as input
2. Returns a new function
3. The returned function can be called only once
4. Subsequent calls return the result of the first call

---

## 💻 Solution

```js
function once(fn) {
  let called = false;
  let result;

  return function (...args) {
    if (!called) {
      called = true;
      result = fn.apply(this, args);
      return result;
    }
    return result; // return the cached result on subsequent calls
  };
}
```

---

### Example Usage

```js
function initialize() {
  console.log("Initializing application...");
  return "Initialized";
}

const initializeOnce = once(initialize);

console.log(initializeOnce()); // "Initializing application..." → "Initialized"
console.log(initializeOnce()); // "Initialized" (no console log)
console.log(initializeOnce()); // "Initialized" (no console log)
```

---

### Another Example (Payment)

```js
function processPayment(amount) {
  console.log(`Processing payment of ₹${amount}`);
  return `Payment of ₹${amount} successful`;
}

const payOnce = once(processPayment);

console.log(payOnce(1000)); // Processing payment of ₹1000 → Payment of ₹1000 successful
console.log(payOnce(1000)); // Payment of ₹1000 successful (no processing again)
console.log(payOnce(5000)); // Payment of ₹1000 successful (still the first result)
```

---

## 🧠 Explanation

1. We use a closure to keep track of whether the function has already been called (`called` flag).
2. We also store the result of the first call.
3. On the first call → execute the original function and store the result.
4. On later calls → simply return the stored result.
5. We use `fn.apply(this, args)` to preserve the correct context and pass all arguments.

---

## 💡 Key Interview Points

- How does closure help in implementing `once`?
- Difference between `once` and `debounce` / `throttle`
- How would you implement a version that throws an error on subsequent calls?
- How would you implement `once` that allows resetting?
- Real-world scenarios where `once` is useful

---

## 🔥 Bonus Challenges

1. Implement a version that ignores subsequent calls completely (returns `undefined`)
2. Add a `.reset()` method so the function can be called once again
3. Implement `once` that works correctly with asynchronous functions (Promises)
```
