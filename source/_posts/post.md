---
title: 近期前端常用包汇总
date: 2025-06-18 08:38:23
tags: npm包
---

### 1. cross-env

跨平台设置环境变量的工具，解决不同操作系统（如 Windows 和 Unix）的环境变量语法兼容问题。

```json
{
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
  }
}
```

NODE_ENV 会被注入到`process.env`

### 2. dotenv

从 .env 文件加载环境变量到 process.env，方便在开发中管理敏感配置。

```ts
import { config } from "dotenv";

config({
  path: [".env", `.env.${process.env.NODE_ENV}`],
  override: true,
});
```

可以结合 cross-env 使用

### 3.zod

TypeScript 优先的运行时数据验证库，提供简洁的 Schema 定义和类型推断。

案例：使用 zod 获取使用安全的 env

```ts
import type { ZodError } from "zod/v4";
import process from "node:process";
import { config } from "dotenv";
import { z } from "zod/v4";

config({
  // eslint-disable-next-line node/no-process-env
  path: [".env", `.env.${process.env.NODE_ENV}`],
  override: true,
});

export const EnvSchema = z.object({
  NODE_ENV: z.enum(["development", "production"]).default("development"),
  PORT: z.coerce.number().default(3000),
});

let env: z.infer<typeof EnvSchema>;

try {
  // eslint-disable-next-line node/no-process-env
  env = EnvSchema.parse(process.env);
} catch (e) {
  const error = e as ZodError;
  console.error("❌ env错误");
  console.error(JSON.stringify(z.treeifyError(error)));
  process.exit(1);
}

export default env;
```

### 4. tsup

基于 ESBuild 的零配置 TypeScript 打包工具，支持快速构建库和应用程序。

tsup.config.ts

```ts
import { defineConfig } from "tsup";

export default defineConfig({
  entry: ["src/index.ts"],
  format: ["cjs"],
  platform: "node",
  target: "node22",
  clean: true,
});
```

### 5.tsx

类似 ts-node 的 TypeScript 运行时，可直接执行 .ts 文件，支持 ESM 和观察模式（watch）。

```bash
tsx watch src/index.ts
```
