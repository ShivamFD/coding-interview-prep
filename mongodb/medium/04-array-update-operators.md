# 📦 MongoDB: Array Update Operators

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Array%20Update%20Operators-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

MongoDB provides powerful operators to update arrays inside documents.

These operators are very commonly used when working with lists (tags, skills, comments, items, etc.).

### Most Important Array Update Operators

| Operator          | Description                                      |
|-------------------|--------------------------------------------------|
| `$push`           | Adds an element to the end of the array          |
| `$addToSet`       | Adds an element only if it doesn’t already exist |
| `$pop`            | Removes first (-1) or last (1) element           |
| `$pull`           | Removes all elements that match a condition      |
| `$pullAll`        | Removes all matching values from the array       |
| `$each`           | Used with `$push` / `$addToSet` to add multiple  |
| `$position`       | Used with `$push` to insert at specific index    |
| `$inc`            | Increments a numeric value                       |

---

## 🗄️ Sample Collection: `students`

```json
[
  {
    "_id": 1,
    "name": "Rahul",
    "skills": ["JavaScript", "React"],
    "scores": [85, 90, 78]
  },
  {
    "_id": 2,
    "name": "Priya",
    "skills": ["Python", "SQL"],
    "scores": [92, 88]
  },
  {
    "_id": 3,
    "name": "Amit",
    "skills": ["JavaScript", "Node.js", "MongoDB"],
    "scores": [70, 75, 80, 85]
  }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Add a new skill using `$push`

```js
db.students.updateOne(
  { name: "Rahul" },
  { $push: { skills: "Node.js" } }
)
```

---

### 🔹 Task 2: Add a skill only if it doesn’t already exist (`$addToSet`)

```js
db.students.updateOne(
  { name: "Rahul" },
  { $addToSet: { skills: "React" } }   // will not add because React already exists
)
```

---

### 🔹 Task 3: Add multiple skills at once using `$each`

```js
db.students.updateOne(
  { name: "Priya" },
  { 
    $push: { 
      skills: { $each: ["HTML", "CSS", "JavaScript"] } 
    } 
  }
)
```

---

### 🔹 Task 4: Remove a specific skill using `$pull`

```js
db.students.updateOne(
  { name: "Amit" },
  { $pull: { skills: "MongoDB" } }
)
```

---

### 🔹 Task 5: Remove multiple values using `$pullAll`

```js
db.students.updateOne(
  { name: "Amit" },
  { $pullAll: { skills: ["JavaScript", "Node.js"] } }
)
```

---

### 🔹 Task 6: Remove the last score using `$pop`

```js
db.students.updateOne(
  { name: "Rahul" },
  { $pop: { scores: 1 } }    // 1 = remove last, -1 = remove first
)
```

---

### 🔹 Task 7: Add a score at the beginning of the array

```js
db.students.updateOne(
  { name: "Priya" },
  { 
    $push: { 
      scores: { 
        $each: [95], 
        $position: 0 
      } 
    } 
  }
)
```

---

### 🔹 Task 8: Increment a value inside an array (using positional operator)

```js
// First, let's say we want to increase the first score of Rahul by 5
db.students.updateOne(
  { name: "Rahul" },
  { $inc: { "scores.0": 5 } }
)
```

---

## 💡 Key Takeaways

- `$push` always adds the element (can create duplicates).
- `$addToSet` adds only unique elements.
- `$pull` removes elements that match a condition.
- `$each` is used when you want to add multiple items at once.
- `$position` allows inserting at a specific index.
- `$pop` can remove from front or back of the array.
- These operators are extremely common in real-world applications.

---

## 🔥 Bonus Challenge

Write update queries for the following:

1. Add “TypeScript” and “GraphQL” to Amit’s skills only if they don’t already exist.
2. Remove all scores less than 80 from Amit’s scores array.
3. Add a new score 99 at the end of Priya’s scores.
```
