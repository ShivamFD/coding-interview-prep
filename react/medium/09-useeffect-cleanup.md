# ⚛️ React: useEffect Cleanup Function

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-useEffect%20Cleanup-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

The cleanup function in `useEffect` is used to clean up side effects before the component unmounts or before the effect runs again.

### Common use cases for cleanup:
- Clearing timers (`setTimeout`, `setInterval`)
- Cancelling API requests (AbortController)
- Removing event listeners
- Unsubscribing from WebSocket / subscriptions

---

## 🎯 Problem Statement

Create two practical examples:

1. A timer component that starts counting when mounted and clears the interval when unmounted
2. A component that adds a window resize event listener and removes it on cleanup

---

## 💻 Solution

### Example 1: Timer with Cleanup

```jsx
import { useState, useEffect } from "react";

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    console.log("Timer started");

    const intervalId = setInterval(() => {
      setSeconds((prev) => prev + 1);
    }, 1000);

    // Cleanup function
    return () => {
      console.log("Timer cleaned up");
      clearInterval(intervalId);
    };
  }, []); // empty dependency → runs only on mount/unmount

  return (
    <div>
      <h2>Timer: {seconds}s</h2>
      <p>Open the console and unmount this component to see cleanup.</p>
    </div>
  );
}

export default Timer;
```

---

### Example 2: Window Resize Listener

```jsx
import { useState, useEffect } from "react";

function WindowSize() {
  const [size, setSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });

  useEffect(() => {
    const handleResize = () => {
      setSize({
        width: window.innerWidth,
        height: window.innerHeight
      });
    };

    window.addEventListener("resize", handleResize);
    console.log("Resize listener added");

    // Cleanup: remove the event listener
    return () => {
      window.removeEventListener("resize", handleResize);
      console.log("Resize listener removed");
    };
  }, []);

  return (
    <div>
      <h2>Window Size</h2>
      <p>Width: {size.width}px</p>
      <p>Height: {size.height}px</p>
    </div>
  );
}

export default WindowSize;
```

---

### Example 3: Cleanup with Fetch (AbortController)

```jsx
import { useState, useEffect } from "react";

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    const controller = new AbortController();

    const fetchUser = async () => {
      try {
        const response = await fetch(
          `https://jsonplaceholder.typicode.com/users/${userId}`,
          { signal: controller.signal }
        );
        const data = await response.json();
        setUser(data);
      } catch (error) {
        if (error.name !== "AbortError") {
          console.error(error);
        }
      }
    };

    fetchUser();

    // Cleanup: abort the request if component unmounts
    return () => {
      controller.abort();
    };
  }, [userId]);

  if (!user) return <p>Loading...</p>;

  return (
    <div>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
}

export default UserProfile;
```

---

## 🧠 Explanation

- The function you return from `useEffect` is the **cleanup function**.
- It runs:
  1. When the component unmounts
  2. Before the effect runs again (if dependencies change)
- Always clean up timers, listeners, and subscriptions to avoid memory leaks.

---

## 💡 Key Interview Points

- When does the cleanup function run?
- Why is cleanup important?
- What happens if you forget to clear an interval?
- How do you cancel a fetch request?
- Difference between cleanup on unmount vs cleanup before re-running the effect

---

## 🔥 Bonus Challenges

1. Create a component that tracks mouse position and cleans up the event listener
2. Build a countdown timer that stops when it reaches 0 and cleans up properly
3. Show how missing cleanup can cause memory leaks (with console logs)
```
