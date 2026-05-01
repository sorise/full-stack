## [嵌入式选择符 Nested Selectors](#)
CSS & 嵌套选择器明确指示在使用 CSS 嵌套时，父规则和子规则的关系。它使得内嵌子规则的选择器相对于其父元素。这极大地提高了样式表的可读性和维护性，尤其是对于组件化的样式。它在概念上与 SASS/LESS 等预处理器中的嵌套非常相似，但它是原生的 CSS 特性。

> 自 December 2023 起，此特性已在最新浏览器中得到支持。但在较旧的设备或浏览器中可能无法运行。
> [CSS 嵌套](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_nesting)

嵌套选择器允许你将子元素的样式规则写在父元素规则的内部。

你可以直接使用子元素的标签名、类名或 ID 选择器等

```css
.card {
  /* .card 的样式 */
  background-color: #f0f0f0;
  padding: 16px;

  /* 嵌套选择器：针对 .card 内部的 h3 元素 */
  h3 {
    color: #333;
    margin-bottom: 8px;
  }

  /* 嵌套选择器：针对 .card 内部的 .button 元素 */
  .button {
    padding: 8px 12px;
    background-color: blue;
  }
}
```

### 使用 & 符号

在某些情况下，特别是在涉及伪类、伪元素或需要引用父选择器自身时，你需要使用 & 符号。

```css
.button {
  background-color: blue;
  color: white;

  /* 嵌套伪类：表示 .button:hover */
  &:hover {
    background-color: darkblue;
  }

  /* 嵌套类名：表示 .button.primary */
  &.primary {
    font-weight: bold;
  }
}
```

虽然嵌套很方便，但在使用时也容易遇到一些问题。

这是使用嵌套最常见的问题。当你嵌套时，浏览器实际上会把它解析成一个更“长”的选择器（例如，.card h3），这会导致选择器的详细度提高。

问题所在
过度嵌套很容易导致你的样式难以被覆盖（!important 除外），因为它们的详细度可能高于你预期的其他规则。

```css
/* style.css */
.parent-container { /* 详细度: 0,1,0 */
  /* ... 样式 ... */

  .child-component { /* 解析为 .parent-container .child-component (详细度: 0,2,0) */
    /* ... 样式 ... */

    .text-element { /* 解析为 .parent-container .child-component .text-element (详细度: 0,3,0) */
      color: red; /* 很难被外部的 .text-element 覆盖 */
    }
  }
}

/* 外部样式尝试覆盖 (详细度: 0,1,0)，但失败了 */
.text-element { 
  color: blue; /* 不生效 */
}
```

虽然嵌套是为了提高可读性，但如果嵌套层级超过 3-4 层，文件会变得非常难以阅读和维护，你很难一眼看出某个规则到底应用到了哪个元素上。

过深的嵌套会混淆选择器之间的关系，违反了扁平化和 BEM 等命名规范所推崇的低详细度原则。

尽管现代浏览器（如 Chrome, Firefox, Safari）大多已支持原生的 CSS 嵌套，但在一些较旧或特定环境下，它可能尚未得到完全支持。

如果你需要广泛的浏览器兼容性，或者已经习惯了嵌套，可以继续使用 CSS 预处理器。它们的功能更强大，并且通过编译确保了所有浏览器都能理解最终的 CSS 代码。

```css
/* SCSS */
.card {
  background-color: #f0f0f0;

  // 预处理器会在编译时生成 .card h3
  h3 {
    color: #333;
  }

  // 预处理器会在编译时生成 .card:hover
  &:hover { 
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  }
}
```

### 后附 & 嵌套选择器
& 嵌套选择器也可以添加到一个选择器的后方，来反转上下文。

```css
.card {
  /* .card 的样式 */
  .featured & {
    /* .featured .card 的样式 */
  }
}

/* 浏览器会将以上嵌套规则解析为 */

.card {
  /* .card 的样式 */
}

.featured .card {
  /* .featured .card 的样式 */
}
```

#### 多次使用
& 嵌套选择器可以在一个选择器里多次使用：
```
.card {
  /* .card 的样式 */
  .featured & & & {
    /* .featured .card .card .card 的样式 */
  }
}

/* 浏览器会将以上嵌套规则解析为 */

.card {
  /* .card 的样式 */
}

.featured .card .card .card {
  /* .featured .card .card .card 的样式 */
}
```

