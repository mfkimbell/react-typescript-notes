# react-typescript-notes

## NextJS updates

Absolutely — here's the **concise version**:

---

### ✅ In Next.js 13+ (`/app` directory):

- **Server Components are default** – every file is a Server Component unless marked otherwise.
- To make a component run in the browser (and use things like `useState`, `useEffect`, `window`, etc.), you must add:

```tsx
'use client';
```

---

### 🧠 Summary:

| Component Type     | Default in `/app` | Needs `'use client'` | Can use `useEffect`, `useState` | Runs on Server |
|--------------------|-------------------|-----------------------|-------------------------------|----------------|
| Server Component   | ✅ Yes            | ❌ No                 | ❌ No                          | ✅ Yes         |
| Client Component   | ❌ No             | ✅ Yes                | ✅ Yes                         | ❌ No          |

---

### ✅ When to use `'use client'`

Use `'use client'` when:
- You need **interactivity**
- You’re using `useState`, `useEffect`, `useContext`, etc.
- You rely on **browser-only APIs** like `localStorage`, `document`, `window`, etc.



---

### 🤔 Wait… If `useState` is so common, why isn’t `'use client'` the default?

Because **Server Components don’t run in the browser** — they’re for **rendering HTML on the server**, not handling interactions like clicks, forms, etc.

---

### ✅ `'use client'` = make it interactive

You only need `'use client'` when you want **interactivity**, like:

- `useState` (local state)
- `useEffect` (side effects)
- `onClick`, `onChange`, etc.
- `localStorage`, `window`, `document`
- animations, modals, etc.

```tsx
'use client';

import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}
```

---

### 🔥 Why not make all components client?

Because **Server Components are faster, smaller, and better for SEO**.

- No JavaScript needed on the client
- Streams straight HTML to the browser
- Can fetch data server-side *in the component*

You only make something a client component if it needs to be interactive.

---

### TL;DR

| Want to show static data?   | ✅ Server Component (default) |
| Want to click things / update state? | 👉 `'use client'` |

**Yes — it really is that beneficial**, especially for **performance**, **SEO**, and **user experience**. Here's the quick breakdown:

---

### 🚀 Benefits of "no JavaScript needed on the client"

1. **⚡ Faster page load**  
   - No JS bundle = less to download, parse, and execute
   - HTML is ready-to-render → the browser can show it immediately

2. **📈 Better Lighthouse/Google scores**  
   - Lower JS = better scores for performance, SEO, and accessibility

3. **🧠 Instant time-to-content**  
   - Server-rendered HTML streams in fast — great for users on slow devices or networks

4. **🔍 SEO wins**  
   - Google prefers HTML it can read right away (no waiting for React to hydrate content)

5. **📉 Less JavaScript = less memory usage**  
   - Mobile devices especially benefit from fewer scripts and smaller memory footprints

6. **🧊 You can still hydrate interactivity**  
   - Add `'use client'` *only* to interactive bits — no need to ship React for your whole app

---

<img width="765" alt="Screenshot 2025-04-03 at 4 38 30 PM" src="https://github.com/user-attachments/assets/a87eeb41-459f-451a-b20b-2ec23226970e" />

### TL;DR

| Fewer client-side components = ✅ Less JS |
| Less JS = ✅ Faster load, better SEO, happier users |

These new `server components` really do make a difference:

<img width="758" alt="Screenshot 2025-04-03 at 4 39 40 PM" src="https://github.com/user-attachments/assets/785e5957-7c1a-4eb9-8fbe-28656b62b2fa" />


<img width="755" alt="Screenshot 2025-04-03 at 2 42 18 PM" src="https://github.com/user-attachments/assets/ca9f0e27-b628-467c-bf00-39e00ebfebf8" />

## Error handling (and specifics for NextJS SSR error handling)

(error boudnary is only for class components so i didnt spend much time thinking about it)

Absolutely — here’s a tighter version that includes both:

---

When you use **`getServerSideProps`**, data is loaded **before** your React component renders.

So if something fails during that process:

- 🛑 Your component never mounts
- 😬 You can’t use `useState`, `useEffect`, or client-side `try/catch`
- ✅ Next.js automatically shows the `_error.tsx` page

---

<img width="611" alt="Screenshot 2025-04-03 at 2 23 16 PM" src="https://github.com/user-attachments/assets/69092179-21c6-4547-9926-0f6fbafeeb95" />

