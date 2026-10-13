# React.memo

is a higher-order component (HOC) used to optimize performance by preventing a functional component from re-rendering if its props have not changed

## How It Works

• Skipping Re-renders: Normally, when a parent component re-renders, all of its child components re-render as well—even if the child's own data has not changed.
• Shallow Comparison: Wrapping a component with React.memo(Component) tells React to compare the new props with the previous props using a shallow comparison (Object.is).
• Caching Output: If the props are equal, React skips rendering the component and reuses its previously cached output.

## When to Use It

• Heavy Components: When a component performs expensive rendering logic or large UI trees (like big tables or complex lists).
• Frequent Parent Updates: When a parent component re-renders often, but the child component consistently receives the exact same props.
• Pure Presentation: When a component is "pure" (meaning its output depends only on its props).

### Example

```tsx
import { useState } from "react";
import { MemoChild } from "@/components/react-memo/memo-child";

export const MemoParent = () => {
  const [count, setCount] = useState(0);
  return (
    <div>
      counter: {count}
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
      <div>
        <MemoChild />
      </div>
    </div>
  );
};
```

```tsx
import { memo } from "react";

// Named export
const MemoChild = memo(() => {
  console.log("MemoChild render");
  return <div>MemoChild</div>;
});

// Default export
export default memo(MemoChild);
```

> **Note:** With the React Compiler enabled, much of this manual memoization is handled automatically.

# useMemo

`useMemo` is a React Hook used to optimize performance by caching the result of an expensive calculation between re-renders, so it only recomputes when its dependencies change.

## How It Works

- **Caching a Value:** On the first render, React runs your function and stores the returned value.
- **Dependency Comparison:** On later renders, React compares each dependency with its previous value using `Object.is`.
- **Reusing the Result:** If no dependency changed, React returns the cached value and skips the function. If one changed, it recomputes and caches the new result.

## When to Use It

- **Expensive Calculations:** Heavy loops, filtering or sorting large lists, or parsing large data.
- **Unrelated State Updates:** When the component re-renders often because of other state (like a counter), but the calculation's inputs rarely change.
- **Stable References:** When passing objects or arrays to a `React.memo` child, or using them in `useEffect`/`useCallback` dependency arrays.

## When NOT to Use It

- **Cheap Calculations:** Simple math, string concatenation, or small array operations. The memoization overhead can cost more than the work it saves.
- **Memoizing Everything by Default:** It adds complexity and memory use with no measurable gain. Profile first, then optimize.
- **Side Effects:** The function must be a pure calculation. Use `useEffect` for fetching, subscriptions, or logging that matters.
- **Needing a Guaranteed Cache:** React may discard cached values (for example, for offscreen components), so your code must still work if the function re-runs.
- **Speeding Up the First Render:** `useMemo` only skips _repeat_ work. The first calculation still runs and blocks rendering. For truly heavy work, consider a Web Worker or moving it off the main thread.
- **Unstable Dependencies:** If a dependency is a new object or array every render, the memo never hits and is wasted.

### Example

```tsx
import { useState, useMemo } from "react";
import { Button } from "@/components/ui/button";

const ExpensiveComponent = () => {
  const sum = () => {
    console.log("Calculating sum...");
    let i = 0;
    for (i; i < 1000000000; i++) {
      i += 1;
    }
    console.log("Sum calculated:", i);
    return i;
  };

  // Without useMemo: runs on every re-render (UI freezes on each click)
  // const total = sum()

  // With useMemo: runs once, then the cached value is reused
  const total = useMemo(() => sum(), []);
  return <div>Expensive Component: {total}</div>;
};

export const UseMemoComponent = () => {
  const [count, setCount] = useState(0);

  return (
    <div className="space-y-4 rounded-lg border border-border bg-background p-6 text-foreground">
      <h1>UseMemo Example</h1>

      <div className="mb-4 rounded-md border border-border bg-muted p-4">
        <ExpensiveComponent />
      </div>

      <p className="text-2xl font-semibold">Counter: {count}</p>

      <div className="flex gap-2">
        <Button onClick={() => setCount((c) => c + 1)}>Increment</Button>
        <Button variant="outline" onClick={() => setCount((c) => c - 1)}>
          Decrement
        </Button>
        <Button variant="destructive" onClick={() => setCount(0)}>
          Reset
        </Button>
      </div>
    </div>
  );
};
```

Clicking Increment, Decrement, or Reset re-renders `UseMemoComponent`, which also re-renders `ExpensiveComponent` since it isn't wrapped in `memo`. Because the dependency array is empty (`[]`), `useMemo` returns the cached `total` and `sum()` doesn't run again. Check the console: "Calculating sum..." should print only once.

> **Note:** With the React Compiler enabled, much of this manual memoization is handled automatically.

# React Compiler

> **Note:** If the React Compiler is enabled in your project, you don't need `React.memo`, `useMemo`, or `useCallback` in most cases.

The React Compiler is a build-time tool (a Babel/bundler plugin) that analyzes your components and automatically memoizes components, JSX elements, and computed values based on what they actually depend on. So a child like `<MemoChild />` is skipped on parent re-renders without you wrapping it in `memo`.

- **Opt-in:** It isn't automatic with React 19. You have to install and enable the plugin in your build setup.
- **Existing `memo` is safe:** It keeps working alongside the compiler, so you don't need to remove it.
- **Rules of React:** Components that break the rules (mutating props or state, impure render logic) are skipped by the compiler and still need manual optimization.
