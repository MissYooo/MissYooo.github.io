---
title: React-useSyncExternalStore
date: 2025-08-05 09:57:25
tags: React
---

https://zh-hans.react.dev/reference/react/useSyncExternalStore

src\hooks\useOnLine.tsx

```tsx
import { useSyncExternalStore } from "react";

const useOnLine = () => {
  return useSyncExternalStore(
    (cb: () => void) => {
      window.addEventListener("online", cb);
      window.addEventListener("offline", cb);
      return () => {
        window.removeEventListener("online", cb);
        window.removeEventListener("offline", cb);
      };
    },
    () => navigator.onLine
  );
};

export { useOnLine };
```