### TL;DR  
Normal error handling (like `try/catch` + state) works **after** a component renders.  
For server-side failures, React never runs — so **Next.js catches the error** and shows `_error.tsx` instead.



**TL;DR:**  
Modern error handling uses try/catch in async code for client-side errors, while Next.js automatically shows your custom `_error.tsx` page for errors in server-side data fetching (like in getServerSideProps), ensuring a robust user experience without manual intervention.

## Uncontrolled component


> **Uncontrolled components grab data directly from the DOM**, using something like `useRef`, instead of storing it in React state.

---

### 🧠 Example:

```tsx
const inputRef = useRef();

<input ref={inputRef} />
```

When you submit:
```tsx
console.log(inputRef.current.value); // ✅ read from the DOM
```

So yes — **`useRef` is how you access the input's value** in an uncontrolled component. No `useState`, no `onChange`, just the DOM doing its thing.

You can also use `useRef` to simulate physical clicks on the screen

## Hooks

<img width="755" alt="Screenshot 2025-04-03 at 2 26 03 PM" src="https://github.com/user-attachments/assets/81d54eb9-8a6f-4ce0-a66d-ec7d8047bec6" />


Hooks let you manage state, lifecycle, and logic in React function components. They're a way to “hook into” React features like state (`useState`) or side effects (`useEffect`) without writing a class.

`useEffect` specifically handles things that happen **after render** — like interacting with the outside world. It's where you put **side effects**: things React doesn’t manage directly, like timeouts, APIs, or browser features. If you reuse that kind of logic across components, you’d move it into a custom hook.

---

### ✅ Common Side Effects with `useEffect` (and why)

**📡 Getting data from an API**  
React doesn’t fetch data for you — we do it after the component mounts.
```tsx
useEffect(() => {
  fetch('/api').then(res => res.json()).then(setData);
}, []);
```

---

**⏱️ Waiting with a timeout**  
Timers are outside React and need cleanup to avoid memory leaks.
```tsx
useEffect(() => {
  const id = setTimeout(() => setShow(true), 1000);
  return () => clearTimeout(id);
}, []);
```

---

**🔁 Repeating with an interval**  
Same idea — React doesn't track intervals, so you manage setup and cleanup.
```tsx
useEffect(() => {
  const id = setInterval(() => setCount(c => c + 1), 1000);
  return () => clearInterval(id);
}, []);
```

---

**📝 Changing the page title**  
Directly affects the DOM outside of React’s control.
```tsx
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);
```

---

**💾 Saving to localStorage**  
React doesn’t persist data — you sync it manually when needed.
```tsx
useEffect(() => {
  localStorage.setItem('theme', theme);
}, [theme]);
```

---

### ✅ TL;DR

Use `useEffect` for anything **external to React’s render system** — data fetching, timers, DOM updates, or browser APIs. It gives you full control over when and how these side effects run and clean up.

<img width="620" alt="Screenshot 2025-04-02 at 7 34 11 PM" src="https://github.com/user-attachments/assets/eeb0a11d-15f3-4871-95da-f4fab2b41f6f" />

<img width="527" alt="Screenshot 2025-04-02 at 7 34 36 PM" src="https://github.com/user-attachments/assets/72e566e2-8d32-47f7-992d-2f98538dae17" />

## Very useful video about hoooks
https://www.youtube.com/watch?v=TNhaISOUy6Q

React Hooks are functions that allow you to use state and other React framework specific features without writing a **Class** component. They were introduced in React 16.8 to enable state and side-effect management in functional components, offering a more concise and expressive way to build components compared to class-based components.

### **useEffect**

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


## When to split a component


### ✅ 1. **It's getting too large or doing too much**
- Hard to read, maintain, or test
- Has multiple responsibilities (e.g. rendering UI **and** managing complex logic)

> 🧠 A component should ideally “do one thing well”

---

### ✅ 2. **You want to reuse a part of it elsewhere**
- Same UI appears in multiple places
- You need to pass it different props in different contexts

> 🧠 Reusability = extract it into its own component


## State

state = data managed within component
props = data passed into a component

Absolutely — here's the version with **Recoil** instead of Zustand:

---

### ✅ `useState`  
Local state for a single component  
→ UI toggles, form inputs, modals

