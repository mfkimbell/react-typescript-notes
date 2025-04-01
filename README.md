# react-typescript-notes


## Twilio React Notes

### What is NodeJS?

Node.js was created to run JavaScript on the server using an event-driven, non-blocking model powered by async/await, enabling lightweight and scalable apps. Unlike traditional runtimes that use multi-threaded, blocking I/O, Node.js handles many connections efficiently with a single thread using asynchronous code.

The event loop is the core mechanism in Node.js that coordinates asynchronous (non-blocking) operations, such as file I/O, network requests, and timers, on a single JavaScript thread.

<img width="739" alt="Screenshot 2025-03-31 at 8 38 10 PM" src="https://github.com/user-attachments/assets/f811c3ab-5b22-4a72-b642-9d7e3cf34fc5" />

In simpler terms:

* Node.js runs JavaScript on one thread.

* When an async task is triggered (e.g., reading a file, querying a database, and other I/O operations), Node offloads that task to a separate threadpool it keeps for IO tasks, freeing the main thread to keep doing other work.
  
Yes! 🙌 You're absolutely right — these days `we prefer async/await because it's cleaner and easier to read than callbacks`. But under the hood, both are doing async non-blocking I/O.

* Once the operation completes, a callback (or Promise resolution) is queued, and the event loop eventually picks it up and runs it.

Async and Await is the more modern way to handle promises:

<img width="793" alt="Screenshot 2025-03-31 at 8 28 43 PM" src="https://github.com/user-attachments/assets/cacbddd1-5b95-45d9-ad96-8f47433dcdd5" />

<img width="797" alt="Screenshot 2025-03-31 at 8 25 37 PM" src="https://github.com/user-attachments/assets/213b7382-3fab-43ee-af8f-8f5c0936fb13" />

<img width="796" alt="Screenshot 2025-03-31 at 8 29 29 PM" src="https://github.com/user-attachments/assets/b2f8ba3c-a197-4cd8-9b81-62448ec089b7" />

#### NextJS vs React

Great question — this one comes up *a lot* in interviews and real-world projects.

Let’s break it down clearly:

---

## ⚛️ React vs. 🔼 Next.js

| Concept               | **React**                           | **Next.js**                          |
|------------------------|--------------------------------------|--------------------------------------|
| What it is             | A **JavaScript library** for building UIs | A **framework** built on top of React |
| Routing                | Manual (via `react-router-dom`)      | Built-in file-based routing           |
| Server-side rendering  | Not built-in (needs custom setup)    | ✅ Built-in SSR (easy to use)         |
| Static site generation | Manual (needs tooling)               | ✅ Built-in (`getStaticProps`)        |
| Full-stack features    | ❌ Frontend only                     | ✅ Supports APIs (`/api` folder)      |
| Configuration          | Up to you                            | Sensible defaults out of the box      |

---

### ⚛️ **React (alone)**

React is mainly the **V (view)** in MVC. It handles **components, state, and rendering**, but leaves things like:
- Routing
- SSR
- API calls
- Optimization

…up to **you**.

#### 🔁 Routing in React:
You use a package like `react-router-dom`:
```jsx
<Route path="/about" element={<About />} />
```

#### 🧠 Server-side rendering (SSR)?
- Not included out of the box
- Needs custom setup with something like Next.js, Express, or Remix

---

### 🔼 **Next.js (React with superpowers)**

Next.js is a **React framework** that adds:
- **Routing**
- **SSR**
- **Static site generation (SSG)**
- **API routes**
- **Image optimization**
- **File-based pages**


#### 🧭 Routing in Next.js:
Just create files in the `/pages` directory — that's it!

```bash
/pages/index.tsx      -->  "/"
/pages/about.tsx      -->  "/about"
/pages/blog/[id].tsx  -->  "/blog/:id"
```

✅ No need for `react-router-dom` — it's automatic.

---

### 🖥️ Server-Side Rendering in Next.js:

SERVER SENDS FULLY RENDERED HTML TO THE BROWSWER (instead of the browser generating the html) so it's FASTER

Next.js makes SSR super easy:

```tsx
export async function getServerSideProps() {
  const res = await fetch('https://api.com/data');
  const data = await res.json();

  return {
    props: { data },
  };
}
```

- Runs **on the server** at request time
- Useful for dynamic, up-to-date content (like dashboards)

---

### 🧊 Static Site Generation (SSG) in Next.js:

```tsx
export async function getStaticProps() {
  const res = await fetch('https://api.com/posts');
  const data = await res.json();

  return {
    props: { data },
  };
}
```

- Runs **at build time**
- Great for performance — pages are pre-rendered and fast

---

### ✅ TL;DR:

| Feature                     | React                  | Next.js                             |
|-----------------------------|-------------------------|--------------------------------------|
| Routing                     | Manual w/ `react-router`| ✅ Built-in file-based routing        |
| SSR (Server-side rendering) | ❌ Manual setup          | ✅ Easy with `getServerSideProps`     |
| SSG                         | ❌ Manual                | ✅ Easy with `getStaticProps`         |
| APIs                        | ❌ External only         | ✅ Can create `/api/hello.ts` etc.    |
| Best for                    | Custom frontends        | Full-stack apps, SEO, performance    |

---

## Why do we care about a server side api?

An API route is just a special kind of function — one that runs on the server, can safely talk to databases or external services, and responds to HTTP requests from the frontend (or any client).

It feels like a function — but it’s part of how apps talk across the network securely and scalably.

## What is Static Site Generation

SSR vs. SSG:

SSR is dynamic per request—always fresh, but more server load.

SSG is pre-built—very fast to serve, but data can be stale unless you rebuild. (data is stored as a zip in an S3 bucket usually)

<img width="749" alt="Screenshot 2025-03-31 at 6 55 36 PM" src="https://github.com/user-attachments/assets/8e2d0703-ed68-4327-b3f5-c7ce4ef12f6a" />

If you use a static site, you CANT do server side rendering because you no longer have a Node Server attached to it, it's JUST HTML AND REACT. 

<img width="734" alt="Screenshot 2025-03-31 at 7 02 55 PM" src="https://github.com/user-attachments/assets/bd6e0425-421d-4433-b312-3adec69abc79" />

A “static site” just means no dynamic, server-side rendering at request time.

<img width="779" alt="Screenshot 2025-03-31 at 7 03 49 PM" src="https://github.com/user-attachments/assets/bdab0379-b677-4a9a-82e2-e285c6b5cca9" />

<img width="793" alt="Screenshot 2025-03-31 at 7 12 33 PM" src="https://github.com/user-attachments/assets/27b6f711-b58f-46b4-adda-dacebdaf3c10" />

So, even with a static site, you can still have a dynamic user experience after the page loads. The difference is that the server didn’t generate that new HTML on demand – the browser did it via React.

The difference between a REGULAR REACT SITE and Static Site Geneartion is that HTML IS ALREADY BUILT WITH SOME DATA 

## 🧠 JavaScript Refresher for React

---

### 🔁 1. Array Methods – Must Know in React

These are essential when working with dynamic lists or state updates:

#### `.map()`
Use to **render lists**:
```jsx
{items.map(item => (
  <li key={item.id}>{item.name}</li>
))}
```

#### `.filter()`
Use to **conditionally exclude** elements:
```js
const visibleItems = items.filter(item => item.visible);
```

#### `.forEach()`
Use for **side effects** (like logging), not rendering:
```js
items.forEach(item => console.log(item));
```

#### `.reduce()`
Use to **calculate totals** or transform arrays:
```js
const total = items.reduce((sum, item) => sum + item.price, 0);
```

---

### 🔄 2. Async JavaScript

#### ✅ Callbacks (OLD AND OUTDATED ASYNC AND AWAIT IS MODERN)
```js
setTimeout(() => console.log("Hello"), 1000);
```

#### ✅ Promises (SAME HERE ASYNC AND AWAIT IS EASIER)
```js
fetch(url)
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

