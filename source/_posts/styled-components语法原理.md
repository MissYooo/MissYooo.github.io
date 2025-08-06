---
title: styled-components语法原理
date: 2025-08-06 10:22:17
tags: JS
---

```js
const div = function (strArr, ...args) {
  return strArr.reduce((result, str, i) => {
    return result + str + (args[i] || "");
  }, "");
};

const a = div`
  color:red;
  width:${30}px;
  height:${50}px;
`;
console.log(a);
//  输出结果
//  color:red;
//  width:30px;
//  height:50px;
```
