---
title: TypeScript类型体操-参数提取
date: 2025-06-11 10:45:54
tags: TypeScript
---

关键思路: 递归+模板字符串类型

```ts
type path1 = "/user/:id/post/:num";
type path2 = "/user/:id/:num";
type ExtractRouteParams<T> = T extends `${string}:${infer Param}/${infer Rest}`
  ? Param | ExtractRouteParams<`${Rest}`>
  : T extends `${string}:${infer Param}`
  ? Param
  : never;

type params1 = ExtractRouteParams<path1>;
type params2 = ExtractRouteParams<path2>;
```
