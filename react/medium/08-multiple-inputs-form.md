# ⚛️ React: Handling Multiple Inputs in a Form

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Multiple%20Inputs%20Form-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

When a form has many fields, creating a separate `useState` for each field becomes messy.

The better approach is to use **a single state object** and update it dynamically using the input's `name` attribute.

This is a very common and clean pattern in React.

---

## 🎯 Problem Statement

Create a form with the following fields:

- Full Name
- Email
- Phone
- City
- Message

Use a single state object to manage all inputs.  
On submit, show the collected data.

---

## 💻 Solution

```jsx
import { useState } from "react";

function ContactForm() {
  const [formData, setFormData] = useState({
    fullName: "",
    email: "",
    phone: "",
    city: "",
    message: ""
  });

  const handleChange = (e) => {
    const { name, value } = e.target;

    setFormData((prev) => ({
      ...prev,
      [name]: value
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("Form Submitted:", formData);
    alert(JSON.stringify(formData, null, 2));
  };

  return (
    <div style={{ maxWidth: "500px", margin: "40px auto" }}>
      <h2>Contact Form</h2>

      <form onSubmit={handleSubmit}>
        <div style={{ marginBottom: "12px" }}>
          <label>Full Name</label>
          <input
            type="text"
            name="fullName"
            value={formData.fullName}
            onChange={handleChange}
            style={{ display: "block", width: "100%", padding: "8px" }}
          />
        </div>

        <div style={{ marginBottom: "12px" }}>
          <label>Email</label>
          <input
            type="email"
            name="email"
            value={formData.email}
            onChange={handleChange}
            style={{ display: "block", width: "100%", padding: "8px" }}
          />
        </div>

        <div style={{ marginBottom: "12px" }}>
          <label>Phone</label>
          <input
            type="text"
            name="phone"
            value={formData.phone}
            onChange={handleChange}
            style={{ display: "block", width: "100%", padding: "8px" }}
          />
        </div>

        <div style={{ marginBottom: "12px" }}>
          <label>City</label>
          <input
            type="text"
            name="city"
            value={formData.city}
            onChange={handleChange}
            style={{ display: "block", width: "100%", padding: "8px" }}
          />
        </div>

        <div style={{ marginBottom: "12px" }}>
          <label>Message</label>
          <textarea
            name="message"
            value={formData.message}
            onChange={handleChange}
            rows="4"
            style={{ display: "block", width: "100%", padding: "8px" }}
          />
        </div>

        <button type="submit">Submit</button>
      </form>
    </div>
  );
}

export default ContactForm;
```

---

## 🧠 Explanation

- We store all form fields in a single object: `formData`.
- Every input has a `name` attribute that matches the key in the state object.
- In `handleChange`, we dynamically update the correct field using computed property names: `[name]: value`.
- This pattern scales very well even if the form has 10–15 fields.

---

## 💡 Key Interview Points

- Why is a single state object better than multiple `useState`?
- What are computed property names?
- Difference between controlled and uncontrolled inputs
- How would you reset the form after submission?
- How would you add validation to this pattern?

---

## 🔥 Bonus Challenges

1. Add a reset button that clears all fields
2. Disable the submit button if any required field is empty
3. Convert this into a reusable custom hook (`useForm`)
```
