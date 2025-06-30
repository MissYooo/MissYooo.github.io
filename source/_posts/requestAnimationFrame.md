---
title: requestAnimationFrame
date: 2025-06-30 15:26:18
tags: JS
---

`requestAnimationFrame`（简称 rAF）是浏览器提供的一个原生 API，专为实现**高性能动画**而设计。它的核心目标是确保动画与屏幕刷新率同步，从而提供流畅的视觉体验。以下是对其的详细介绍：

---

### **一、核心原理**

1. **与屏幕刷新率同步**

   - 浏览器通常以 **60Hz** 的频率刷新屏幕（即每秒 60 帧），每次刷新间隔约为 **16.7ms**。
   - `requestAnimationFrame` 会在每次屏幕刷新前调用注册的回调函数，确保动画更新与屏幕刷新同步，避免卡顿或掉帧。

2. **浏览器渲染流程中的调用时机**

   - 浏览器的完整帧流程包括：  
     **事件处理 → rAF 回调 → 样式计算 → 布局（Layout） → 绘制（Paint） → 合成（Composite） → 显示**
   - rAF 的回调函数在 **样式计算和布局之前** 执行，允许开发者在此时修改 DOM 或 CSS 属性，确保后续渲染使用最新的状态。

3. **自动优化与资源管理**
   - **页面不可见时暂停**：当用户切换到其他标签页或浏览器最小化时，rAF 会自动暂停，节省 CPU/GPU 资源。
   - **动态调整帧率**：在高负载设备（如移动端）或低性能场景下，浏览器会降低调用频率（如 30Hz），以保持动画流畅。

---

### **二、使用方式**

1. **基本语法**

   ```javascript
   const requestId = requestAnimationFrame(callback);
   ```

   - **`callback(timestamp)`**：回调函数，接收一个高精度时间戳（单位：毫秒），表示当前帧的执行时间。
   - **`requestId`**：请求的唯一标识符，用于通过 `cancelAnimationFrame(requestId)` 取消动画。

2. **动画循环**  
   通过递归调用 `requestAnimationFrame` 实现持续动画：

   ```javascript
   function animate(timestamp) {
     // 更新动画状态（如位置、颜色等）
     requestAnimationFrame(animate);
   }
   requestAnimationFrame(animate);
   ```

3. **时间戳的应用**  
   时间戳用于计算动画的 **相对时间**，确保动画在不同设备上运行一致：
   ```javascript
   let startTime;
   function animate(timestamp) {
     if (!startTime) startTime = timestamp;
     const progress = timestamp - startTime;
     // 根据 progress 更新动画
     if (progress < 2000) {
       requestAnimationFrame(animate);
     }
   }
   requestAnimationFrame(animate);
   ```

---

### **三、与传统定时器的对比**

| **特性**     | **`requestAnimationFrame`**                    | **`setTimeout`/`setInterval`**        |
| ------------ | ---------------------------------------------- | ------------------------------------- |
| **执行时机** | 与屏幕刷新率同步（60Hz 或更高）                | 固定时间间隔（如 16ms），易受阻塞影响 |
| **资源消耗** | 页面不可见时自动暂停，节省资源                 | 即使页面不可见仍持续执行，浪费资源    |
| **帧率控制** | 自动匹配设备刷新率（如 60Hz、120Hz）           | 需手动计算时间间隔（如 16.7ms）       |
| **性能优化** | 浏览器批量处理多个 rAF 请求，减少重绘/回流次数 | 多次调用可能导致频繁重绘/回流         |
| **适用场景** | 动画、游戏、实时更新等                         | 简单任务或非视觉相关的周期性操作      |

---

### **四、实际应用场景**

1. **网页动画**

   - 实现元素位移、缩放、旋转等动画（如粒子特效、雪花飘落）。
   - 示例：
     ```javascript
     const box = document.getElementById("box");
     let position = 0;
     function animate() {
       position += 2;
       box.style.transform = `translateX(${position}px)`;
       if (position < window.innerWidth - 50) {
         requestAnimationFrame(animate);
       }
     }
     requestAnimationFrame(animate);
     ```

2. **游戏开发**

   - 实时更新游戏状态（如角色移动、碰撞检测）。
   - 结合 `WebGL` 或 `Canvas` 实现高性能渲染。

3. **数据可视化**
   - 动态图表（如折线图、柱状图）的平滑过渡效果。

---

### **五、优点与缺点**

#### **优点**

1. **流畅性**：与屏幕刷新率同步，避免卡顿或跳帧。
2. **资源节省**：页面不可见时自动暂停，降低能耗。
3. **性能优化**：浏览器批量处理 rAF 请求，减少重绘/回流次数。
4. **跨设备适配**：自动适配不同设备的刷新率（如 60Hz、120Hz）。

#### **缺点**

1. **兼容性**：早期浏览器需添加前缀（如 `-webkit-`），但现代浏览器已广泛支持。
2. **依赖主线程**：若主线程阻塞（如执行复杂计算），动画仍可能卡顿。
3. **无法精确控制时间间隔**：rAF 的调用间隔由浏览器决定，不适合需要严格时间控制的场景。

---

### **六、最佳实践**

1. **避免阻塞主线程**：将复杂计算移至 Web Worker。
2. **及时取消动画**：使用 `cancelAnimationFrame` 避免内存泄漏。
   ```javascript
   let animationId;
   function animate() {
     animationId = requestAnimationFrame(animate);
     // 动画逻辑
   }
   animate();
   // 停止动画
   cancelAnimationFrame(animationId);
   ```
3. **结合 `performance.now()`**：使用高精度时间戳控制动画节奏。
   ```javascript
   const start = performance.now();
   function animate(timestamp) {
     const elapsed = timestamp - start;
     // 根据 elapsed 更新动画
     requestAnimationFrame(animate);
   }
   requestAnimationFrame(animate);
   ```

---

### **七、总结**

`requestAnimationFrame` 是现代网页动画的首选方案，其核心优势在于 **与浏览器渲染流程深度集成**，确保动画的流畅性与性能。相比传统定时器，它能自动适配设备特性、节省资源，并提供更精准的帧率控制。开发者应优先使用 rAF 实现视觉相关的动态效果，同时注意结合性能优化策略（如减少重绘/回流、避免主线程阻塞）以充分发挥其潜力。
