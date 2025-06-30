---
title: CSS linear-gradient
date: 2025-06-30 14:38:03
tags: CSS
---

CSS 的 `linear-gradient()` 函数是用于创建**线性渐变背景**的强大工具。它允许开发者通过指定颜色、方向和颜色停止点（color stops），生成平滑的颜色过渡效果。以下是关于 `linear-gradient()` 的详细解析：

---

### **1. 基本概念**

线性渐变（Linear Gradient）是一种颜色沿**直线方向**从一种颜色过渡到另一种颜色（或多种颜色）的效果。它可以通过以下方式定义：

- **方向**：指定渐变的方向（如从上到下、从左到右、倾斜角度等）。
- **颜色**：定义渐变的起始颜色、中间颜色（可选）和结束颜色。
- **颜色停止点**：控制颜色在渐变中的具体位置（百分比或长度值）。

---

### **2. 语法**

`linear-gradient()` 的基本语法如下：

```css
background-image: linear-gradient(
  [方向或角度],
  [颜色停止点1],
  [颜色停止点2],
  ...
);
```

#### **2.1 方向参数**

方向可以通过以下方式定义：

- **关键字**：`to top`（从下到上）、`to bottom`（从上到下，默认）、`to left`（从右到左）、`to right`（从左到右）。
- **角度**：用 `deg` 表示角度，例如 `45deg`（从左下到右上）。
  - **角度规则**：
    - `0deg` 等同于 `to top`（从下到上）。
    - `90deg` 等同于 `to right`（从左到右）。
    - `180deg` 等同于 `to bottom`（从上到下）。
    - `270deg` 等同于 `to left`（从右到左）。

#### **2.2 颜色停止点**

颜色停止点由颜色值和可选的位置值组成：

- **颜色值**：可以是颜色名称（如 `red`）、十六进制（如 `#ff0000`）、RGB（如 `rgb(255,0,0)`）或 RGBA（支持透明度）。
- **位置值**：用百分比（如 `50%`）或长度单位（如 `10px`）指定颜色在渐变中的位置。
  - 如果未指定位置，颜色会**均匀分布**在整个渐变区域。
  - 如果指定位置，颜色会在该位置开始过渡。

---

### **3. 示例代码**

#### **3.1 基础示例**

```css
/* 从上到下的线性渐变（默认方向） */
background-image: linear-gradient(red, yellow);

/* 从左到右的线性渐变 */
background-image: linear-gradient(to right, red, yellow);

/* 倾斜 45 度的线性渐变 */
background-image: linear-gradient(45deg, red, yellow);
```

#### **3.2 多色渐变**

```css
/* 从上到下，红→绿→蓝 */
background-image: linear-gradient(red, green, blue);

/* 自定义颜色位置 */
background-image: linear-gradient(
  to right,
  red 0%,
  /* 起始颜色在 0% 位置 */ yellow 50%,
  /* 中间颜色在 50% 位置 */ blue 100% /* 结束颜色在 100% 位置 */
);
```

#### **3.3 透明渐变**

```css
/* 从透明到红色的渐变 */
background-image: linear-gradient(
  rgba(255, 0, 0, 0),
  /* 透明 */ rgba(255, 0, 0, 1) /* 不透明的红色 */
);
```

#### **3.4 多方向复杂渐变**

```css
/* 从左下到右上的渐变，颜色为橙色→黄色→蓝绿色 */
background-image: linear-gradient(to top right, orange, yellow 50%, turquoise);
```

---

### **4. 颜色停止点的细节**

- **硬过渡**：如果两个颜色停止点位置相同，会形成**硬边界的过渡**（无平滑效果）。

  ```css
  background-image: linear-gradient(
    red 50%,
    yellow 50% /* 红色和黄色在 50% 位置相遇，形成硬边界 */
  );
  ```

- **重复渐变**：使用 `repeating-linear-gradient()` 可以创建重复的渐变模式。
  ```css
  background-image: repeating-linear-gradient(
    45deg,
    red 0px,
    yellow 10px,
    blue 20px
  );
  ```

---

### **5. 应用场景**

1. **按钮和背景设计**：  
   创建渐变按钮或页面背景，提升视觉吸引力。

   ```css
   .button {
     background: linear-gradient(to right, #ff7e5f, #feb47b);
     padding: 10px 20px;
     border: none;
     color: white;
   }
   ```

2. **装饰元素**：  
   用于装饰线条、分隔符或图标背景。

   ```css
   .divider {
     height: 2px;
     background: linear-gradient(to right, transparent, #000, transparent);
   }
   ```

3. **动态效果**：  
   结合 CSS 动画，实现渐变动画效果。

   ```css
   @keyframes gradientMove {
     0% {
       background-position: 0% 50%;
     }
     100% {
       background-position: 100% 50%;
     }
   }

   .animated-gradient {
     background: linear-gradient(45deg, red, yellow, blue);
     background-size: 200% 200%;
     animation: gradientMove 5s linear infinite;
   }
   ```

---

### **6. 兼容性**

- **现代浏览器**：所有主流浏览器（Chrome、Firefox、Safari、Edge）均支持 `linear-gradient()`。
- **旧版浏览器**：IE 10+ 支持 `-ms-linear-gradient`，但需要添加前缀。
  ```css
  /* IE 10+ 兼容性写法 */
  background-image: -ms-linear-gradient(45deg, red, yellow);
  background-image: linear-gradient(45deg, red, yellow);
  ```

---

### **7. 总结**

- `linear-gradient()` 是 CSS3 中实现渐变效果的核心工具，支持灵活的方向、颜色和位置控制。
- 通过合理设置颜色停止点和方向，可以创建从简单双色渐变到复杂多色渐变的背景。
- 实际开发中，建议结合在线工具（如 [CSS Gradient Generator](https://www.jyshare.com/front-end/9921/)）快速生成渐变代码，并测试不同浏览器的效果。