### 🌐 `Context`  
Share simple global data  
→ Theme, user, language — avoid prop drilling

### 🧱 External State (Redux, Recoil)  
Complex app-wide state  
→ Auth, carts, caching, cross-page or multi-tab sync

---

Let me know if you want a tiny decision table format for this too!

## Unit testing

<img width="739" alt="Screenshot 2025-04-02 at 7 29 43 PM" src="https://github.com/user-attachments/assets/efc0b334-67c0-46eb-aa28-26eef3129058" />

<img width="763" alt="Screenshot 2025-04-02 at 7 30 23 PM" src="https://github.com/user-attachments/assets/3de51435-b80b-4f61-9ffe-91b8453d125f" />

So if we were unit testing a button, we wouldn't care how it **renders in a form** we only care about it being rendered by itself, we only care about the logic in the button if it is WITHIN the component. 

<img width="473" alt="Screenshot 2025-04-02 at 7 31 26 PM" src="https://github.com/user-attachments/assets/4eec4f4f-8ed4-45b7-b777-c46d1e28e5c9" />

## Inheritance vs Composition

React prefers composition

<img width="791" alt="Screenshot 2025-04-02 at 7 26 52 PM" src="https://github.com/user-attachments/assets/8599e5c4-5669-4100-a80b-218cd32bb52f" />

## Code splitting

### ⚛️ **React (Manual Code Splitting)**

In plain React (like Create React App), you use `React.lazy()` + `Suspense` to split components manually:

```tsx
import React, { Suspense } from "react";

const HeavyComponent = React.lazy(() => import("./HeavyComponent"));

function App() {
  return (
    <div>
      <h1>Hello</h1>
      <Suspense fallback={<p>Loading...</p>}>
        <HeavyComponent />
      </Suspense>
    </div>
  );
}
```

✅ This delays loading `HeavyComponent` until it's actually rendered.

❌ Without this, **everything gets bundled into one big JS file**.

---

### 🧱 **Next.js (Automatic Page-Level Code Splitting)**

> In Next.js, **every file inside the `/pages` directory is automatically code-split**.  
> That means each route (like `/about`, `/dashboard`) loads only the JavaScript it needs.

You don’t have to do anything — it’s built-in.

For example:

```bash
/pages/index.tsx       → only loaded on homepage
/pages/dashboard.tsx   → only loaded when you visit /dashboard
```

✅ You get faster load times  
✅ No extra setup required  
✅ Each page is its own JS chunk  
✅ Add `dynamic()` for manual component-level splitting if needed

```tsx
import dynamic from "next/dynamic";

const HeavyComponent = dynamic(() => import("../components/HeavyComponent"), {
  loading: () => <p>Loading...</p>,
});
```

---

## ✅ TL;DR Summary

| Feature             | React (`CRA`)                        | Next.js                            |
|---------------------|--------------------------------------|-------------------------------------|
| Code splitting      | Manual with `React.lazy()` + `Suspense` | Automatic for each `pages/` file     |
| Component splitting | ✅ Manual with `lazy()`               | ✅ Optional with `dynamic()`         |
| Route-level chunks  | ❌ No                                | ✅ Yes                              |
| Optimization level  | Basic by default                    | Advanced, automatic, production-ready |

---

Let me know if you want a flashcard-style version or visual to memorize this fast!

## performance

React re-renders a component whenever its state or props change.
Any change to a component’s props or state will cause it to re-render.

Memoization means:
“If the inputs didn’t change, don’t recalculate — just reuse the last result.”

### React.memo

Only rerenders if the PROPS BEING PASSSED have changed

<img width="808" alt="Screenshot 2025-04-02 at 6 11 01 PM" src="https://github.com/user-attachments/assets/ae2703a6-6657-46d2-a5ca-986b82912f69" />

IF we are using React.memo, an **inline function will get recreated every render so it will *LOOK* like new data is being passed in even when it isn't**

<img width="835" alt="Screenshot 2025-04-02 at 6 14 41 PM" src="https://github.com/user-attachments/assets/f8494dc2-d2c7-45c2-8fa1-00f15579e68c" />


### useMemo()

```javascript
const sortedData = useMemo(() => {
  return bigData.sort((a, b) => a.value - b.value);
}, [bigData]);
```
Without useMemo, this sort runs every render, even if bigData didn't change.