#### ✅ Async/Await
```js
async function getData() {
  const res = await fetch(url);
  const data = await res.json();
  return data;
}
```

---

### 🔥 3. Promises in `useEffect`

Use `async` functions inside `useEffect` for data fetching:
```js
useEffect(() => {
  async function fetchData() {
    const res = await fetch('/api/data');
    const json = await res.json();
    setData(json);
  }
  fetchData();
}, []);
```

Things to understand:
- `fetch()`
- `.then()` / `.catch()`
- `try` / `catch` inside `async` functions

---

### 🧩 4. Destructuring

#### Object:
```js
const { name } = person;
```

#### Array:
```js
const [first, second] = arr;
```

---

### 📦 5. Spread / Rest

#### Spread:
```js
const newArr = [...oldArr, 4];
```

#### Rest:
```js
const sum = (...nums) => nums.reduce((a, b) => a + b);
```

---

### ⚖️ 6. Type Coercion: `==` vs `===`

Always use `===` in React to avoid unexpected behavior:
```jsx
{count == '0'}   // ❌ unpredictable
{count === 0}    // ✅ clean and safe
```

---

Absolutely! Here's the full list again — **with an improved, clearer explanation for #2 (the React timer)** — and still **excluding** the Node prompt.

---

## ✅ React/JavaScript Interview Questions & Answers

