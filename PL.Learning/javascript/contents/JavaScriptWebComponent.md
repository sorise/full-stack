## [Web Components](#)
> **介绍**：Web Components 也是一个浏览器原生支持的组件化方案，允许你创建新的自定义、可封装的HTML 标记，使用时不用加载任何额外的模块。自定义组件和小部件基于 Web Components 标准构建，可跨现代浏览器工作，并可与任何支持 HTML 的 JavaScript 库或框架一起使用。

----
- [1. Web Component 基本概念](#)

---
### [1. Web Component 基本概念](#)
Web Component 的目的也很明确，从原生层面实现组件化，使开发者开发、复用、扩展自定义组，实现自定义标签。意味着前端开发人员开发组件时可以实现 Write once, run anywhere。

从VUE、React来看一个组件有：
- 自己的 JavaScript 类。
- DOM 结构，并且只由自己的类管理，无法被外部代码操作。（「封装」原则）。
- CSS 样式，作用在这个组件上。
- API：事件，类方法等等，让组件可以与其他组件交互。

现在已经有了很多框架和开发方法论可以实现组件化，它们各个都有自己的卖点。通常情况下，采用特殊的 CSS 类命名和一些规范，已经可以带来「组件化的感觉」 —— 即 CSS 作用域和 DOM 封装。

Web Components 本身不是一个单独的规范，也不是一门单一的技术，而是由一组 DOM API 和 HTML 规范所组成。它其中就包含了：

- **Custom elements（自定义元素）** —— 一组 JavaScript API，允许你定义 custom elements 及其行为，然后可以在你的用户界面中按照需要使用它们。.
- **Shadow DOM** —— 一组 JavaScript API，用于将封装的“影子”DOM 树附加到元素（与主文档 DOM 分开呈现）并控制其关联的功能。通过这种方式，你可以保持元素的功能私有，这样它们就可以被脚本化和样式化，而不用担心与文档的其他部分发生冲突。
    - CSS Scoping —— 申明仅应用于组件的 Shadow DOM 内的样式。
    - Event retargeting 以及更多的小东西，让自定义组件更适用于开发工作。
- **HTML template（HTML 模板）**： `<template>` 和 `<slot>` 元素使你可以编写不在呈现页面中显示的标记模板。然后它们可以作为自定义元素结构的基础被多次重用。

**实现 web component 的基本方法通常如下所示：**
1. 自定义元素作为一个类来实现，该类可以扩展 HTMLElement（在独立元素的情况下）或者你想要定制的接口（在自定义内置元素的情况下）。
2. 使用 `CustomElementRegistry.define()` 方法注册你的新自定义元素，并向其传递要定义的元素名称、指定元素功能的类、以及可选的其所继承自的元素。
3. 如果需要的话，使用 `Element.attachShadow()`方法将一个 shadow DOM 附加到自定义元素上。使用通常的 DOM 方法向 shadow DOM 中添加子元素、事件监听器等等。
4. 如果需要的话，使用 `<template>` 和 `<slot>` 定义一个 HTML 模板。再次使用常规 DOM 方法克隆模板并将其附加到你的 shadow DOM 中。
5. 在页面任何你喜欢的位置使用自定义元素，就像使用常规 HTML 元素那样。


### [2. 使用自定义元素 Custom elements](#)
我们可以通过描述带有自己的方法、属性和事件等的类来创建自定义 HTML 元素。在 custom elements （自定义标签）定义完成之后，我们可以将其和 HTML 的内建标签一同使用。

有两种类型的自定义元素：
- 自定义内置元素（Customized built-in element）继承自标准的 HTML 元素，例如 HTMLImageElement 或 HTMLParagraphElement。它们的实现定义了标准元素的行为。
- 独立自定义元素（Autonomous custom element）继承自 HTML 元素基类 HTMLElement。你必须从头开始实现它们的行为。

在 custom elements （自定义标签）定义完成之后，我们可以将其和 HTML 的内建标签一同使用。

这是一件好事，因为虽然 HTML 有非常多的标签，但仍然是有穷尽的。如果我们需要像 `<easy-tabs>、<sliding-carousel>、<beautiful-upload>……` 这样的标签，内建标签并不能满足我们。

[基本API和教材请看.....](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_components/Using_custom_elements)

```js
// 为这个元素创建类
class MyCustomElement extends HTMLElement {
  static observedAttributes = ["color", "size"];

  constructor() {
    // 必须首先调用 super 方法
    super();
  }

  connectedCallback() {
    console.log("自定义元素添加至页面。");
  }

  disconnectedCallback() {
    console.log("自定义元素从页面中移除。");
  }

  adoptedCallback() {
    console.log("自定义元素移动至新页面。");
  }

  attributeChangedCallback(name, oldValue, newValue) {
    console.log(`属性 ${name} 已变更。`);
  }
}

customElements.define("my-custom-element", MyCustomElement);
```