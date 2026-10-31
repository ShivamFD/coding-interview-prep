# ⚛️ React: Custom Hook - useFetch

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Custom%20Hook%20useFetch-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Creating custom hooks is one of the most powerful features of React.

A well-designed `useFetch` hook can be reused across the entire application for data fetching, handling loading, error, and success states cleanly.

This is a very common hard-level interview question.

---

## 🎯 Problem Statement

Create a custom hook `useFetch` that:

1. Accepts a URL
2. Returns `{ data, loading, error, refetch }`
3. Handles loading and error states
4. Supports aborting the request on unmount or URL change
5. Allows manual refetching

Then use it in a component to fetch and display users.

---

## 💻 Solution

### 1. useFetch.js (Custom Hook)

```jsx
import { useState, useEffect, useCallback } from "react";

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  const fetchData = useCallback(async (abortController) => {
    try {
      setLoading(true);
      setError(null);

      const response = await fetch(url, {
        signal: abortController?.signal
      });

      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }

      const result = await response.json();
      setData(result);
    } catch (err) {
      if (err.name !== "AbortError") {
        setError(err.message);
      }
    } finally {
      setLoading(false);
    }
  }, [url]);

  useEffect(() => {
    const controller = new AbortController();
    fetchData(controller);

    return () => {
      controller.abort();
    };
  }, [fetchData]);

  const refetch = () => {
    const controller = new AbortController();
    fetchData(controller);
  };

  return { data, loading, error, refetch };
}

export default useFetch;
```

---

### 2. Using the Hook

```jsx
import useFetch from "./useFetch";

function UserList() {
  const { data: users, loading, error, refetch } = useFetch(
    "https://jsonplaceholder.typicode.com/users"
  );

  if (loading) {
    return <h2>Loading users...</h2>;
  }

  if (error) {
    return (
      <div>
        <h2 style={{ color: "red" }}>Error: {error}</h2>
        <button onClick={refetch}>Try Again</button>
      </div>
    );
  }

  return (
    <div>
      <h2>Users</h2>
      <button onClick={refetch}>Refresh</button>

      <ul>
        {users?.map((user) => (
          <li key={user.id}>
            <strong>{user.name}</strong> — {user.email}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default UserList;
```

---

## 🧠 Explanation

1. We manage three main states: `data`, `loading`, and `error`.
2. `useCallback` ensures the fetch function is stable.
3. `AbortController` is used to cancel the request when the component unmounts or the URL changes.
4. The `refetch` function allows manual re-triggering of the API call.
5. The hook is completely reusable — just pass any URL.

---

## 💡 Key Interview Points

- Why should we create custom hooks?
- How do you handle race conditions in data fetching?
- Why do we use `AbortController`?
- Difference between putting fetch logic in the component vs a custom hook
- How would you extend this hook to support POST/PUT requests?
- How would you add caching to this hook?

---

## 🔥 Bonus Challenges

1. Add support for request options (method, headers, body)
2. Add caching so the same URL is not fetched again
3. Create a `usePost` hook using the same pattern
4. Add a timeout feature (abort if request takes more than 5 seconds)
```
