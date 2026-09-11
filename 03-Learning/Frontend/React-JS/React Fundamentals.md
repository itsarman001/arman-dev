# React Fundamentals

## Summary

### Components

- Components Describe a part of the user interface
- Components are JavaScript functions that return HTML(JSX\*)
- Components can be nested inside other components

### Import and Export Summary

- Organizing components into separate files makes your code more maintainable
- Each component has its own space, and you can easily find and update components as your app grows
- This modular approach means you can reuse components across different parts of your app just by importing them

### JSX Rules take away

- JSX rules exists because JSX is JavaScript, not HTML
  - single root element? That's how JavaScript return work
  - self-closing tags? That's XML syntax
  - camelCase attributes? That's avoiding JavaScript reserved words
  - curly braces? That's your gateway to all of JavaScript's power

### Props Summary

- Props are how components talk to each other
- They're like the arguments to a function
- They make components dynamic and truly reusable
- You pass them when you invoke a component
- You access them in the component using the `props` object
- You can pass any JavaScript value, including functions, arrays, objects and JSX
- Destrucuring is a cleaner way to access props

### Props Patterns

- Default props
  - the default value is only used if the **prop is missing** or if you pass **undefined**
  - if you pass **null** or **0**, the default value won't be used.
- Forwarding props with spread operator
- Passing JSX as children
  - In HTML, you know that it is natural to nest elements inside each other
  - in React, you can do the exact same thing with components
  - Layouts, Cards, Modals etc

### Conditional Rendering

- Conditional rendering is how we make our components show different content based on different conditions
  - show a login button if the user isn't logged in
  - show their profile if they are logged in
  - show an error message only when there's an error

### 4 Ways to Conditional Rendering

- If/Else Statements
- The Ternary Operator
- And Operator (&&)
```jsx
	  // const messages = ["a", "b", "c", "d"];
	  {/* in this example if messages.length = 0 -> React renders : 0*/}
	  {messages.length && <span>{messages.length} Messages</span>}
	  <br />
	  {/* so we need to be explicit */}
	  {messages.length > 0 && <span>{messages.length} Messages</span>}
```
- Variables for complex logics
```jsx
let onlineStatus = isOnline ? "🟢 Online" : "🔴 Offline";
return <div><span>{onlineStatus}</span></div>
```

### Conditionl Rendering Summary
- If Statements - Great for completely different renders of returning null
- Ternary Operator (?:) - Perfect for either / or situation
- And Operator (&&) - Ideal for show/hide scenarios
- Variables - Best for complex logic that would make your JSX messy
- Activity Component (React 19.2) 

### Rendering Lists
 A common scenario when building web applications is to display a list of items
 - a list of products in an e-commerce app
 - courses in a learning platform
 - comments on a blog post
To render a list of data in React, we need to take our data, put it in an array, and then transform that array into the HTML we need

Rendering lists in React is just JavaScript. You take an array, you transform it with `map` into an array of JSX elements, and React renders them all

#### Key Prop
When you render a list of elements, you need to add a key prop to each element with a unique value within the list
- if your data comes from a  database you can use the database keys/IDs which are unique by nature
- if your data is created and persisted locally, use the incrementing counter, crypto.randomUUID() or a package like uuid

#### Rules of keys
- The key prop goes on the outermost repeated element
- Keu is a special prop that React uses internally. It's not a prop that you pass to a child component and destructure there to get access to it

#### Why does React need keys?
- Keys are used by React to track the items in the list
- They're crucial for React to efficiently update the UI

#### Keys Summary
- key is a special prop that react uses internally to track items in the list
- it need to be unique with the list
- the key prop goes on the outermost repeated element
- keys are crucial for React to efficiently update the UI

#### Using item index as key
You'll see tons of example where developers use the array index as the key when rendering lists. It looks clean, it's simple, ad it makes that warning go away. But there's a reason this is considered an anti-pattern

#### Index as key anti-pattern
- The problem with using index as a key is that the index represents the position and not the item itself
- That can lead to subtle and confusing bugs when  the order of the items changes in the list

##### When it is safe to use index as a key?
- Your Items don't have a unique ID
- Your list is completely static - you do not add or remove items in the list
- Your list is never reorder or filtered.

Note: If you ever see weird behavior in your lists - like data jumping between items, input losing focus, or animations glitching - check your keys first.

#### Event Handling
Responding to events in React isa simple two-step process
- You define a function that should be executed when the event occurs
- You assign the function to a special prop that start with **on**
React supports all the events from regular JavaScript
- onChange for inputs
- onSumbit for forms
- onMouseEnter for hovering
Note: Events should be in written camelCase.
```jsx
export const AlertButton = () => {

  const handleClick = () => {

    alert("Button Clicked!");

  };

  

  return (

    <button

      onClick={handleClick}

      className="px-2 py-1 bg-slate-600 text-slate-400 rounded"

    >

      Click Me!

    </button>

  );

};
```

