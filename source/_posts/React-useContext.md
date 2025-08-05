---
title: React-useContext
date: 2025-08-05 09:22:01
tags: React
---

https://zh-hans.react.dev/reference/react/useContext

src\context\countContext.tsx

```tsx
import { createContext } from "react";

interface CountContextType {
  count: number;
  setCount: (val: number) => void;
}

export const CountContext = createContext({} as CountContextType);
```

App.tsx

```tsx
import { useState } from "react";
import { Card } from "./components/Card";
import { CountContext } from "./context/countContext";

function App() {
  const [count, setCount] = useState(0);
  return (
    <CountContext value={{ count, setCount }}>
      <Card></Card>
    </CountContext>
  );
}

export default App;
```

src\components\Card.tsx

```tsx
import type React from "react";
import { useContext } from "react";
import { CountContext } from "../context/countContext";

const Card: React.FC = () => {
  const countContext = useContext(CountContext);
  return (
    <div>
      <div>{countContext.count}</div>
      <button
        onClick={() => {
          countContext.setCount(countContext.count + 1);
        }}
      >
        设置Count
      </button>
    </div>
  );
};

export { Card };
```
