# ⏱️ JavaScript: Implement Throttle

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Throttle-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

**Throttle** is a technique used to limit the rate at which a function can be executed.

Unlike Debounce (which waits until the user stops calling), Throttle ensures that the function is called at most once in a given time interval.

### Real-world use cases:
- Scroll event handling
- Window resize events
- Button click rate limiting
- API polling / continuous events

### Debounce vs Throttle

| Feature       | Debounce                          | Throttle                          |
|---------------|-----------------------------------|-----------------------------------|
| When it runs  | After user stops calling          | At regular intervals              |
| Use case      | Search input                      | Scroll / Resize                   |
| Behavior      | Delays execution                  | Limits frequency                  |

---

## 🎯 Problem Statement

Write a `throttle` function that:

- Takes a function `fn` and a delay `delay` (in milliseconds)
- Returns a new function
- The new function should execute `fn` at most once every `delay` milliseconds
- Extra calls within the delay period should be ignored

---

## 💻 Solution

```js
function throttle(fn, delay) {
  let lastCall = 0;

  return function (...args) {
    const now = Date.now();

    if (now - lastCall >= delay) {
      lastCall = now;
      fn.apply(this, args);
    }
  };
}
```

---

### Example Usage

```js
function onScroll() {
  console.log("Scroll event fired at", new Date().toLocaleTimeString());
}

const throttledScroll = throttle(onScroll, 1000);

// Simulate rapid scroll events
window.addEventListener("scroll", throttledScroll);

// Even if scroll fires 100 times in a second,
// onScroll will execute at most once every 1000ms
```

---

## 🧠 Explanation

1. We keep track of the last time the function was called using `lastCall`.
2. Every time the throttled function is invoked, we check the current time.
3. If enough time (`delay`) has passed since the last call → execute the function and update `lastCall`.
4. If not enough time has passed → ignore the call.
5. We use `fn.apply(this, args)` to preserve the correct context and pass arguments.

---

## 💡 Key Interview Points

- Difference between Debounce and Throttle (most common follow-up)
- Why do we use `Date.now()` instead of `setTimeout` in this version?
- How would you implement a trailing-edge throttle (call at the end of the interval)?
- Can you implement both leading and trailing options?
- Real-world examples of throttle

---

## 🔥 Bonus Challenges

1. Implement a version that supports both **leading** and **trailing** options.
2. Add a `.cancel()` method to the throttled function.
3. Compare performance of Debounce vs Throttle on a scroll event.
```