#### Handling Events Summary
- Handling events in React is all about passing functions to special props like **onClick**
- Remember to pass the function, not call it.
- Use the event object when you need info about the event
- Event handlers have access to all the component's variables and props since they're defined inside the component

#### Event Handlers as props
- When you need a child component to communicate with its parent, you pass event handlers as props
- The Child says something happened and the parent decides what to do about it
- You'll use it every time you build a reusable component that needs different behaviors in different places
```jsx
export const ActionButton = ({ text, onClick }) => {

  return <button onClick={() => onClick(text)}>{text}</button>;

};
```

#### State
##### useState Hook
When we call setCount
1. React updates the state the value
2. React re-renders the component
3. useState gives us the new value
4. our UI shows the updated count
```jsx
import { useState } from "react";
import { ActionButton } from "./ActionButton";

export const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <ActionButton onClick={() => setCount((prev) => prev + 1)} text={count} />
  );
};
```
##### useState - Lazy Initialization
Useful when you have any expensive computation you need to do to calculate the initial state
- reading from local storage
- fetching data from an API
- doing any other heavy computation

##### useState summary
- To add state to a component, import `useState` from React and call it with an initial value
- You can also pass a function to `useState` for lazy initialization
- It returns an array with two items: `[currentValue, setterFunction]`
- Use array destructuring to name them
- Use the state value in your JSX
- Call the setter function to update state and trigger a re-render
- You can have multiple state variables, each managing its own data
- Multiple instances of a component each have their own local state

##### Golden rules of hooks
1. Only call hooks at the top level of your function - Not inside loops, conditions, nested functions or any try/catch blocks
2. Only call hooks from React functions - From React Components or custom hooks
##### Why Rules of hooks exists
"Why did React make these rules? why not just let us put hooks wherever?"
The answer is predictability
React need to know that every time your component renders, ti will call the exact same hooks in the exact same order.

This lets React:
- Keep track of state between renders
- know which useState call corresponds to which piece of state
- optimize performance
- provide helpful error message

```jsx
import { useState } from "react";

export const UserDashboard = ({ isPremium = false }) => {
const [credits, setCredits] = useState(100);

if (!isPremium) return <div>upgrade to premium to see the credits</div>;

return (

<div>

<p>You have {credits}</p>

<button onClick={() => setCredits(0)}>Spend all Credits</button>

</div>

);

};
```

##### How State update works
Updating the UI is a 3-Phase Process
1. Trigger: This happens the moment you call a state setter function like a setCount(), You're not updating the UI yet.
2. Render: React Calls your component function again. React figures out which part of the UI, if any, need to be updated. Re-running the function doesn't immediately change what's on screen
3. Commit: React takes the changes it calculated during the render phase and applies them to the DOM
```
<p>Count: 0</p>
		  |
<p>Count: 1</p>
```

##### How `setCount` update works
1. You call `setCount(count + 1)` *(trigger phase)*
2. React marks your component as needing an update *(trigger phase)*
3. React calls your component function again *(render phase)*
4. Your function returns new JSX with the updated count *(render phase)*
5. React compares this render with the previous one and figures out what changed *(render phase)*
6. React updates only what changed in the actual DOM *(commit phase)*

```jsx
import { useState } from "react";

  

export const SimpleCounter = () => {

const [count, setCount] = useState(0);

const handleClick = () => setCount((prev) => prev + 1);

return (

<div>

<p>Count: {count}</p>

<button onClick={handleClick}>Increment</button>

</div>

);

}; 
```

##### State as a snapshot
in React, state updates are not immediate. When you call setState, you're scheduling an update for the next render. Inside your current render, the state values won't change. They're a snapshot, and snapshots don't change. If you log the count inside you event handler, you'll always get the same value. Think of it like taking a photo. Once you take the photo, the people in it don't move, even if the real people walk away. State in a render is like that photo - it's frozen at that moment in time.

```jsx
const handleClick = () => {

console.log("Count value before increment", count); // 0

setCount((prev) => prev + 1);

console.log("Count value before increment", count); // 0

};
```

##### Batching
React waits untill your event handler finishes, gathers all your state updates and applies them in one render
##### State Update Cycle
1. Triggering Updates
2. the snapshot behavior
3. using updater functions for dependent updates
4. how React optimizes everything with batching

##### useState with Objects

```jsx
const [user, setUser] = useState({
    name: "John Wick",
    age: 30,
    email: "johnwick@email.com",
    address: {
      city: "New York",
      country: "USA",
    },
  });
```

When your state is an object, always create a **new object** instead of modifying the existing one.

```jsx
setUser({
  ...user,
  name: "John Wick 2",
});
```

