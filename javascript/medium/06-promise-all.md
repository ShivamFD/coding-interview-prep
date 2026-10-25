# 🤝 JavaScript: Implement Promise.all

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Promise.all-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`Promise.all` is a static method that takes an iterable of promises and returns a single Promise.

### Behavior:
- Resolves when **all** promises resolve
- Rejects immediately if **any** promise rejects
- Preserves the order of results

This is one of the most commonly asked Promise-related interview questions.

---

## 🎯 Problem Statement

Implement your own version of `Promise.all` called `myPromiseAll`.

Requirements:
1. Accept an array of promises (or values)
2. Return a new Promise
3. Resolve with an array of results in the same order
4. Reject as soon as any promise rejects
5. Handle non-promise values correctly

---

## 💻 Solution

```js
function myPromiseAll(promises) {
  return new Promise((resolve, reject) => {
    // Handle empty array
    if (!Array.isArray(promises)) {
      return reject(new TypeError("Argument must be an array"));
    }

    const results = [];
    let completed = 0;

    if (promises.length === 0) {
      return resolve([]);
    }

    promises.forEach((promise, index) => {
      // Convert non-promise values to Promise
      Promise.resolve(promise)
        .then((value) => {
          results[index] = value;
          completed++;

          if (completed === promises.length) {
            resolve(results);
          }
        })
        .catch((error) => {
          reject(error); // reject immediately on first failure
        });
    });
  });
}
```

---

### Example Usage

```js
const p1 = Promise.resolve(10);
const p2 = new Promise((resolve) => setTimeout(() => resolve(20), 1000));
const p3 = Promise.resolve(30);

myPromiseAll([p1, p2, p3])
  .then((results) => {
    console.log(results); // [10, 20, 30]
  })
  .catch((error) => {
    console.log("Error:", error);
  });

// Example with rejection
const p4 = Promise.reject("Something went wrong");

myPromiseAll([p1, p4, p3])
  .then((results) => console.log(results))
  .catch((error) => console.log(error)); // "Something went wrong"
```

---

## 🧠 Explanation

1. We create a new Promise.
2. We keep an array `results` to store resolved values at the correct index.
3. We track how many promises have completed using a counter.
4. We use `Promise.resolve()` so that even non-promise values work.
5. As soon as any promise rejects → we immediately reject the main promise.
6. When all promises have resolved → we resolve with the results array.

---

## 💡 Key Interview Points

- Why do we use `results[index] = value` instead of `.push()`?  
  (To preserve order)
- Difference between `Promise.all` and `Promise.allSettled`
- Difference between `Promise.all` and `Promise.race`
- How does `Promise.all` behave with an empty array?
- How would you implement `Promise.allSettled`?
- What happens if one of the values is not a Promise?

---

## 🔥 Bonus Challenges

1. Implement `myPromiseAllSettled`
2. Implement `myPromiseRace`
3. Implement `myPromiseAny`
4. Add support for async iterable (advanced)
```