---

### 🔹 **1. Implement a React service call to an API**

**🧠 Question:**
> How would you fetch data from an API in a React component?

**✅ Answer:**
Use `useEffect` to trigger the fetch when the component mounts, and `useState` to store the result:

```tsx
import { useEffect, useState } from 'react';

function UsersList() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    async function fetchUsers() {
      const res = await fetch('/api/users');
      const data = await res.json();
      setUsers(data);
    }

    fetchUsers();
  }, []);

  return (
    <ul>
      {users.map(user => <li key={user.id}>{user.name}</li>)}
    </ul>
  );
}
```

---

### 🔹 **2. UI Prompt in React to create a decrementing timer**

**🧠 Question:**
> How do you implement a countdown timer in React that starts when a button is clicked?

**✅ Answer:**
Use `useState` to track the remaining time and whether the timer is running, and `useEffect` with `setInterval` to count down every second.

```tsx
import { useEffect, useState } from 'react';

function CountdownTimer() {
  const [time, setTime] = useState(10);       // Starting time in seconds
  const [running, setRunning] = useState(false); // Tracks if the timer is active

  useEffect(() => {
    if (!running) return; // Don’t run if timer hasn't started

    const interval = setInterval(() => {
      setTime(prev => {
        if (prev <= 1) {
          clearInterval(interval); // Stop at 0
          return 0;
        }
        return prev - 1; // Decrease time by 1 every second
      });
    }, 1000);

    // Cleanup to avoid memory leaks
    return () => clearInterval(interval);
  }, [running]);

  return (
    <div>
      <h2>Time left: {time}</h2>
      <button onClick={() => setRunning(true)}>Start</button>
    </div>
  );
}
```
the `return` in the useEffect is a react default CLEANUP FUNCTION, so that's whhy the clearInterval only runs when it unmounts
the dependency array decides whether the cleanup function is run. so it'll run if "running" changes. 

---<img width="778" alt="Screenshot 2025-03-28 at 2 23 29 PM" src="https://github.com/user-attachments/assets/0700f89f-a091-4203-8b8d-af375e811101" />


### 🧠 Explanation of how it works:

- `time` state holds the countdown value.
- `running` state tracks whether the countdown has started.
- `useEffect` runs when `running` becomes `true`.
- `setInterval()` is used to tick the countdown every second.
- When the timer reaches 0, the interval is cleared to stop counting.
- We also clear the interval if the component unmounts or `running` changes — this prevents memory leaks.

---

### 🔹 **3. Consume endpoints and process data (filter/sort)**

**🧠 Question:**
> After fetching data from an API, how would you filter and sort it before displaying?

**✅ Answer:**
```tsx
import { useEffect, useState } from 'react';

function SortedUsers() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    async function fetchUsers() {
      const res = await fetch('/api/users');
      const data = await res.json();

      // Keep only active users
      const filtered = data.filter(user => user.active);

      // Sort them alphabetically by name
      const sorted = filtered.sort((a, b) => a.name.localeCompare(b.name));

      setUsers(sorted);
    }

    fetchUsers();
  }, []);

  return (
    <ul>
      {users.map(user => <li key={user.id}>{user.name}</li>)}
    </ul>
  );
}
```
<img width="623" alt="Screenshot 2025-03-28 at 2 24 55 PM" src="https://github.com/user-attachments/assets/6b7d566a-b30b-40ec-bcaa-52c7330b2d0e" />