## Recoil (global state manager)

### 🧱 `userAtom.ts` — define global state

```ts
import { atom } from "recoil";

export const userAtom = atom({
  key: "userAtom",
  default: null,
});
```

---

### 🧪 `Profile.tsx` — use it like local state

```tsx
import { useRecoilState } from "recoil";
import { userAtom } from "./userAtom";

export default function Profile() {
  const [user, setUser] = useRecoilState(userAtom);

  return (
    <div>
      <p>Hi {user?.name || "Guest"}</p>
      <button onClick={() => setUser({ name: "Alice" })}>Set User</button>
      <button onClick={() => setUser(null)}>Clear User</button>
    </div>
  );
}
```

---

### ✅ TL;DR

| What Recoil Gives You      | Redux Equivalent           |
|----------------------------|----------------------------|
| `atom()`                   | Redux slice state          |
| `useRecoilState(atom)`     | `useSelector` + `dispatch` |
| `setUser(data)` directly   | `dispatch(setUser(data))`  |

Super clean. No reducers. No actions. Just reactive global state. ✅

Let me know if you want to add selectors next!


## Redux (global state manager)

`UI → Dispatch → Reducer → Store → UI`

<img width="766" alt="Screenshot 2025-04-02 at 5 45 20 PM" src="https://github.com/user-attachments/assets/ca641e89-2224-4063-ab3c-cc041943badf" />

Absolutely! Here's a complete working setup for **Redux Toolkit with TypeScript in a Next.js project**, using your `useUser` hook and everything wired together.

---

### 🧱 1. `store.ts` — Redux store setup

```ts
// store.ts
import { configureStore } from "@reduxjs/toolkit";
import userReducer from "./userSlice"; // default export from userSlice

export const store = configureStore({
  reducer: {
    user: userReducer, // this controls state.user
  },
});

// TypeScript types
export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```


---

### 🔧 2. `userSlice.ts` — Redux Toolkit slice

```ts
// userSlice.ts
import { createSlice, PayloadAction } from "@reduxjs/toolkit";

type UserState = {
  id: string;
  name: string;
  email: string;
} | null;

const initialState: UserState = null;

const userSlice = createSlice({
  name: "user",
  initialState,
  reducers: {
    setUser: (state, action: PayloadAction<UserState>) => action.payload,
    clearUser: () => null,
  },
});

export const { setUser, clearUser } = userSlice.actions;
export default userSlice.reducer; // 👈 this is the reducer you're importing as `userReducer`
```


---

### ⚙️ 3. `useUser.ts` — Custom hook for user state

```ts
// useUser.ts
import { useSelector, useDispatch } from "react-redux";
import { RootState } from "./store";
import { setUser, clearUser } from "./userSlice";

const useUser = () => {
  const user = useSelector((state: RootState) => state.user);
  const dispatch = useDispatch();

  const updateUser = (userData: { id: string; name: string; email: string }) => {
    dispatch(setUser(userData));
  };

  const clearUserData = () => {
    dispatch(clearUser());
  };

  return { user, updateUser, clearUserData };
};

export default useUser;
```
To be clear, state is the WHOLE redux store, and to select one of the slices we do state.user, or state.transaactions


---

### ✅ 1. What you're seeing:

```ts
reducers: {
  setUser: (state, action) => action.payload,
}
```

- `setUser` = object key  
- `(state, action) => ...` = arrow function as the value

---

### ✅ 2. Same logic using function shorthand:

```ts
reducers: {
  setUser(state, action) {
    return action.payload;
  }
}
```

---

### ✅ 3. What it’s like in plain JS:

```ts
const obj = {
  sayHi: () => console.log("hi")
}
```

---

> You're assigning a **function as a value inside an object** using `:` — that’s just how object literals work in JavaScript.
---

### 🌍 4. `_app.tsx` — Wrap the app in the Redux Provider

```tsx
// pages/_app.tsx
import { Provider } from "react-redux";
import { store } from "../store";
import type { AppProps } from "next/app";

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <Provider store={store}>
      <Component {...pageProps} />
    </Provider>
  );
}

export default MyApp;
```

---

### 🧪 5. `pages/index.tsx` — Example usage in a page

