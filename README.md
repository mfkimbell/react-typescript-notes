# react-typescript-notes
<img width="1148" alt="Screenshot 2024-08-09 at 4 39 37 PM" src="https://github.com/user-attachments/assets/a6ca452e-d66d-4d9d-8250-679806954df2">
<img width="1243" alt="Screenshot 2024-08-09 at 4 40 16 PM" src="https://github.com/user-attachments/assets/08a76e30-ffbe-49f9-a0fd-ad699eb32064">
<img width="1258" alt="Screenshot 2024-08-09 at 4 42 27 PM" src="https://github.com/user-attachments/assets/9cb7a23c-6a1d-490a-84e7-4938ffa1612b">

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