---

## 🧠 Summary Table

| Concept                                   | Key Skills Tested                         | Tools/Concepts                       |
|------------------------------------------|-------------------------------------------|--------------------------------------|
| API Call in React                        | `useEffect`, `fetch`, `useState`          | React, async/await                   |
| Countdown Timer                          | React state, intervals, side-effects      | `useState`, `useEffect`, `setInterval` |
| Consume + Process API Data (filter/sort) | Array methods, state updates, rendering   | `.filter()`, `.sort()`, JSX          |

---

Let me know if you want to add search input, pagination, or TypeScript interfaces next!


#### How to cause an infinite loop in React?
<img width="763" alt="Screenshot 2025-03-27 at 7 49 36 PM" src="https://github.com/user-attachments/assets/22af58c7-87c8-42da-9b6f-9f8ebac826bb" />
Use effect with run on the first render and upon state change, we add the array of dependencies [] after to control which state causes it to run.

#### UseContext is for sharing Data without props!!!

<img width="1148" alt="Screenshot 2024-08-09 at 4 39 37 PM" src="https://github.com/user-attachments/assets/a6ca452e-d66d-4d9d-8250-679806954df2">
<img width="1243" alt="Screenshot 2024-08-09 at 4 40 16 PM" src="https://github.com/user-attachments/assets/08a76e30-ffbe-49f9-a0fd-ad699eb32064">
<img width="1258" alt="Screenshot 2024-08-09 at 4 42 27 PM" src="https://github.com/user-attachments/assets/9cb7a23c-6a1d-490a-84e7-4938ffa1612b">

<img width="808" alt="Screenshot 2025-03-27 at 7 39 36 PM" src="https://github.com/user-attachments/assets/d662d17d-e9b8-40aa-9eaa-0471344dc7aa" />

# this video is good, REWATCH IT, its SHORT
https://www.youtube.com/watch?v=wIyHSOugGGw


# React

## Children
* passing react components or JSX elements in props
<img width="766" alt="Screenshot 2024-08-09 at 4 37 52 PM" src="https://github.com/user-attachments/assets/0d07a4d2-8ef5-4232-8740-80aaf3b9f659">

## Keys
<img width="811" alt="Screenshot 2024-08-09 at 4 38 46 PM" src="https://github.com/user-attachments/assets/63356f19-7b8e-4d9c-9664-85f2d10d18fd">

* Keys are used to identify components in lists, they can be unique strings or numbers
* 

## Class and Function based components
* Function based components are becoming more popular and are considered the better option.

## JSX (Javascript XML)

Map v.s. ForEach 
* Map alters and returns a new array
* ForEach just goes though and performs an action, doesn't return a new array

### Subtle differences to html
* class declarations are `className` instead of `class`
* Inline styles are passed as an object
``` Javascript
<div style={{ color: 'red', fontSize: '20px' }}>Styled Text</div>
```
* broswers can't read JSX, so it's run through a compliler to HTML and Javascript when it's output in the broswer

### React Router
* Used to output different pages of the React application via different routes
<img width="934" alt="Screenshot 2024-07-07 at 1 53 11 PM" src="https://github.com/mfkimbell/react-typescript-notes/assets/107063397/bc6d66ad-b3ba-4cf4-b59c-b34eb477f3bb">


### Props
* passing props down through multiple layers is called prop drilling
<img width="998" alt="Screenshot 2024-07-07 at 1 54 22 PM" src="https://github.com/mfkimbell/react-typescript-notes/assets/107063397/356d6619-5b3d-4b30-9d18-9f18b0479761">

### Hooks

## Very useful video about hoooks
https://www.youtube.com/watch?v=TNhaISOUy6Q

React Hooks are functions that allow you to use state and other React framework specific features without writing a **Class** component. They were introduced in React 16.8 to enable state and side-effect management in functional components, offering a more concise and expressive way to build components compared to class-based components.

#### **useEffect**

