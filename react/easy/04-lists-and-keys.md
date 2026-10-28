# ⚛️ React: Lists and Keys

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Lists%20%26%20Keys-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

In React, we render lists of data using the `.map()` method.

When rendering lists, we must provide a unique **`key`** prop to each item.  
Keys help React identify which items have changed, been added, or removed.

---

## 🎯 Problem Statement

Create a component that:

1. Displays a list of users
2. Allows adding a new user
3. Allows deleting a user
4. Uses proper keys

---

## 💻 Solution

```jsx
import { useState } from "react";

function UserList() {
  const [users, setUsers] = useState([
    { id: 1, name: "Rahul" },
    { id: 2, name: "Priya" },
    { id: 3, name: "Amit" }
  ]);

  const [input, setInput] = useState("");

  const addUser = () => {
    if (input.trim() === "") return;

    const newUser = {
      id: Date.now(),
      name: input.trim()
    };

    setUsers([...users, newUser]);
    setInput("");
  };

  const deleteUser = (id) => {
    setUsers(users.filter((user) => user.id !== id));
  };

  return (
    <div>
      <h2>User List</h2>

      <input
        type="text"
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Enter name"
      />
      <button onClick={addUser}>Add User</button>

      <ul>
        {users.map((user) => (
          <li key={user.id}>
            {user.name}
            <button onClick={() => deleteUser(user.id)}>Delete</button>
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

- We use `.map()` to loop over the array and return JSX for each item.
- Each item must have a unique `key` prop.
- Using the array `index` as key is **not recommended** when the list can change (add/remove/reorder).
- Best practice: use a unique and stable id (`id` from database or `Date.now()`).

---

## 💡 Key Interview Points

- Why do we need keys?
- Why should we not use index as key?
- What happens if keys are missing or duplicated?
- How does React use keys for reconciliation?
- Difference between key and id/ref

---

## 🔥 Bonus Challenges

1. Add an edit feature for each user
2. Show a message when the list is empty
3. Sort the list alphabetically
```
