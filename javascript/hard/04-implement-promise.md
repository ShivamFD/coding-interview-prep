# 🤝 JavaScript: Implement a Basic Promise

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Implement%20Promise-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

A **Promise** is an object that represents the eventual completion (or failure) of an asynchronous operation.

Implementing a basic Promise from scratch is a classic hard JavaScript interview question. It tests your understanding of:

- Asynchronous behavior
- Closures
- State management
- Microtasks

---

## 🎯 Problem Statement

Implement a basic `MyPromise` class that supports:

1. States: `pending`, `fulfilled`, `rejected`
2. `.then()` method
3. `.catch()` method
4. Executor function with `resolve` and `reject`
5. Chaining (basic level)

---

## 💻 Solution

```js
class MyPromise {
  constructor(executor) {
    this.state = "pending";
    this.value = undefined;
    this.reason = undefined;
    this.onFulfilledCallbacks = [];
    this.onRejectedCallbacks = [];

    const resolve = (value) => {
      if (this.state === "pending") {
        this.state = "fulfilled";
        this.value = value;
        this.onFulfilledCallbacks.forEach((fn) => fn());
      }
    };

    const reject = (reason) => {
      if (this.state === "pending") {
        this.state = "rejected";
        this.reason = reason;
        this.onRejectedCallbacks.forEach((fn) => fn());
      }
    };

    try {
      executor(resolve, reject);
    } catch (error) {
      reject(error);
    }
  }

  then(onFulfilled, onRejected) {
    onFulfilled = typeof onFulfilled === "function" ? onFulfilled : (value) => value;
    onRejected = typeof onRejected === "function" ? onRejected : (reason) => { throw reason; };

    const promise2 = new MyPromise((resolve, reject) => {
      if (this.state === "fulfilled") {
        queueMicrotask(() => {
          try {
            const x = onFulfilled(this.value);
            resolve(x);
          } catch (error) {
            reject(error);
          }
        });
      }

      if (this.state === "rejected") {
        queueMicrotask(() => {
          try {
            const x = onRejected(this.reason);
            resolve(x);
          } catch (error) {
            reject(error);
          }
        });
      }

      if (this.state === "pending") {
        this.onFulfilledCallbacks.push(() => {
          queueMicrotask(() => {
            try {
              const x = onFulfilled(this.value);
              resolve(x);
            } catch (error) {
              reject(error);
            }
          });
        });

        this.onRejectedCallbacks.push(() => {
          queueMicrotask(() => {
            try {
              const x = onRejected(this.reason);
              resolve(x);
            } catch (error) {
              reject(error);
            }
          });
        });
      }
    });

    return promise2;
  }

  catch(onRejected) {
    return this.then(null, onRejected);
  }
}
```

---

### Example Usage

```js
const promise = new MyPromise((resolve, reject) => {
  setTimeout(() => {
    resolve("Success!");
  }, 1000);
});

promise
  .then((value) => {
    console.log("Then 1:", value);
    return value + " Continued";
  })
  .then((value) => {
    console.log("Then 2:", value);
  })
  .catch((error) => {
    console.log("Caught:", error);
  });
```

---

## 🧠 Explanation

1. A Promise has three states: `pending` → `fulfilled` or `rejected`.
2. Once settled, the state cannot change.
3. We store callbacks in arrays because `.then()` can be called before the Promise settles.
4. `queueMicrotask` is used to make the behavior asynchronous (like real Promises).
5. `.then()` always returns a new Promise so that chaining is possible.
6. `.catch()` is just syntactic sugar for `.then(null, onRejected)`.

---

## 💡 Key Interview Points

- What are the three states of a Promise?
- Why do we use `queueMicrotask` or `setTimeout`?
- Difference between microtasks and macrotasks
- How does Promise chaining work?
- What happens if we call `resolve` twice?
- Difference between Promise and callback patterns
- How would you implement `Promise.all` using this?

---

## 🔥 Bonus Challenges

1. Add support for `Promise.resolve()` and `Promise.reject()` static methods
2. Improve the resolution procedure to handle thenables (Promise/A+ compliance)
3. Implement `finally()` method
