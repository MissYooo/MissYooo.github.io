---
title: pnpm dlx 命令详解
date: 2025-06-24 15:13:26
tags: pnpm
---

# `pnpm dlx` 命令详解

`pnpm dlx` 是 pnpm 包管理器提供的一个便捷命令，用于临时下载并执行 npm 包中的命令，而不需要将这些包永久安装到项目中。

## 基本语法

```bash
pnpm dlx <package> [args...]
```

## 功能特点

1. **临时执行**：下载包、运行命令，然后自动清理，不会将包添加到项目的 `node_modules` 或 `package.json` 中
2. **无需安装**：适合只需要运行一次的 CLI 工具
3. **节省空间**：避免为一次性使用的工具永久安装依赖

## 使用示例

### 创建 React 应用（临时使用 create-react-app）

```bash
pnpm dlx create-react-app my-app
```

### 使用 Vite 脚手架

```bash
pnpm dlx create-vite my-project --template react
```

### 运行代码格式化工具

```bash
pnpm dlx prettier --write .
```

## 与类似命令的比较

| 命令       | 包管理器 | 特点                     |
| ---------- | -------- | ------------------------ |
| `pnpm dlx` | pnpm     | 临时执行，自动清理       |
| `npx`      | npm      | 类似功能，但缓存策略不同 |
| `yarn dlx` | Yarn     | 与 pnpm dlx 类似         |

## 工作原理

1. 从 npm 注册表下载指定的包
2. 执行包中定义的二进制命令
3. 完成后自动删除下载的包

## 适用场景

- 运行项目脚手架工具（如 create-react-app、vite 等）
- 使用一次性代码生成或转换工具
- 执行不需要长期依赖的 CLI 工具

`pnpm dlx` 是 pnpm 用户替代 `npx` 的一个高效选择，特别适合在保持项目依赖整洁的同时临时使用各种 npm 包工具。
