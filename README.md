[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://claV# Create a new React app inside the repo folder
npm create vite@latest .  # (dot means current folder)

# Choose:
# - Project name: (leave as default)
# - Framework: React
# - Variant: JavaScript or TypeScript (depends on your preference)

# Install dependencies
npm install
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
@tailwind base;
@tailwind components;
@tailwind utilities;
src/
  components/
    Button.jsx
    Card.jsx
    Navbar.jsx
    Footer.jsx
  pages/
    Home.jsx
    Tasks.jsx
  layout/
    Layout.jsx
  hooks/
    useLocalStorage.js
  context/
    ThemeContext.jsx
npm install react-router-dom
import React from 'react'
import ReactDOM from 'react-dom/client'
import { BrowserRouter, Routes, Route } from 'react-router-dom'
import Home from './pages/Home'
import Tasks from './pages/Tasks'
import Layout from './layout/Layout'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')).render(
  <BrowserRouter>
    <Layout>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/tasks" element={<Tasks />} />
      </Routes>
    </Layout>
  </BrowserRouter>
)
export default function Button({ children, variant = "primary" }) {
  const base = "px-4 py-2 rounded text-white";
  const styles = {
    primary: `${base} bg-blue-500 hover:bg-blue-600`,
    secondary: `${base} bg-gray-500 hover:bg-gray-600`,
    danger: `${base} bg-red-500 hover:bg-red-600`
  };
  return <button className={styles[variant]}>{children}</button>;
}
import { useState, useEffect } from "react";
import useLocalStorage from "../hooks/useLocalStorage";

export default function TaskManager() {
  const [tasks, setTasks] = useLocalStorage("tasks", []);
  const [text, setText] = useState("");

  function addTask() {
    if (text.trim()) {
      setTasks([...tasks, { id: Date.now(), text, completed: false }]);
      setText("");
    }
  }

  function toggleTask(id) {
    setTasks(tasks.map(task =>
      task.id === id ? { ...task, completed: !task.completed } : task
    ));
  }

  function deleteTask(id) {
    setTasks(tasks.filter(task => task.id !== id));
  }

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={addTask}>Add</button>
      <ul>
        {tasks.map(task => (
          <li key={task.id}>
            <span
              className={task.completed ? "line-through" : ""}
              onClick={() => toggleTask(task.id)}
            >
              {task.text}
            </span>
            <button onClick={() => deleteTask(task.id)}>❌</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
npm install axios
