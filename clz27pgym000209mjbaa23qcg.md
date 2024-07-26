---
title: "React Hooks and Their Uses: A Must Read"
seoTitle: "Understanding React Hooks: Essential Guide"
seoDescription: "Understand React Hooks, their uses, and learn to create custom hooks to enhance your React applications"
datePublished: Fri Jul 26 2024 04:38:25 GMT+0000 (Coordinated Universal Time)
cuid: clz27pgym000209mjbaa23qcg
slug: react-hooks-and-their-uses-a-must-read
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721968700366/0d6d3618-b36c-4401-bfd3-beb61298c29b.png
tags: ai, cpp, algorithms, software-development, programming-blogs, css, java, javascript, web-development, backend, apis, reactjs, blockchain, programming-tips, reacthooks

---

# Introduction -

React has revolutionized how we build user interfaces by providing a component-based architecture that encourages reusability and modularity. However, managing state and side effects within components has always been a challenge. Enter React Hooks. Introduced in React 16.8, Hooks allow you to use state and other React features without writing a class. They simplify the logic and enhance the readability of your components. In this blog, we’ll explore all the built-in React Hooks and how to create your own custom Hook to harness the full power of React.

---

## The Basics: Built-in React Hooks

### 1\. **useState**

The `useState` Hook is the cornerstone of state management in functional components. It allows you to add state variables to your components.

```javascript
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

### 2\. **useEffect**

The `useEffect` Hook lets you perform side effects in your components, such as fetching data, directly interacting with the DOM, or subscribing to services.

```javascript
import React, { useState, useEffect } from 'react';

function DataFetcher() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => setData(data));
  }, []);

  return <div>{data ? data.message : 'Loading...'}</div>;
}
```

### 3\. **useContext**

The `useContext` Hook allows you to access the context object, making it easier to share data across the component tree without prop drilling.

```javascript
import React, { useContext } from 'react';

const ThemeContext = React.createContext('light');

function ThemeButton() {
  const theme = useContext(ThemeContext);

  return <button style={{ background: theme === 'dark' ? '#333' : '#FFF' }}>Theme Button</button>;
}
```

### 4\. **useReducer**

The `useReducer` Hook is an alternative to `useState` for managing more complex state logic. It’s similar to Redux but integrated directly into your components.

```javascript
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}
```

### 5\. **useCallback**

The `useCallback` Hook returns a memoized version of the callback function that only changes if one of the dependencies has changed. It’s useful for optimizing performance.

```javascript
import React, { useState, useCallback } from 'react';

function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <Child onClick={handleClick} />
    </div>
  );
}

function Child({ onClick }) {
  return <button onClick={onClick}>Click me</button>;
}
```

### 6\. **useMemo**

The `useMemo` Hook returns a memoized value that only recomputes when one of the dependencies changes. It’s used to optimize performance by avoiding expensive calculations on every render.

```javascript
import React, { useState, useMemo } from 'react';

function ExpensiveCalculation({ num }) {
  const result = useMemo(() => {
    let sum = 0;
    for (let i = 0; i < num; i++) {
      sum += i;
    }
    return sum;
  }, [num]);

  return <div>Sum: {result}</div>;
}
```

### 7\. **useRef**

The `useRef` Hook is used to persist values across renders without causing a re-render. It’s often used to access DOM elements directly.

```javascript
import React, { useRef } from 'react';

function TextInput() {
  const inputRef = useRef();

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

### 8\. **useLayoutEffect**

The `useLayoutEffect` Hook runs synchronously after all DOM mutations but before the browser has a chance to paint. It’s used for reading layout from the DOM and synchronously re-rendering.

```javascript
import React, { useState, useLayoutEffect } from 'react';

function LayoutEffectComponent() {
  const [height, setHeight] = useState(0);
  const ref = useRef();

  useLayoutEffect(() => {
    setHeight(ref.current.clientHeight);
  });

  return (
    <div ref={ref} style={{ height: '100px' }}>
      <p>Height: {height}</p>
    </div>
  );
}
```

### 9\. **useImperativeHandle**

The `useImperativeHandle` Hook customizes the instance value that is exposed when using `ref` in a parent component. It’s used in conjunction with `forwardRef`.

```javascript
import React, { useImperativeHandle, useRef, forwardRef } from 'react';

const CustomInput = forwardRef((props, ref) => {
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    }
  }));

  const inputRef = useRef();

  return <input ref={inputRef} type="text" />;
});

function Parent() {
  const inputRef = useRef();

  return (
    <div>
      <CustomInput ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>Focus Input</button>
    </div>
  );
}
```

### 10\. **useDebugValue**

The `useDebugValue` Hook is used to display a label for custom hooks in React DevTools, making it easier to debug custom hook behavior.

```javascript
import React, { useState, useEffect, useDebugValue } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  useDebugValue(isOnline ? 'Online' : 'Offline');

  useEffect(() => {
    // Imagine this subscribes to a friend's status
    setIsOnline(friendID % 2 === 0); // Just a mock
  }, [friendID]);

  return isOnline;
}
```

---

## Creating Custom Hooks

Custom Hooks let you extract component logic into reusable functions. They follow the same principles as built-in Hooks and can use other Hooks inside them.

### Example: useFetch

Let’s create a custom Hook `useFetch` that fetches data from an API.

```javascript
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      const response = await fetch(url);
      const result = await response.json();
      setData(result);
      setLoading(false);
    };

    fetchData();
  }, [url]);

  return { data, loading };
}

// Usage
import React from 'react';

function App() {
  const { data, loading } = useFetch('https://api.example.com/data');

  if (loading) return <p>Loading...</p>;

  return (
    <div>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}

export default App;
```

### Example: useLocalStorage

Another example is `useLocalStorage`, which syncs state with `localStorage`.

```javascript
import { useState } from 'react';

function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(error);
      return initialValue;
    }
  });

  const setValue = value => {
    try {
      setStoredValue(value);
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(error);
    }
  };

  return [storedValue, setValue];
}

// Usage
import React from 'react';

function App() {
  const [name, setName] = useLocalStorage('name', 'Anonymous');

  return (
    <div>
      <input
        type="text"
        value={name
```