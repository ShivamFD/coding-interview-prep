# 📂 MongoDB: $unwind Stage

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-%24unwind-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

The `$unwind` stage in the Aggregation Pipeline deconstructs an array field from the input documents and outputs **one document for each element** of the array.

### Why use `$unwind`?
- When you want to work with individual elements of an array
- Before grouping or filtering on array values
- Very commonly used with `$lookup` results
- Useful for reporting and analytics

### Example

**Before `$unwind`:**
```json
{ "name": "Rahul", "skills": ["JS", "React", "Node"] }
```

**After `$unwind` on skills:**
```json
{ "name": "Rahul", "skills": "JS" }
{ "name": "Rahul", "skills": "React" }
{ "name": "Rahul", "skills": "Node" }
```

---

## 🗄️ Sample Collection: `students`

```json
[
  {
    "_id": 1,
    "name": "Rahul",
    "skills": ["JavaScript", "React", "Node.js"],
    "scores": [85, 90, 78]
  },
  {
    "_id": 2,
    "name": "Priya",
    "skills": ["Python", "SQL"],
    "scores": [92, 88, 95]
  },
  {
    "_id": 3,
    "name": "Amit",
    "skills": ["JavaScript", "MongoDB"],
    "scores": [70, 75]
  },
  {
    "_id": 4,
    "name": "Sneha",
    "skills": [],
    "scores": [80]
  }
]
```

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Basic $unwind on skills

```js
db.students.aggregate([
  { $unwind: "$skills" }
])
```

---

### 🔹 Task 2: Keep documents even if the array is empty

```js
db.students.aggregate([
  {
    $unwind: {
      path: "$skills",
      preserveNullAndEmptyArrays: true
    }
  }
])
```

---

### 🔹 Task 3: Count how many students know each skill

```js
db.students.aggregate([
  { $unwind: "$skills" },
  {
    $group: {
      _id: "$skills",
      studentCount: { $sum: 1 }
    }
  },
  { $sort: { studentCount: -1 } }
])
```

---

### 🔹 Task 4: Find average score of each student (unwind scores)

```js
db.students.aggregate([
  { $unwind: "$scores" },
  {
    $group: {
      _id: "$name",
      averageScore: { $avg: "$scores" },
      totalScores: { $sum: 1 }
    }
  }
])
```

---

### 🔹 Task 5: Get the highest score of each student

```js
db.students.aggregate([
  { $unwind: "$scores" },
  {
    $group: {
      _id: "$name",
      highestScore: { $max: "$scores" }
    }
  },
  { $sort: { highestScore: -1 } }
])
```

---

### 🔹 Task 6: List all unique skills across all students

```js
db.students.aggregate([
  { $unwind: "$skills" },
  {
    $group: {
      _id: null,
      allSkills: { $addToSet: "$skills" }
    }
  }
])
```

---

## 💡 Key Takeaways

- `$unwind` converts array elements into separate documents.
- Use `preserveNullAndEmptyArrays: true` if you want to keep documents with empty/missing arrays.
- Very commonly used after `$lookup` (because `$lookup` returns an array).
- Often combined with `$group` for analytics.
- Be careful: `$unwind` can significantly increase the number of documents.

---

## 🔥 Bonus Challenge

Write an aggregation that shows:
- Student name
- Each skill they know
- Their average score
- Only include students who have at least one skill
```
