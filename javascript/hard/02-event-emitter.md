# 📡 JavaScript: Implement Event Emitter

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Event%20Emitter-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

An **Event Emitter** is a design pattern that allows objects to emit named events and register listeners (callbacks) for those events.

This pattern is used heavily in Node.js (`EventEmitter` class) and in frontend libraries.

### Core Methods we need to implement:

| Method              | Description                                      |
|---------------------|--------------------------------------------------|
| `on(event, listener)`    | Register a listener for an event            |
| `off(event, listener)`   | Remove a specific listener                  |
| `emit(event, ...args)`   | Trigger all listeners for an event          |
| `once(event, listener)`  | Register a listener that runs only once     |

---

## 🎯 Problem Statement

Implement a class `EventEmitter` with the following methods:

1. `on(eventName, callback)` → Subscribe to an event
2. `off(eventName, callback)` → Unsubscribe from an event
3. `emit(eventName, ...args)` → Emit an event and call all listeners
4. `once(eventName, callback)` → Subscribe to an event that automatically unsubscribes after first call

---

## 💻 Solution

```js
class EventEmitter {
  constructor() {
    // Store events as { eventName: [callback1, callback2, ...] }
    this.events = {};
  }

  // Subscribe to an event
  on(eventName, callback) {
    if (!this.events[eventName]) {
      this.events[eventName] = [];
    }
    this.events[eventName].push(callback);
  }

  // Unsubscribe from an event
  off(eventName, callback) {
    if (!this.events[eventName]) return;

    this.events[eventName] = this.events[eventName].filter(
      (cb) => cb !== callback
    );
  }

  // Emit an event
  emit(eventName, ...args) {
    if (!this.events[eventName]) return;

    // Create a copy to avoid issues if listeners are removed during emit
    const listeners = [...this.events[eventName]];
    listeners.forEach((callback) => {
      callback(...args);
    });
  }

  // Subscribe to an event that runs only once
  once(eventName, callback) {
    const wrapper = (...args) => {
      callback(...args);
      this.off(eventName, wrapper); // remove after first call
    };
    this.on(eventName, wrapper);
  }
}
```

---

### Example Usage

```js
const emitter = new EventEmitter();

function onUserLogin(name) {
  console.log(`User logged in: ${name}`);
}

function onUserLogout() {
  console.log("User logged out");
}

// Subscribe
emitter.on("login", onUserLogin);
emitter.on("logout", onUserLogout);

// Emit
emitter.emit("login", "Rahul");     // User logged in: Rahul
emitter.emit("logout");             // User logged out

// Once
emitter.once("notification", (msg) => {
  console.log("Notification:", msg);
});

emitter.emit("notification", "Welcome!"); // Notification: Welcome!
emitter.emit("notification", "Hello!");   // (nothing happens)

// Unsubscribe
emitter.off("login", onUserLogin);
emitter.emit("login", "Priya");     // (nothing happens)
```

---

## 🧠 Explanation

1. We use a plain object (`this.events`) to store event names as keys and arrays of callbacks as values.
2. `on` simply pushes the callback into the array.
3. `off` filters out the specific callback.
4. `emit` calls every callback registered for that event and passes any arguments.
5. `once` creates a wrapper function that calls the original callback and then immediately removes itself.

---

## 💡 Key Interview Points

- Why do we create a copy of the listeners array inside `emit`?  
  (To avoid problems if a listener removes itself or another listener during execution)
- Difference between `on` and `once`
- How would you implement `removeAllListeners(eventName)`?
- How does Node.js EventEmitter handle `error` events specially?
- Memory leaks: forgetting to call `off` can cause memory leaks

---

## 🔥 Bonus Challenges

1. Implement `removeAllListeners(eventName)` method
2. Implement a maximum number of listeners warning (like Node.js)
3. Support wildcard events (e.g., listening to `"user.*"`)
```