```tsx
// pages/index.tsx
import useUser from "../useUser";

export default function HomePage() {
  const { user, updateUser, clearUserData } = useUser();

  return (
    <div>
      <h1>Welcome {user?.name || "Guest"}</h1>

      <button
        onClick={() =>
          updateUser({
            id: "1",
            name: "Alice",
            email: "alice@example.com",
          })
        }
      >
        Set User
      </button>

      <button onClick={clearUserData}>Clear User</button>
    </div>
  );
}
```

---

## ✅ Recap: What Each File Does

| File           | Responsibility                            |
|----------------|--------------------------------------------|
| `store.ts`     | Creates the Redux store + root types       |
| `userSlice.ts` | Contains user reducer + actions            |
| `useUser.ts`   | Encapsulates Redux logic in a clean hook   |
| `_app.tsx`     | Wraps app in the Redux `<Provider>`        |
| `index.tsx`    | Example usage of the user state and hook   |



## Lifting state up

Lifting state up means moving a piece of useState to a common parent component, so two or more child components can share and update the same state.

AKA we could move `setUsers` and `users` to a parent and pass it to children so  both children could have access to changing users 

#### CAN BE BAD

<img width="751" alt="Screenshot 2025-04-02 at 7 25 08 PM" src="https://github.com/user-attachments/assets/b3e44ef3-28a4-4d49-be84-02a3662c5962" />


## Side Effects

<img width="727" alt="Screenshot 2025-04-02 at 5 38 45 PM" src="https://github.com/user-attachments/assets/4accd059-14b9-499b-9361-362b8d014b98" />

For React, A side effect is any code that runs outside of React’s render flow.

<img width="768" alt="Screenshot 2025-04-02 at 5 37 55 PM" src="https://github.com/user-attachments/assets/37a57f57-a6a4-4f56-9e3b-60273bdaa785" />


If you're touching anything React doesn’t manage directly, it’s probably a side effect — and should be done inside useEffect().

React’s rendering should stay pure and predictable — side effects belong outside that cycle.

## Custom Hooks

Custom hooks are functions that **can use other react hooks** like useState, like in my project i have a `useUser` that automatically connects to Redux, gets the current user, and gives you functions to update or clear them — all in a clean, reusable way.

## Props

Props are meant to be read-only in React because they represent data passed down from a parent. Making them immutable ensures a one-way data flow, which is one of the core principles of React.

<img width="580" alt="Screenshot 2025-04-02 at 5 26 50 PM" src="https://github.com/user-attachments/assets/4630bdda-26a8-4bfa-abbf-be98d5017458" />

If we DO want to change it, we generally pass a function to the child that calls it in the parent to actually change it.

## What is NodeJS?

Node.js was created to run JavaScript on the server using an event-driven, non-blocking model powered by async/await, enabling lightweight and scalable apps. Unlike traditional runtimes that use multi-threaded, blocking I/O, Node.js handles many connections efficiently with a single thread using asynchronous code (offloads work to other threads under the hood)

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

## Callbacks

A callback is a function that is passed in to be called later, it was the old way of handling delays, we now use async/await

## Session Auth vs JWT Auth

Sure! Here's a **super brief comparison** of **JWT vs Cookie/Session** auth in **Next.js**:

---

### 🔐 **JWT Auth (Stateless)**

- Stores user info **in the token** (e.g. userId, email).
- Sent in `Authorization` header:  
  `Authorization: Bearer <token>`
- Stored in **localStorage** or browser cookie storage 🍪.
- ✅ Easy to use in APIs/microservices.  
- ❌ Harder to revoke (token lives until expiry).  
- ❌ Exposed if stored in `localStorage` (vulnerable to XSS).

---

### 🍪 **Session + Cookie Auth (Stateful)**

- Stores user info **on the server** (e.g. Redis or DB).
- Stores the session cookie in the Browser's Cookie Storage
- Browser sends a **`sessionId` cookie automatically** on every request.
- Cookie config: `httpOnly`, `secure`, `sameSite`.
- ✅ Safer (cookie can’t be accessed by JS).
- ✅ Easy to revoke by deleting session server-side.
- ✅ Ideal for browser-based Next.js apps.

THIS IS WHY ITS SAFE 

<img width="792" alt="Screenshot 2025-04-01 at 12 00 56 PM" src="https://github.com/user-attachments/assets/8694dc38-5ab5-4ac7-bba0-192755801478" />