* Purpose: Handles side effects such as data fetching, subscriptions, or manually changing the DOM. It runs after the component renders.
* Syntax: useEffect(() => { /* side effect */ }, [dependencies]);
  
Here are some component lifecycle side effects that can happen:

<img width="789" alt="Screenshot 2024-09-06 at 2 48 12 PM" src="https://github.com/user-attachments/assets/2d39cdd5-9190-4f76-83bb-8081a82fe3ca">

This is an example of it running our function (our side effect) on first render (initialized) and whenever stateful data changes:

<img width="1047" alt="Screenshot 2024-09-06 at 2 49 19 PM" src="https://github.com/user-attachments/assets/6a7425d9-d343-45d8-bcbd-b7477feb43ee">

This would cause an infinite loop, cause it reruns after it alters the stateful data:

<img width="996" alt="Screenshot 2024-09-06 at 2 51 22 PM" src="https://github.com/user-attachments/assets/75227ba9-5d26-4d3e-ac54-51b8381ee590">

This is the fix, no dependencies means it'll only run once when the component is initialized:

<img width="652" alt="Screenshot 2024-09-06 at 2 53 25 PM" src="https://github.com/user-attachments/assets/3a597f12-7ccd-41a7-a121-e6f85b6816ed">

Now it will run when "count" changes:

<img width="790" alt="Screenshot 2024-09-06 at 3 07 31 PM" src="https://github.com/user-attachments/assets/16563ea4-a1e9-43c1-aa31-a85dc648144b">

### **useContext()**

<img width="788" alt="Screenshot 2024-09-06 at 3 08 35 PM" src="https://github.com/user-attachments/assets/3da6609c-0030-4ebb-adeb-f50459290816">

<img width="993" alt="Screenshot 2024-09-06 at 3 10 29 PM" src="https://github.com/user-attachments/assets/50c6838b-707d-4dc2-a24f-3d542fc77aac">

So basically, we have a context provider, and all of the child classes within that component can use that parent context to access data. When the data updates in the parent, it'll update all context data that the children are using. 

```javascript
import React, { useContext } from 'react';

// Step 1: Define a Context
interface ThemeContextType {
  theme: 'light' | 'dark';
  toggleTheme: () => void;
}

const ThemeContext = React.createContext<ThemeContextType | undefined>(undefined);

// Step 2: Create a Provider Component
const ThemeProvider: React.FC = ({ children }) => {
  const [theme, setTheme] = React.useState<'light' | 'dark'>('light');

  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light');
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// Step 3: Use the useContext Hook
const ThemeToggle: React.FC = () => {
  const themeContext = useContext(ThemeContext);

  if (!themeContext) {
    throw new Error('useContext must be used within a ThemeProvider');
  }

  return (
    <button onClick={themeContext.toggleTheme}>
      Toggle Theme ({themeContext.theme})
    </button>
  );
};

// Step 4: Use the Provider and Consumer Components in your App
const App: React.FC = () => {
  return (
    <ThemeProvider>
      <div>
        <h1>Theme Toggle Example</h1>
        <ThemeToggle />
      </div>
    </ThemeProvider>
  );
};

export default App;
```
#### **useState**

* sets the state

#### **useRef()**

<img width="875" alt="Screenshot 2024-09-06 at 3 29 42 PM" src="https://github.com/user-attachments/assets/257d92e1-8830-46ed-8f54-5186e623fdbe">

Count would not change in the UI


More Typical use is to grab elements from the DOM:

<img width="947" alt="Screenshot 2024-09-06 at 3 30 31 PM" src="https://github.com/user-attachments/assets/1422783a-b2fd-40dd-ab8e-881e0dcd81f3">

Here we can "programatically" click the button: (maybe for unit tests)

<img width="768" alt="Screenshot 2024-09-06 at 3 30 43 PM" src="https://github.com/user-attachments/assets/38c92cfa-e20e-41c2-a545-38e5a59ad24c">

### useReducer()

<img width="517" alt="Screenshot 2024-09-06 at 3 31 25 PM" src="https://github.com/user-attachments/assets/8bf2e480-ca8b-4243-959b-7061d5fd75f1">

