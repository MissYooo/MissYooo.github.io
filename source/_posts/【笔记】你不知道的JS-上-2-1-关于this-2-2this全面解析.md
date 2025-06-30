---
title: 【笔记】你不知道的JS(上)-2.1.关于this&&2.2this全面解析
date: 2025-06-30 11:34:20
tags: JS
---

本章系统性地介绍了 JavaScript 中的 `this` 机制，解释了 `this` 的绑定规则、常见误区，以及如何正确理解和使用 `this`。

---

### **详细总结**

#### **1. `this` 的基本概念**

- **定义**：`this` 是一个**执行上下文**（Execution Context）的绑定，指向函数被调用时的关联对象。
- **核心特点**：
  - `this` **不是编写时绑定，而是运行时绑定**，取决于函数的调用方式。
  - `this` **不指向函数自身**，也不指向函数的词法作用域（常见误区）。

#### **2. `this` 的绑定规则**

JavaScript 中 `this` 的绑定遵循**四种规则**，优先级从低到高依次为：

##### **(1) 默认绑定（Default Binding）**

- **场景**：独立函数调用（无任何修饰的调用）。
- **规则**：
  - 非严格模式下，`this` 指向全局对象（浏览器中为 `window`，Node.js 中为 `global`）。
  - 严格模式下（`"use strict"`），`this` 为 `undefined`。
- **示例**：
  ```javascript
  function foo() {
    console.log(this.a);
  }
  var a = 2;
  foo(); // 2（默认绑定到全局对象）
  ```

##### **(2) 隐式绑定（Implicit Binding）**

- **场景**：函数作为对象的属性被调用。
- **规则**：`this` 指向调用该函数的**上下文对象**。
- **示例**：
  ```javascript
  var obj = {
    a: 2,
    foo: function () {
      console.log(this.a);
    },
  };
  obj.foo(); // 2（this指向obj）
  ```
- **隐式丢失问题**：  
  若函数被赋值或作为回调传递，可能丢失 `this` 绑定（退化为默认绑定）：
  ```javascript
  var bar = obj.foo;
  bar(); // undefined（默认绑定到全局）
  ```

##### **(3) 显式绑定（Explicit Binding）**

通过 `call`、`apply` 或 `bind` 强制指定 `this` 的绑定对象。

- **`call` / `apply`**：立即执行函数，并显式绑定 `this`。
  ```javascript
  function foo() {
    console.log(this.a);
  }
  var obj = { a: 2 };
  foo.call(obj); // 2
  ```
- **`bind`**：返回一个硬绑定函数，`this` 不可更改。
  ```javascript
  var boundFoo = foo.bind(obj);
  boundFoo(); // 2
  ```

##### **(4) `new` 绑定（New Binding）**

- **场景**：通过 `new` 调用构造函数。
- **规则**：`this` 指向新创建的实例对象。
- **示例**：
  ```javascript
  function Foo(a) {
    this.a = a;
  }
  var bar = new Foo(2);
  console.log(bar.a); // 2
  ```

#### **3. 绑定规则的优先级**

优先级从高到低：

1. `new` 绑定 > 2. 显式绑定 > 3. 隐式绑定 > 4. 默认绑定。

#### **4. 箭头函数的 `this`**

- **特点**：箭头函数没有自己的 `this`，而是继承外层词法作用域的 `this`（与普通函数不同）。
- **示例**：
  ```javascript
  function foo() {
    setTimeout(() => {
      console.log(this.a); // 继承foo的this
    }, 100);
  }
  var obj = { a: 2 };
  foo.call(obj); // 2
  ```

#### **5. 常见误区与注意事项**

- **误区 1**：认为 `this` 指向函数自身（实际需通过 `arguments.callee` 或函数名访问）。
- **误区 2**：混淆词法作用域和 `this`（`this` 不指向函数定义时的环境）。
- **建议**：
  - 使用 `bind` 或箭头函数避免隐式丢失。
  - 避免在回调中依赖默认绑定（严格模式更安全）。

---

### **关键结论**

- `this` 的绑定是**动态的**，由函数调用方式决定。
- 掌握四种绑定规则（默认、隐式、显式、`new`）是理解 `this` 的核心。
- 箭头函数的 `this` 行为特殊，需与普通函数区分。

**关联章节**：

- 第 2 章（对象）和第 3 章（混合对象“类”）将深入探讨 `this` 在面向对象中的应用。
