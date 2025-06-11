---
title: Node.js-文件上传案例
date: 2025-06-04 16:31:25
tags: Node.js
---

### Formidable 模块

**index.html**

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>上传文件Demo</title>
  </head>
  <body>
    <form method="post" action="/upload" enctype="multipart/form-data">
      <input type="file" name="file" />
      <button type="submit">上传</button>
    </form>
  </body>
</html>
```

**index.js**

```javascript
import http from "http";
import fs from "fs";
import formidable from "formidable";

const form = formidable({
  uploadDir: "./resource",
  keepExtensions: true,
});

const server = http.createServer(async (req, res) => {
  const url = req.url;
  switch (url) {
    case "/upload":
      if (req.method === "POST") {
        form.parse(req, (err) => {
          if (err) {
            res.writeHead(500, {
              "content-type": "text/plain",
            });
            res.end("上传出错");
            return;
          }
          res.writeHead(200, {
            "content-type": "text/plain",
          });
          res.end("上传成功");
        });
      }
      break;
    // 显示上传表单
    case "/":
      fs.readFile("index.html", "utf-8", (_err, data) => {
        res.writeHead(200, {
          "content-type": "text/html",
        });
        res.end(data);
      });
      break;
    default:
      break;
  }
});

server.listen(5000);
```