<img width="1015" alt="Screenshot 2024-09-06 at 3 31 50 PM" src="https://github.com/user-attachments/assets/0bd6544e-44db-4a3b-9666-c784379c5fd8">

<img width="506" alt="Screenshot 2024-09-06 at 3 32 09 PM" src="https://github.com/user-attachments/assets/f9a97f80-2e56-4bc6-b2de-5e9d873143ff">

people say this is useful as your program gets more complex, others disagree

### useMemo()
<img width="938" alt="Screenshot 2024-09-06 at 3 42 05 PM" src="https://github.com/user-attachments/assets/9278d37a-a1fc-43a4-9838-9ddf52b98e9d">
* use only as needed for expensive calculations
* instead of recomputing every render, we recompute every time a specific dependency changes

### useCallback()
* instead of memoizing return values, we memoize an entire function
<img width="821" alt="Screenshot 2024-09-06 at 3 43 26 PM" src="https://github.com/user-attachments/assets/3e14a036-4b9d-4ef2-88b4-aa1409c82e6c">
<img width="732" alt="Screenshot 2024-09-06 at 3 45 20 PM" src="https://github.com/user-attachments/assets/71569420-0d75-4082-8936-8e7fa07f141b">
* when you define a function, a new function object is created every time it's rendered
* therefore a good usecase is when we pass a component down to multiple children
* 
### React with Typescript
Typing props
<img width="465" alt="Screenshot 2024-08-09 at 12 54 48 PM" src="https://github.com/user-attachments/assets/78fbcc33-bc22-4dd1-afd3-f319e79cd760">

<img width="775" alt="Screenshot 2024-08-09 at 3 36 55 PM" src="https://github.com/user-attachments/assets/7bcda5ad-aee2-4790-8b5d-02e5e791c0b0">
<img width="799" alt="Screenshot 2024-08-09 at 3 37 30 PM" src="https://github.com/user-attachments/assets/81d799d7-c4d3-4b2c-a9a5-5b84d215b510">
<img width="740" alt="Screenshot 2024-08-09 at 3 37 52 PM" src="https://github.com/user-attachments/assets/c3ddac77-241b-446b-84a8-cc55f1fc5bbb">

### Here’s a concise difference between interface and type in TypeScript:

* Use interface when you need to define the shape of an object, especially if you plan to extend it or use it with classes.
* Use type when you need to define more complex types, like unions, intersections, or when you need to alias primitive types or other types.
  
In short:

* interface = Best for objects and when you need to extend or implement, **makes more sense when there are functions involved.** 
* type = Best for flexibility, including complex or composite types, **you also wouldn't include functions.**

Typescript basics

<img width="750" alt="Screenshot 2024-08-09 at 1 00 12 PM" src="https://github.com/user-attachments/assets/444d5868-6fce-4c72-aae9-e88f3cef6fe0">
<img width="737" alt="Screenshot 2024-08-09 at 1 46 35 PM" src="https://github.com/user-attachments/assets/1e7ad45c-b9de-481e-abe3-2fa2318a9705">
<img width="715" alt="Screenshot 2024-08-09 at 1 47 29 PM" src="https://github.com/user-attachments/assets/ff058fa5-51c9-4477-bba0-e3934d1892e8">
<img width="684" alt="Screenshot 2024-08-09 at 1 47 43 PM" src="https://github.com/user-attachments/assets/93afe168-12ac-4b0e-a7f1-37595b44d1fe">

<img width="812" alt="Screenshot 2024-08-09 at 1 50 33 PM" src="https://github.com/user-attachments/assets/01a856fa-ba95-411c-836f-f154976b51f1">
<img width="812" alt="Screenshot 2024-08-09 at 1 51 36 PM" src="https://github.com/user-attachments/assets/3ac9e230-7248-47ef-a0d5-07f747583d14">
<img width="726" alt="Screenshot 2024-08-09 at 1 57 04 PM" src="https://github.com/user-attachments/assets/fa5b0d59-2c01-4fbf-8483-d6a087307d80">
<img width="813" alt="Screenshot 2024-08-09 at 1 58 44 PM" src="https://github.com/user-attachments/assets/3c91fd2f-8c70-45d7-b138-6d11c76fe53c">
<img width="764" alt="Screenshot 2024-08-09 at 2 00 16 PM" src="https://github.com/user-attachments/assets/6259fa0c-f574-43a7-9990-5f3d8890d4b5">