For nested objects, spread every level that you're updating.

```jsx
setUser({
  ...user,
  address: {
    ...user.address,
    city: "Los Angeles",
  },
});
```

##### useState with Objects Summary
- Treat state as **immutable**—never modify it directly.
- Always use the setter function returned by `useState`.
- Calling the setter with an object **replaces** the entire object.
- Use the spread operator (`...`) to preserve existing properties.
- For nested objects, spread both the parent object and the nested object before updating a field.
- The spread operator performs a **shallow copy**, so every nested level you update must also be copied.

##### useState with arrays summary
Always use the setter function to update state as changing the array directly won't trigger a re-render
Spread operator or concat to add items
Filter to remove items
Map to update items

##### Share state summary
- When two or more components need the same changing data, lift the state up
- Put That state in the closest common parent
- The component with useState owns the data and controls how it changes
- Children receive the state as props
- Children request changes by calling setter functions passed down from the parent
##### useReducer() Summary
useReducer is another hook for state management in React
The Hook accepts two parameters:
- a reducer function
- the initial state
The Reducer function itself accepts two parameters:
- The Current State
- An Action
Based on the inputs, it returns the new state and along with it, a dispatch function. You call dispatch to trigger state updates

##### useReducer() mechanics
- Action describe what happened
- Dispatch sends those actions to the reducer
- The reducer decides how to update state
- The component re-renders with the new state

```jsx
import { useReducer } from "react";

const initaialState = 0;
const counterReducer = (state, action) => {
  switch (action) {
    case "increment":
      return state + 1;
    case "decrement":
      return state - 1;
    case "reset":
      return initaialState;
    default:
      return state;
  }
};

export const CounterWithReducer = () => {
  const [count, dispatch] = useReducer(counterReducer, initaialState);
  return (
    <div>
      <div>Count: {count}</div>
      <div>
        <button onClick={() => dispatch("increment")}>Increment</button>
        <button onClick={() => dispatch("decrement")}>Decrement</button>
        <button onClick={() => dispatch("reset")}>Reset</button>
      </div>
    </div>
  );
};
```

##### useReducer() lazy initialization summay
- useReducer accepts an optional third argument, an init function
- React calls init once, passing it the second argument
- Whatever init return becomes the actual initial state
- The init function only runs when the component mounts, not on every render
- it is ideal for expensive initial computations or reading from external sources like local storage

##### useState() and useReducer()
useState is really just a specialized version of useReducer where:
- The reducer always returns the action as the new state
- if the action is a function, it calls it with the current state first
- the dispatch function is wrapped in a setState function for a nicer API
useReducer is the most fundamental hook
useState is simply a convenience built on top of it for the most common use case: replacing state with a new value

##### Prop Drilling
Sending props through layers of components just to reach the ones that actually need them
Two Main Problems:
1. maintenance: Every time you need to add a new property to user, update an existing one, or remove one you no longer need, you have to touch every single component in the chain
2. performance: if a prop changes in a parent, every child that receives that prop re-renders, even if they don't use the prop. when user updates, all those middle components re-render unnecessarily

#### Context
Context lets you share data across your component tree without having to manually pass props through every intermediate component
- Context lets you broadcast a value at a high level
- any component nested inside can subscribe to it directly

##### Use API
In react 19 meta has introduced `USE` api that work like `Context api` and follow the same structure but it is not a hook so it don't need to follow the rules of hooks

in short `use` is simply a more flexible alternative to useContext it does the same job (reading values from Context) but without the restrictions that comes with hooks

##### Refs and useRef hook
A ref is a way to store a mutable value that
- persists across renders
- doesn't cause re-renders when it changes

##### Refs and useRef hook summary
Refs are used to store values that don't affect the UI but need to persist across renders
- State is for rendering
- refs are for remembering
We can create a ref using the useRef hook
it accepts an initial value and returns and object with a single property called current which holds the value of the ref
we can use refs to store value like interval IDs, timeout IDs, previous values etc

##### Manipulating the DOM with Refs
React is declarative - you describe what the UI should look like and React updates the DOM for you
But there are certain situations like
- Focusing an input
- scrolling an element into view
- measuring the size or position of an element
You need a direct handle to the DOM
Refs are how you get that handle
```jsx
import { useRef } from "react";

export const FocusInput = () => {
  const inputRef = useRef(null);

  const handleInputFocus = () => {
    inputRef.current.focus();
  };
  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Focus Ref" />
      <button onClick={handleInputFocus}>FocusInput</button>
    </div>
  );
};
```

##### A note about refs
1. Refs are an escape hatch
2. A necessary one, but still a escape hatch
3. You should use them sparingly
4. If you find yourself constantly manipulating the DOM, it usually means you're fighting React instead of working with it