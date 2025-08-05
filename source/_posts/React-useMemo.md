---
title: React-useMemo
date: 2025-08-05 09:53:48
tags: React
---

https://zh-hans.react.dev/reference/react/useMemo

App.tsx

```tsx
import { useMemo, useState } from "react";

function App() {
  const [count1, setCount1] = useState(0);
  const [count2, setCount2] = useState(0);

  const count3 = useMemo(() => {
    console.log("计算count3");
    return count1 + 1;
  }, [count1]);

  return (
    <div>
      <div>{count3}</div>
      <button onClick={() => setCount1(count1 + 1)}>count1++</button>
      <button onClick={() => setCount2(count2 + 1)}>count2++</button>
    </div>
  );
}

export default App;
```
