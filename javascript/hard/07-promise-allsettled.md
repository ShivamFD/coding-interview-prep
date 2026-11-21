# 🤝 JavaScript: Implement Promise.allSettled

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Promise.allSettled-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`Promise.allSettled` is a static method that takes an iterable of promises and returns a single Promise.

### Key Behavior:
- It **waits for all promises** to settle (either fulfilled or rejected)
- It **never rejects**
- It returns an array of result objects

Each result object has this shape:

```js
{ status: "fulfilled", value: any }   // when resolved
{ status: "rejected", reason: any }   // when rejected
```

### Difference from Promise.all

| Method               | Behavior on rejection          | Returns                          |
|----------------------|--------------------------------|----------------------------------|
| `Promise.all`        | Rejects immediately            | Array of values                  |
| `Promise.allSettled` | Waits for all to settle        | Array of status objects          |

---

## 🎯 Problem Statement

Implement your own version of `Promise.allSettled` called `myAllSettled`.

Requirements:
1. Accept an array of promises (or values)
2. Return a new Promise
3. The Promise should always resolve (never reject)
4. Resolve with an array of objects containing `status` and `value`/`reason`
5. Preserve the original order

---

## 💻 Solution

```js
function myAllSettled(promises) {
  return new Promise((resolve) => {
    if (!Array.isArray(promises)) {
      return resolve([]);
    }

    const results = new Array(promises.length);
    let completed = 0;

    if (promises.length === 0) {
      return resolve([]);
    }

    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then((value) => {
          results[index] = {
            status: "fulfilled",
            value: value
          };
        })
        .catch((reason) => {
          results[index] = {
            status: "rejected",
            reason: reason
          };
        })
        .finally(() => {
          completed++;
          if (completed === promises.length) {
            resolve(results);
          }
        });
    });
  });
}
```

---

### Example Usage

```js
const p1 = Promise.resolve(10);
const p2 = Promise.reject("Something went wrong");
const p3 = new Promise((resolve) => setTimeout(() => resolve(30), 500));

myAllSettled([p1, p2, p3]).then((results) => {
  console.log(results);
  /*
  [
    { status: "fulfilled", value: 10 },
    { status: "rejected", reason: "Something went wrong" },
    { status: "fulfilled", value: 30 }
  ]
  */
});
```

---

### Another Example

```js
myAllSettled([
  Promise.resolve("Success"),
  Promise.reject(new Error("Failed")),
  42,                           // non-promise value
  Promise.resolve("Done")
]).then((results) => {
  results.forEach((result, i) => {
    if (result.status === "fulfilled") {
      console.log(`Promise ${i}:`, result.value);
    } else {
      console.log(`Promise ${i} failed:`, result.reason);
    }
  });
});
```

---

## 🧠 Explanation

1. We create a results array with the same length as the input.
2. We convert every item to a Promise using `Promise.resolve()` so non-promise values also work.
3. For every promise we attach both `.then` and `.catch`.
4. In both cases we store the result in the correct index.
5. We use a counter (`completed`) to know when all promises have settled.
6. Once all are settled, we resolve the main promise with the results array.
7. Because we catch every rejection, the main promise **never rejects**.

---

## 💡 Key Interview Points

- Difference between `Promise.all`, `Promise.allSettled`, `Promise.race`, and `Promise.any`
- Why does `allSettled` never reject?
- How do you preserve the order of results?
- What is the shape of the returned objects?
- Real-world use cases (running multiple independent API calls and handling partial failures)
- How would you implement `Promise.race`?

---

## 🔥 Bonus Challenges

1. Implement `myPromiseRace`
2. Implement `myPromiseAny`
3. Write a version of `allSettled` that also supports an AbortSignal
4. Create a helper that returns only the fulfilled values from `allSettled`
```
