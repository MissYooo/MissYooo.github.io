---
title: Node.js 文件系统模块
date: 2025-06-03 16:06:10
tags: Node.js
---

### 读取文件

回调版本

```js
import fs from "fs";
fs.readFile("package.json", "utf8", (err, data) => {
  console.log(data);
});
```

Promise 版本

```js
import { promises as fs } from "fs";

const fn = async () => {
  const data = await fs.readFile("package.json", "utf8");
  console.log(data);
};
fn();
```

### 写入文件

appendFile

```js
import fs from "fs";

fs.appendFile("fs.txt", "Hello Node", (err) => {
  console.log(err);
});
```

writeFile-会替换指定的文件和内容

```
略...
```

### 删除文件

```js
import fs from "fs";

fs.unlink("fs.txt", (err) => {
  console.log(err);
});
```

### 重命名文件

```js
import fs from "fs";

fs.rename("fs.txt", "fs-new.txt", (err) => {
  console.log(err);
});
```
