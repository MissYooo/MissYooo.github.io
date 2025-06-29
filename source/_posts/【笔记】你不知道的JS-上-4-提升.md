---
title: 【笔记】你不知道的JS(上)-4.提升
date: 2025-06-29 11:57:11
tags: JS
---

第 4 章详细解释了 JavaScript 中的**变量提升（Hoisting）**机制，包括函数和变量的声明如何被“提升”至作用域顶部，以及 `var` 和 `let/const` 在提升行为上的关键区别。

---

### **详细总结**

#### **1. 变量提升（Hoisting）的基本概念**

- **定义**：JavaScript 引擎在代码执行前会先扫描作用域内的变量和函数声明，并将其“提升”至作用域顶部。
- **影响**：
  - `var` 声明的变量会被提升，但赋值不会（初始值为 `undefined`）。
  - 函数声明（`function foo() {}`）会被整体提升，而函数表达式（`var foo = function() {}`）仅提升变量声明。

#### **2. `var` 的变量提升**

- **示例**：
  ```javascript
  console.log(a); // undefined（变量提升）
  var a = 2;
  ```
  - 实际执行顺序相当于：
    ```javascript
    var a; // 声明被提升
    console.log(a); // undefined
    a = 2; // 赋值保留原位
    ```

#### **3. 函数声明 vs 函数表达式**

- **函数声明**：整体提升，可在声明前调用。
  ```javascript
  foo(); // "正常执行"
  function foo() {
    console.log("正常执行");
  }
  ```
- **函数表达式**：仅变量声明提升，赋值不提升。
  ```javascript
  bar(); // TypeError: bar is not a function
  var bar = function () {
    console.log("不会执行");
  };
  ```

#### **4. `let` 和 `const` 的暂时性死区（TDZ）**

- **与 `var` 的区别**：
  - `let/const` 声明也会提升，但在赋值前访问会触发 **ReferenceError**（暂时性死区）。
  - 块级作用域限制变量仅在 `{}` 内有效。
- **示例**：
  ```javascript
  console.log(b); // ReferenceError: Cannot access 'b' before initialization
  let b = 2;
  ```

#### **5. 提升的底层机制**

- **编译阶段**：JavaScript 引擎会先处理声明（变量和函数），再执行代码。
- **函数优先**：同作用域下，函数声明会覆盖变量声明（但变量赋值会覆盖函数）。
  ```javascript
  foo(); // 3（函数声明覆盖同名变量）
  var foo;
  function foo() {
    console.log(1);
  }
  foo = function () {
    console.log(2);
  };
  foo(); // 2（赋值覆盖函数）
  ```

#### **6. 最佳实践**

- **避免依赖提升**：始终先声明再使用变量，减少意外行为。
- **使用 `let/const`**：取代 `var`，利用块作用域和 TDZ 减少错误。
- **函数声明优先**：若需函数提升，使用声明而非表达式。

---

### **关键结论**

- **提升是编译阶段的行为**，而非代码物理位置的移动。
- `var` 的变量提升可能导致逻辑错误，而 `let/const` 通过 TDZ 提供更安全的编程模式。
- 理解提升有助于调试“变量未定义”或“函数覆盖”等问题。

**关联章节**：

- 第 3 章（作用域）是提升的基础，第 5 章（闭包）将结合作用域链进一步扩展。
