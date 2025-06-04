---
title: Node.js WHATWG URL API
date: 2025-06-04 09:34:47
tags: Node.js
---

### 基本语法

```javascript
const url = new URL(input[, base]);
```

### 示例

```javascript
const url = new URL("https://example.org:8080/foo/bar?a=1&b=2#hash");
console.log(url);
// {
//   href: 'https://example.org:8080/foo/bar?a=1&b=2#hash',
//   origin: 'https://example.org:8080',
//   protocol: 'https:',
//   username: '',
//   password: '',
//   host: 'example.org:8080',
//   hostname: 'example.org',
//   port: '8080',
//   pathname: '/foo/bar',
//   search: '?a=1&b=2',
//   searchParams: URLSearchParams { 'a' => '1', 'b' => '2' },
//   hash: '#hash'
// }
console.log(Object.fromEntries(url.searchParams.entries()));
// { a: '1', b: '2' }
```
