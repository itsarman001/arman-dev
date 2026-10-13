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
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
      <div>
        <MemoChild />
      </div>
    </div>
  );
};
```

```tsx
import { memo } from "react";

export const MemoChild = memo(() => {
  console.log("MemoChild render");
  return <div>MemoChild</div>;
});

// with default export
export default memo(MemoChild);

// Note: In React 19 with react compiler it is not needed as react compiler handles it internally
```

