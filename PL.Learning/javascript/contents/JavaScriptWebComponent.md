## [Web Components](#)
> **介绍**：Web Components 也是一个浏览器原生支持的组件化方案，允许你创建新的自定义、可封装的HTML 标记，使用时不用加载任何额外的模块。自定义组件和小部件基于 Web Components 标准构建，可跨现代浏览器工作，并可与任何支持 HTML 的 JavaScript 库或框架一起使用。

----
- 

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

- Custom elements —— 用于自定义 HTML 元素.
- Shadow DOM —— 为组件创建内部 DOM，它对外部是不可见的。
- CSS Scoping —— 申明仅应用于组件的 Shadow DOM 内的样式。
- Event retargeting 以及更多的小东西，让自定义组件更适用于开发工作。

### [2. Custom elements](#)
我们可以通过描述带有自己的方法、属性和事件等的类来创建自定义 HTML 元素。在 custom elements （自定义标签）定义完成之后，我们可以将其和 HTML 的内建标签一同使用。

在 custom elements （自定义标签）定义完成之后，我们可以将其和 HTML 的内建标签一同使用。

这是一件好事，因为虽然 HTML 有非常多的标签，但仍然是有穷尽的。如果我们需要像 `<easy-tabs>、<sliding-carousel>、<beautiful-upload>……` 这样的标签，内建标签并不能满足我们。