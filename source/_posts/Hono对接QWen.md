---
title: Hono对接QWen
date: 2025-06-20 16:31:40
tags: [AI]
---

### 参考

[通义前问 API 参考](https://help.aliyun.com/zh/model-studio/use-qwen-by-calling-api#4ec3e641c294d)
[可读字节数据流](https://developer.mozilla.org/zh-CN/docs/Web/API/ReadableStream)
[Response 对象](https://developer.mozilla.org/zh-CN/docs/Web/API/Response/Response)
[EventSource](https://developer.mozilla.org/zh-CN/docs/Web/API/EventSource)

### Hono.js 示例代码

chat.service.ts

```ts
import openai from "@/lib/openai.ts";

export const chatService = {
  chat: (content: string) => {
    const rs = new ReadableStream<string>({
      start: async (controller) => {
        const completion = await openai.chat.completions.create({
          model: "qwen-plus",
          messages: [{ role: "user", content }],
          stream: true,
          stream_options: {
            include_usage: true,
          },
        });
        for await (const chunk of completion) {
          if (Array.isArray(chunk.choices) && chunk.choices.length > 0) {
            console.log(chunk.choices[0].delta.content);
            controller.enqueue(`data: ${chunk.choices[0].delta.content}\n\n`);
          }
        }
        controller.enqueue(`data: [DONE]\n\n`);
        controller.close();
      },
    });
    return rs;
  },
};
```

chat.controller.ts

```ts
import { ClientError } from "@orbit/server";
import { createRouter } from "@/lib/creata-app.ts";
import { chatService } from "./service.ts";

export const chat = createRouter().basePath("/chat");

chat.get("/", async (c) => {
  const content = c.req.query("content");
  if (content === undefined) {
    throw new ClientError({
      message: "请输入您的问题",
    });
  }
  const rs = chatService.chat(content);
  return new Response(rs, {
    headers: {
      "Content-Type": "text/event-stream",
      "Cache-Control": "no-cache",
      Connection: "keep-alive",
    },
  });
});
```