---

**✅ Recommendation for Next.js:**  
Use **cookie + session auth** for secure, server-rendered apps.  
Use **JWT** if you need mobile support or token-based APIs.

To be clear **both are given their cookie from the backend** it's just what is in it:

🧠 So yes — both can use cookies, but:
* Session Auth: cookie is a 🔑 to server memory

* JWT Auth: cookie is the 📦 that contains all the data inside

## Security

`.env` files to store secrets

## How does the Virtual DOM work with React

The Virtual DOM (VDOM) is a lightweight copy of the real DOM kept in memory.

State or Prop Change:

When your component state or props change, React creates a new Virtual DOM tree.

Diffing:

React compares the new tree with the old one (this is called **reconciliation**).

React updates only the real DOM nodes that changed — not the whole page.


### What is JSX?

JSX (JavaScript XML) is a syntax extension for JavaScript that looks like HTML but actually compiles to JavaScript.

This is why `class=...` becomes `className=...`

When You Run npm run build or npm start, JSX becomes Javscript?

Then React uses that JavaScript to render HTML in the browser's DOM (or on the server if using SSR).






## NextJS vs React

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

## ⚛️ **React (alone)**

React is mainly the **V (view)** in MVC. It handles **components, state, and rendering**, but leaves things like:
- Routing
- SSR
- API calls
- Optimization

…up to **you**.

## 🔁 Routing in React:
You use a package like `react-router-dom`:
```jsx
<Route path="/about" element={<About />} />
```

## 🧠 Server-side rendering (SSR)?
- Not included out of the box
- Needs custom setup with something like Next.js, Express, or Remix

How CSR works:
The browser downloads your large JavaScript bundle, which then:
* Loads React,
* Fetches data (e.g., from an API),
* And finally renders the UI.

How SSR Works:
Initial HTML:
* The server runs your React code, fetches the necessary data, and renders a full HTML page with content included.
* User sees: The complete page immediately upon loading—no spinner or blank screen.

Afterward:
* The browser downloads the JavaScript to "hydrate" the page (i.e., add interactivity), but the useful content is already visible.

