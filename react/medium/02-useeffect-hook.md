# ⚛️ React: useEffect Hook

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-useEffect-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`useEffect` is a React Hook that lets you perform **side effects** in functional components.

### Common use cases of useEffect:
- Fetching data from an API
- Updating the document title
- Setting up subscriptions or timers
- Logging
- Manually changing the DOM

### Basic Syntax

```js
useEffect(() => {
  // side effect code

  return () => {
    // cleanup function (optional)
  };
}, [dependencies]);
```

---

## 🎯 Problem Statement

Create a component called `UserList` that:

1. Fetches a list of users from an API when the component mounts
2. Shows a loading state while data is being fetched
3. Displays the list of users
4. Handles errors gracefully
5. Uses `useEffect` properly with dependency array

**API to use:**  
`https://jsonplaceholder.typicode.com/users`

---

## 💻 Solution

```jsx
import { useState, useEffect } from "react";

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchUsers = async () => {
      try {
        setLoading(true);
        const response = await fetch("https://jsonplaceholder.typicode.com/users");

        if (!response.ok) {
          throw new Error("Failed to fetch users");
        }

        const data = await response.json();
        setUsers(data);
        setError(null);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchUsers();
  }, []); // empty dependency array → runs only once on mount

  if (loading) {
    return <h2>Loading users...</h2>;
  }

  if (error) {
    return <h2 style={{ color: "red" }}>Error: {error}</h2>;
  }

  return (
    <div>
      <h2>User List</h2>
      <ul>
        {users.map((user) => (
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

1. **Three states** are managed:
   - `users` → to store the data
   - `loading` → to show loading UI
   - `error` → to handle errors

2. `useEffect` with **empty dependency array `[]`** runs only once when the component mounts (perfect for data fetching).

3. We use `async/await` inside `useEffect` for clean asynchronous code.

4. `try/catch/finally` is used for proper error and loading handling.

5. Always provide a unique `key` when rendering lists.

---

## 💡 Key Interview Points

- Why do we need a dependency array?
- What happens if we don’t pass a dependency array?
- Difference between:
  - `useEffect(() => {}, [])`          → runs once
  - `useEffect(() => {})`              → runs on every render
  - `useEffect(() => {}, [value])`     → runs when value changes
- Why should we not make the useEffect callback itself async?
- How to perform cleanup in useEffect (for timers, subscriptions, etc.)?
- How to cancel a fetch request when component unmounts?

---

## 🔥 Bonus Challenges

1. Add a button to refetch the users
2. Add a search input that filters users by name
3. Implement cleanup to abort the fetch if the component unmounts
4. Convert this into a custom hook called `useFetch(url)`
```
