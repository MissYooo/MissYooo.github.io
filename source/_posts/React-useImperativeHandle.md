---
title: React-useImperativeHandle
date: 2025-08-05 08:58:46
tags: React
---

https://zh-hans.react.dev/reference/react/useImperativeHandle

card.tsx

```tsx
import type React from "react";
import { useImperativeHandle, type Ref } from "react";

export interface CardRef {
  sayHi: () => void;
}

export interface CardProps {
  title?: string;
  children?: React.ReactElement | React.ReactElement[];
  ref?: Ref<CardRef>;
}

const Card: React.FC<CardProps> = (props) => {
  useImperativeHandle(
    props.ref,
    () => ({
      sayHi: () => {
        console.log("Hi~");
      },
    }),
    []
  );

  return (
    <div>
      <div>{props.title}</div>
      <div>{props.children}</div>
    </div>
  );
};

export { Card };
```

App.tsx

```tsx
import { useRef } from "react";
import { Card, type CardRef } from "./components/Card";

function App() {
  const cardRef = useRef<CardRef>(null);
  return (
    <Card title="哈哈" ref={cardRef}>
      <div>Children</div>
      <button
        onClick={() => {
          console.log("cardRef", cardRef);
          cardRef.current?.sayHi();
        }}
      >
        触发子组件方法
      </button>
    </Card>
  );
}

export default App;
```