```
// Define a generic interface ListProps with a type parameter T
interface ListProps<T> {
  items: T[]; // An array of items of type T
  renderItem: (item: T) => React.ReactNode; // A function that takes an item of type T and returns a React node
}

// Define a generic functional component List with a type parameter T
function List<T>({ items, renderItem }: ListProps<T>) {
  return <ul>{items.map(renderItem)}</ul>; // Render the items using the renderItem function
}

// Use the List component with a string type
const stringList = <List items={['A', 'B', 'C']} renderItem={(item) => <li>{item}</li>} />;

```
We defined a function in the props as well as what the funciton should return. 


```
import React, { useState, useEffect } from 'react';

function FetchData() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => setData(data));
  }, []); // Empty array means this effect runs once after the initial render

  return (
    <div>
      {data ? <pre>{JSON.stringify(data, null, 2)}</pre> : 'Loading...'}
    </div>
  );
}
```
This makes sense because it'll show "Loading..." Until "setData()" is called, then it'll re-render the component. 

#### **useContext**

* Purpose: Accesses the context value from a Context Provider. This hook helps in managing global state or passing data through the component tree without prop drilling.
* Syntax: const contextValue = useContext(MyContext);
* Kinda like global state, can be used to avoid prop drilling
``` Typescript
import React, { createContext, useContext } from 'react';

const ThemeContext = createContext('light');

function ThemedComponent() {
  const theme = useContext(ThemeContext);

  return <div style={{ background: theme === 'dark' ? 'black' : 'white' }}>Themed Component</div>;
}

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <ThemedComponent />
    </ThemeContext.Provider>
  );
}
```

# NodeJS

## Runtimes
* Javascript's runtime is (usually) NodeJS. 
* Python's runtime is (usually) CPython. It's an interpreted lanaguage. Uses garbage collection for memory management. 
* C#'s runtime is (usually) .NET. It uses JIT compliation. Uses garbage collection for memory management.
* GoLang has it's own runtime. Go is a compiled language, meaning that Go source code is converted into machine code that can be executed directly by the operating system Uses garbage collection for memory management.
## Rust
* Rust's runtime is built into the language. It's a compiled language.
* Memory Management: Rust uses a unique approach to memory management that does not involve garbage collection. Instead, it relies on a system of ownership, borrowing, and lifetimes to manage memory safely and efficiently. This system ensures that memory is allocated and deallocated correctly at compile time without the need for a runtime garbage collector.

Ownership: Each value in Rust has a single owner, and ownership can be transferred between variables. When the owner of a value goes out of scope, the value is automatically deallocated.
Borrowing: References to values can be borrowed, either mutably or immutably, but Rust enforces rules to prevent data races and ensure safety.
Lifetimes: Rust tracks how long references to values are valid, ensuring that references do not outlive the data they point to.
This approach eliminates the overhead associated with garbage collection and enables Rust to achieve high performance and safety without sacrificing control over memory.

In the context of Node.js, the runtime environment includes:
* V8 Engine: The JavaScript engine that compiles and executes the JavaScript code.
* Standard Library: A set of built-in modules providing utilities like file system access, networking, and other fundamental functions.
* Asynchronous I/O: Non-blocking I/O operations that allow Node.js to handle multiple operations concurrently.
* Event Loop: A loop that continuously checks for and executes events or tasks, enabling asynchronous programming.

# Java/Typescript

* JavaScript: Primarily interpreted with JIT compilation at runtime by JavaScript engines. (JIT compilation involves converting code into machine code at runtime, rather than before execution (as in Ahead-Of-Time, or AOT, compilation) or interpreting line by line. This allows the program to benefit from both the speed of compiled code and the flexibility of interpreted code.
* TypeScript: Compiled (transpiled) into JavaScript, which is then executed by JavaScript engines.)

Node.js enables JavaScript to be run on the server-side, outside of a browser, allowing developers to build backend applications, server scripts, and more using JavaScript.
