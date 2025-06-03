---
title: Node.js HTTP 模块
date: 2025-06-03 15:59:56
tags: Node.js
---

```js
import http from "http";

const server = http.createServer(async (req, res) => {
  console.log("req.url", req.url);
  res.writeHead(200, {
    "Content-Type": "text/html",
  });
  res.write("Hello");
  res.write("Node");
  await new Promise((resolve) => setTimeout(resolve, 2000));
  res.end("!");
});

server.listen(5000);
```
