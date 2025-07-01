---
title: webworker导出Excel示例
date: 2025-07-01 15:26:24
tags:
---

App.vue

```vue
<script setup lang="ts">
import { writeFile } from "xlsx";
// --Vite可以使用以下方式导入worker
// --https://cn.vitejs.dev/guide/features.html#web-workers--
// import MyWorker from "./utils/worker?worker";
// const worker: Worker = new MyWorker();

const worker: Worker = new Worker(
  new URL("./utils/worker.js", import.meta.url),
  {
    type: "module",
  }
);

const handleExportExcel = () => {
  worker.postMessage([
    ["姓名", "年龄", "城市"],
    ["张三", 25, "上海"],
    ["李四", 30, "北京"],
  ]);
};

worker.addEventListener("message", (e) => {
  // 将工作簿数据写入文件，'exported_data.xlsx'为导出文件的名称
  writeFile(e.data, "export.xlsx");
});
</script>

<template>
  <div>Web Worker导出Excel测试</div>
  <button @click="handleExportExcel">导出Excel</button>
</template>

<style scoped></style>
```

./utils/worker.ts

```ts
import { utils } from "xlsx";

self.addEventListener("message", (e) => {
  const data = e.data;
  // 将二维数组转换为工作表对象
  const ws = utils.aoa_to_sheet(data);
  // 创建一个新的工作簿对象
  const wb = utils.book_new();
  // 将工作表对象添加到工作簿中，'Sheet1'为工作表的名称
  utils.book_append_sheet(wb, ws, "Sheet1");
  self.postMessage(wb);
});
```
