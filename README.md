# react-typescript-notes

# React

## Class and Function based components
* Function based components are becoming more popular and are considered the better option.

## JSX (Javascript XML)

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
React Hooks are functions that allow you to use state and other React features without writing a class component. They were introduced in React 16.8 to enable state and side-effect management in functional components, offering a more concise and expressive way to build components compared to class-based components.
**useState**
* sets the state
**useEffect**
* Purpose: Handles side effects such as data fetching, subscriptions, or manually changing the DOM. It runs after the component renders.
* Syntax: useEffect(() => { /* side effect */ }, [dependencies]);
``` Typescript
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
**useContext**
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