Total wait time: The server might take, say, 500ms to build the page, but once the HTML arrives, the user sees real content immediately.
With SSR, the server sends fully rendered HTML (as opposed to building it  so users see useful content immediately—even if the page isn’t interactive yet. Then, as the JavaScript loads in the background, React “hydrates” the page, adding interactivity (like clickable buttons). This approach improves perceived performance because users don't stare at a blank screen or spinner; they see content right away, and then the functionality kicks in.

---

### 🔼 **Next.js (React with superpowers)**

Next.js is a **React framework** that adds:
- **Routing**
- **SSR**
- **Static site generation (SSG)**
- **API routes**
- **Image optimization**
- **File-based pages**

Difference between SSG and CreatReactApp or Vite is that 
<img width="785" alt="Screenshot 2025-04-03 at 4 12 36 PM" src="https://github.com/user-attachments/assets/bad812ad-36d6-4050-ba7e-80d45e149a8d" />

<img width="797" alt="Screenshot 2025-04-03 at 4 14 31 PM" src="https://github.com/user-attachments/assets/aade6296-65d3-4449-9b2c-ce86a15661ea" />

Here's a concise explanation:

- **Hydration** means attaching interactivity (event listeners, state management, etc.) to already-rendered HTML in the browser—it doesn't generate the HTML.
- In **CRA/Vite**, the index.html is mostly an empty container (e.g. `<div id="root"></div>`). The browser downloads the JS bundle and React **builds all the HTML** on the client, then hydrates that markup.
- In **Next.js with SSG**, the HTML is **pre-generated on the server (or at build time)**. When a user visits the page, they immediately see the full, static HTML content. Later, React hydrates it in the browser to make it interactive.
- So, the key difference is that in SSG the non-root (actual) HTML is created ahead of time, while in CRA the HTML is generated by the browser. In both cases, hydration (adding functionality) happens in the browser.

This clarifies that I was mistaken if I implied hydration builds the HTML—it only adds interactivity to HTML that's already there.

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

### 🧠 What is getServerSideProps?
* `getServerSideProps` is a special Next.js function that runs on the server before rendering a page.
It fetches data on every request, then passes it to your component as props.

`Yes, your component receives props, but they're generated server-side and used to pre-render the UI before it's delivered to the user.`

```javascript
// pages/user.tsx

export default function UserPage({ user }) {
  return (
    <div>
      <h1>Welcome, {user.name}!</h1>
      <p>Email: {user.email}</p>
    </div>
  );
}

// This runs ONLY on the server per request
export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/user');
  const user = await res.json();

  return {
    props: {
      user, // 👈 becomes a prop to <UserPage />
    },
  };
}
```
<img width="681" alt="Screenshot 2025-04-02 at 8 02 19 PM" src="https://github.com/user-attachments/assets/46e0b0aa-ba93-488e-bae2-30314263f6e0" />

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

<img width="806" alt="Screenshot 2025-04-01 at 6 26 30 PM" src="https://github.com/user-attachments/assets/9d87e2e4-d98e-4a1b-b62b-ca32543f4bd6" />

Rest takes the input and automatically turns it into an array

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

We use `() => { }` when we want to return a FUNCTION and not a FUNCTION CALL, arrow syntax also prevents scope issues and allows you to grab variables outside the function and avoid using `this`

this:
```javascript
return () => clearInterval(interval);
```
is the same as this:

```javascript
function cleanup() {
  clearInterval(interval);
}
return cleanup;
```

This is why we also use arrow funcrtions when we need to pass arguments on `onClick`, if we pass an EXECUTION, it will get called on page load:

<img width="780" alt="Screenshot 2025-04-02 at 3 43 54 PM" src="https://github.com/user-attachments/assets/eaba7041-6d35-40b2-9a3d-64aaf3c28c90" />



the `return` in the useEffect is a react default CLEANUP FUNCTION, so that's whhy the clearInterval only runs when it unmounts
the dependency array decides whether the cleanup function is run. so it'll run if "running" changes. 

## Memory Leaks

Memory leaks happen when an async task outlives the component and tries to do something like setState() after it's unmounted.

Absolutely! Here's just the table for quick reference 👇

---

### ✅ Common Async Actions That Can Cause Memory Leaks

| Type             | Example                            | Why it's Risky                                  |
|------------------|-------------------------------------|-------------------------------------------------|
| 🛰️ `fetch` / `axios`  | `fetch('/api/data')`             | Might resolve **after** the component unmounts |
| ⏱️ `setTimeout`       | `setTimeout(() => ..., 3000)`    | Might fire after the component is gone         |
| 🔁 `setInterval`      | `setInterval(() => ..., 1000)`   | Keeps running unless you manually stop it      |
| 🔌 Subscriptions      | WebSocket, Event listeners       | Keep running unless unsubscribed               |
| 📦 Promises           | `someAsyncFn().then(...)`        | Still async under the hood                     |

---


#### A memory leak would be caused if we hid the timer (making it unmount), but it doesn't clear the interval. Same if we paginated away from it. 

---<img width="778" alt="Screenshot 2025-03-28 at 2 23 29 PM" src="https://github.com/user-attachments/assets/0700f89f-a091-4203-8b8d-af375e811101" />


<img width="786" alt="Screenshot 2025-04-02 at 7 35 40 PM" src="https://github.com/user-attachments/assets/54b8c630-cd9d-4116-81ef-75c4d56b64d5" />

Fixing `fetch` memory leaks

<img width="337" alt="Screenshot 2025-04-02 at 7 35 57 PM" src="https://github.com/user-attachments/assets/7a39a5b6-bb96-4914-89a8-c6373b756156" />



<img width="675" alt="Screenshot 2025-04-02 at 7 40 52 PM" src="https://github.com/user-attachments/assets/0d78eb1b-000a-44e1-9fbf-05678cee6cfa" />

But if you're fetching data inside `useEffect`

, use `AbortController`
`AbortController` lets us cancel the fetch if the component unmounts

<img width="687" alt="Screenshot 2025-04-02 at 7 42 56 PM" src="https://github.com/user-attachments/assets/29f1b4aa-349f-4839-93b1-377a75a01b67" />

### Fetch based memory leaks are solved by getServerSideProps aka Server Side Rendering (SSR), timeout memory leaks would still be relevan however

<img width="681" alt="Screenshot 2025-04-02 at 8 02 19 PM" src="https://github.com/user-attachments/assets/46e0b0aa-ba93-488e-bae2-30314263f6e0" />


Fixing `timeout` memory leaks:

<img width="784" alt="Screenshot 2025-04-02 at 7 45 27 PM" src="https://github.com/user-attachments/assets/98735f42-9170-427d-8acb-7d8798975c74" />



> ❗ We worry about **memory leaks only when calling APIs from inside components** — regardless of **when** it's triggered.

---

### ✅ Safe from memory leaks:
| Where it's called                     | Why it's safe                           |
|--------------------------------------|------------------------------------------|
| `getServerSideProps()`               | Runs only once on the server             |
| `getStaticProps()`                   | Runs at build time (no live lifecycle)   |
| Global state managers (Redux, etc.)  | Outside of component lifecycle           |
| Inside a user action **that finishes quickly** | Component likely stays mounted |


---

<img width="812" alt="Screenshot 2025-04-02 at 8 08 55 PM" src="https://github.com/user-attachments/assets/384c2fa1-1eaa-40d3-a9c9-4d1e5eabd14f" />


### ⚠️ Can cause memory leaks:
| Situation                                 | Why it's risky                           |
|------------------------------------------|------------------------------------------|
| API calls in `useEffect()`               | If component unmounts before it completes |
| Long-running API calls (user stays < page) | May still resolve after unmount          |
| Forgetting to use `AbortController`      | Will try `setState` on unmounted component |

---

### ✅ So when do you care?

You need to prevent memory leaks **anytime:**
- You're using `useEffect` for fetching data
- You're doing something async inside a component
- The component **might unmount** before the task finishes

---

## 🔁 TL;DR:

> You only worry about memory leaks when using **async code inside components**.  
> If you fetch data with `getServerSideProps` or in a global store — you’re safe.  
> If you're using `useEffect` + `fetch`, always clean it up with `AbortController`.



### 🧠 Explanation of how it works:

- `time` state holds the countdown value.
- `running` state tracks whether the countdown has started.
- `useEffect` runs when `running` becomes `true`.
- `setInterval()` is used to tick the countdown every second.
- When the timer reaches 0, the interval is cleared to stop counting.
- We also clear the interval if the component unmounts or `running` changes — this prevents memory leaks.

- When you pass a function to setState (prev => prev -1) (like setTime), React will automatically call your function with the most recent state value as the first argument.
- 
<img width="809" alt="Screenshot 2025-04-01 at 6 32 35 PM" src="https://github.com/user-attachments/assets/2b3b0724-32a3-49a5-b723-550d81249512" />

<img width="820" alt="Screenshot 2025-04-01 at 6 35 38 PM" src="https://github.com/user-attachments/assets/1d7b4dfd-a0b0-45a3-8665-a49420c615d0" />

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

A child component is any component rendered inside another component, even if you don’t pass it any props.

<img width="832" alt="Screenshot 2025-04-02 at 4 01 53 PM" src="https://github.com/user-attachments/assets/ebbeb918-ccc5-45f7-8c68-8e37360ca890" />

here is how you can access what was placed between a component, but remember, all a child is: whatever we put between a tag. Could be a <div> or a <p>

* passing react components or JSX elements in props
<img width="766" alt="Screenshot 2024-08-09 at 4 37 52 PM" src="https://github.com/user-attachments/assets/0d07a4d2-8ef5-4232-8740-80aaf3b9f659">

## Keys
<img width="811" alt="Screenshot 2024-08-09 at 4 38 46 PM" src="https://github.com/user-attachments/assets/63356f19-7b8e-4d9c-9664-85f2d10d18fd">

* Keys are used to identify components in lists, they can be unique strings or numbers
* usually you can use the current index:

<img width="777" alt="Screenshot 2025-04-02 at 4 04 14 PM" src="https://github.com/user-attachments/assets/287d14fb-6557-46fc-8667-ce6441ab2a45" />

## Events

Most popular `onclick` `onChange` `onSubmit`


## Class and Function based components
* Function based components are becoming more popular and are considered the better option.
* Class compoenents are outdated and almost never necessary, they are OLD
  
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


## Props
* passing props down through multiple layers is called prop drilling
<img width="998" alt="Screenshot 2024-07-07 at 1 54 22 PM" src="https://github.com/mfkimbell/react-typescript-notes/assets/107063397/356d6619-5b3d-4b30-9d18-9f18b0479761">


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
