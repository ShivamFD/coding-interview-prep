# ⚛️ React: Tabs Component

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Tabs%20Component-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

A Tabs component is a very common UI pattern used to switch between different views without leaving the page.

This problem helps you practice:
- Managing active state
- Conditional rendering
- Clean component structure
- Passing data as props

---

## 🎯 Problem Statement

Create a `Tabs` component that:

1. Displays multiple tab titles
2. Shows the content of the currently active tab
3. Highlights the active tab
4. Allows switching tabs on click

---

## 💻 Solution

```jsx
import { useState } from "react";

const tabData = [
  {
    id: "home",
    label: "Home",
    content: "Welcome to the Home tab. This is the default view of the application."
  },
  {
    id: "profile",
    label: "Profile",
    content: "This is your Profile tab. You can view and edit your personal information here."
  },
  {
    id: "settings",
    label: "Settings",
    content: "Manage your account settings, notifications, and preferences in this tab."
  },
  {
    id: "about",
    label: "About",
    content: "Learn more about this application and the technologies used to build it."
  }
];

function Tabs() {
  const [activeTab, setActiveTab] = useState(tabData[0].id);

  const activeContent = tabData.find((tab) => tab.id === activeTab)?.content;

  return (
    <div style={{ maxWidth: "600px", margin: "40px auto", fontFamily: "sans-serif" }}>
      <h2>Tabs Component</h2>

      {/* Tab Headers */}
      <div style={{ display: "flex", borderBottom: "2px solid #ddd", marginBottom: "20px" }}>
        {tabData.map((tab) => {
          const isActive = activeTab === tab.id;

          return (
            <button
              key={tab.id}
              onClick={() => setActiveTab(tab.id)}
              style={{
                padding: "12px 20px",
                border: "none",
                background: "transparent",
                cursor: "pointer",
                fontSize: "16px",
                fontWeight: isActive ? "600" : "400",
                color: isActive ? "#2563eb" : "#555",
                borderBottom: isActive ? "3px solid #2563eb" : "3px solid transparent",
                marginBottom: "-2px"
              }}
            >
              {tab.label}
            </button>
          );
        })}
      </div>

      {/* Tab Content */}
      <div style={{ padding: "15px", lineHeight: "1.6", color: "#333" }}>
        {activeContent}
      </div>
    </div>
  );
}

export default Tabs;
```

---

## 🧠 Explanation

1. We store the `id` of the currently active tab in state.
2. When a tab header is clicked, we update `activeTab`.
3. We find the matching content using `.find()`.
4. The active tab is highlighted using conditional styling.
5. This pattern is clean and easy to extend.

---

## 💡 Key Interview Points

- How would you make this component reusable (accept tabs as props)?
- How can you support controlled + uncontrolled modes?
- How would you add animation when switching tabs?
- Accessibility: how do you handle keyboard navigation?
- Difference between keeping content mounted vs unmounting inactive tabs

---

## 🔥 Bonus Challenges

1. Convert this into a reusable component that accepts a `tabs` prop
2. Keep all tab contents mounted but hide inactive ones with CSS
3. Add previous / next buttons to move between tabs
4. Add icons next to the tab labels
```
