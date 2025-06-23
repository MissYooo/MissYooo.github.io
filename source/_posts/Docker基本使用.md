---
title: Docker基本使用
date: 2025-06-23 13:43:22
tags: Docker
---

### 参考

[【docker 入门】10 分钟，快速学会 docker](https://www.bilibili.com/video/BV1R4411F7t9/?share_source=copy_web&vd_source=2a7f2edcf0f3b5511476aedeb092f4c5)

[【docker 入门 2】实战~如何组织一个多容器项目 docker-compose](https://www.bilibili.com/video/BV1Wt411w72h/?share_source=copy_web&vd_source=2a7f2edcf0f3b5511476aedeb092f4c5)

[【Docker】Dockerfile 用法全解析](https://www.bilibili.com/video/BV1k7411B7QL/?share_source=copy_web&vd_source=2a7f2edcf0f3b5511476aedeb092f4c5)

### 前端项目实操

**Dockerfile**

```DockerFile
FROM node:24-alpine AS stage1
WORKDIR /root/web
RUN npm install -g pnpm
COPY ./package.json ./
RUN pnpm install
COPY ./ ./
RUN pnpm build

FROM nginx:stable-alpine
COPY --from=stage1 /root/web/dist /usr/share/nginx/html
EXPOSE 80
```

---

通过先只复制 package.json 并运行 pnpm install，Docker 可以缓存这一层
只有当 package.json 发生变化时，才会重新安装依赖

---

**docker-compose.yaml**

```yaml
version: "3"
services:
  vue3-web:
    container_name: "vue3-web"
    image: "vue3-web"
    ports:
      - "80:80"
```

`docker compose up -d`启动多个容器
