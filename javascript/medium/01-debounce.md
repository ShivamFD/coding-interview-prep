# ⏱️ JavaScript: Implement Debounce

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Debounce-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

**Debounce** is a technique used to limit how often a function can execute.  

It waits for a certain amount of time after the last call before actually executing the function.

### Real-world use cases:
- Search input (wait until user stops typing)
- Window resize events
- Button click spam prevention
- API call optimization

---

## 🎯 Problem Statement

Write a `debounce` function that:

- Takes a function `fn` and a delay `delay` (in milliseconds)
- Returns a new function
- The new function should only call `fn` after the user has stopped calling it for `delay` milliseconds
- If the function is called again before the delay is over, the previous timer should be cancelled

---

## 💻 Solution

```js
function debounce(fn, delay) {
  let timerId;

  return function (...args) {
    // Clear the previous timer
    clearTimeout(timerId);

    // Start a new timer
    timerId = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}
```

---

### Example Usage

```js
function search(query) {
  console.log("Searching for:", query);
}

const debouncedSearch = debounce(search, 500);

// Simulate rapid typing
debouncedSearch("H");
debouncedSearch("He");
debouncedSearch("Hel");
debouncedSearch("Hell");
debouncedSearch("Hello");

// Only "Searching for: Hello" will be printed after 500ms
```

---

## 🧠 Explanation

1. We create a variable `timerId` to store the timeout ID (this is closed over by the returned function → **Closure**).
2. Every time the debounced function is called:
   - We clear the previous timeout using `clearTimeout`.
   - We set a new timeout.
3. Only when the user stops calling the function for `delay` ms, the original function runs.
4. We use `fn.apply(this, args)` to preserve the correct `this` context and pass all arguments.

---

## 💡 Key Interview Points

- Debounce vs Throttle → Very common follow-up question
- Why do we use Closure here?
- Why `apply` / `call` instead of directly calling `fn()`?
- How would you cancel a pending debounced call?
- Can you implement leading-edge debounce (execute immediately first time)?

---

## 🔥 Bonus Challenges

1. Implement a version that executes **immediately** on the first call (leading edge) and then debounces subsequent calls.
2. Add a `.cancel()` method to the debounced function so you can cancel the pending execution.
3. Implement **Throttle** function.
```
