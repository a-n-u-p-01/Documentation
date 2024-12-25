### Basic Concepts

1. **What is React?**
   - React is a JavaScript library for building user interfaces, primarily for single-page applications, developed by Facebook.

2. **What are the main features of React?**
   - Component-based architecture, Virtual DOM, Unidirectional data flow, JSX (JavaScript XML), and the ability to manage state.

3. **What is JSX?**
   - JSX is a syntax extension that allows writing HTML-like code in JavaScript, which gets transpiled to JavaScript.

4. **What are components in React?**
   - Components are reusable building blocks in React that can be functional or class-based, representing parts of the UI.

5. **What is the difference between functional and class components?**
   - Functional components are stateless and are defined as functions, while class components can maintain internal state and lifecycle methods.

### State and Props

6. **What are props in React?**
   - Props (short for properties) are inputs to a React component, allowing data to be passed from parent to child components.

7. **What is state in React?**
   - State is an object that holds data that may change over the lifetime of a component, affecting the rendering of the UI.

8. **How do you update state in React?**
   - State can be updated using the `setState()` method in class components or the `useState` hook in functional components.
### Lifecycle Methods

9. **What are lifecycle methods in React?**
   - Lifecycle methods are hooks that allow you to run code at specific points in a component's life, such as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`.

10. **What is the purpose of `componentDidMount`?**
    - It is called after a component is rendered to the DOM, often used for fetching data or initializing subscriptions.

### Hooks

11. **What are React Hooks?**
    - Hooks are functions that let you use state and other React features in functional components. Common hooks include `useState`, `useEffect`, and `useContext`.

12. **How does the `useEffect` hook work?**
    - The `useEffect` hook allows you to perform side effects in functional components, similar to lifecycle methods. It can return a cleanup function.

### Routing

13. **What is React Router?**
    - React Router is a library for routing in React applications, enabling navigation between different views or components.

14. **How do you create a route in React Router?**
    ```jsx
    import { BrowserRouter as Router, Route } from 'react-router-dom';

    <Router>
      <Route path="/about" component={About} />
    </Router>
    ```

### Performance

15. **What is the Virtual DOM?**
    - The Virtual DOM is a lightweight copy of the actual DOM, allowing React to minimize direct manipulation of the DOM for better performance.

16. **How does React improve performance with the Virtual DOM?**
    - React updates the Virtual DOM and then efficiently compares it with the actual DOM to determine the minimal number of updates needed.

### Context and State Management

17. **What is Context API?**
    - The Context API allows you to share values (like state) between components without having to pass props down manually at every level.

18. **What is Redux?**
    - Redux is a state management library that helps manage application state in a predictable way, often used with React for larger applications.

### Miscellaneous

19. **What are higher-order components (HOCs)?**
    - HOCs are functions that take a component and return a new component, allowing you to reuse component logic.

20. **What is a controlled component?**
    - A controlled component is a form element whose value is controlled by React state, enabling better control over form data.

21. **What is a key in React?**
    - A key is a unique identifier used by React to identify which items have changed, are added, or are removed from a list, helping optimize rendering.

22. **What is the difference between `useMemo` and `useCallback`?**
    - `useMemo` memoizes a value, while `useCallback` memoizes a function, both helping optimize performance by preventing unnecessary recalculations.

### Problem Solving

23. **How do you conditionally render a component?**
    ```jsx
    {isLoggedIn ? <Dashboard /> : <Login />}
    ```

24. **How do you handle forms in React?**
    ```jsx
    const handleSubmit = (event) => {
      event.preventDefault();
      // Handle form submission
    };

    <form onSubmit={handleSubmit}>
      <input type="text" />
      <button type="submit">Submit</button>
    </form>
    ```

25. **How would you fetch data in a component?**
    ```jsx
    useEffect(() => {
      fetch('api/data')
        .then(response => response.json())
        .then(data => setData(data));
    }, []);
    ```

