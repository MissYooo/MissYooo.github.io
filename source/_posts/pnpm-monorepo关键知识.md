---
title: pnpm+monorepo关键知识
date: 2025-06-10 20:57:34
tags: pnpm
---

### 1. 工作区配置

```yaml
packages:
  - "packages/*"
  - "apps/*"
```

### 2. 常用命令

```bash
# 全局安装依赖
pnpm add <pkg> -w
# 为特定项目安装依赖
pnpm add <pkg> --filter <project>
# 运行特定项目的脚本
pnpm run --filter <project> <script>
```

### 3. workspace: 协议

```json
{
  "dependencies": {
    "shared-utils": "workspace:*"
  }
}
```
