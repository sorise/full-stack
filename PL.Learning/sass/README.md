## [Sass 预处理器脚本语言](#)

> **介绍** Sass (英文全称：Syntactically Awesome Stylesheets) 是一个最初由 Hampton Catlin 设计并由 Natalie Weizenbaum 开发的层叠样式表语言。


### [1. 介绍](#)
Sass（英文全称：`Syntactically Awesome Stylesheets`）是一个最初由 Hampton Catlin 设计并由 Natalie Weizenbaum 开发的层叠样式表语言。是一种动态样式语言，Sass 语法属于缩排语法，比 CSS 多出变量、嵌套、运算、混入(Mixin)、继承、颜色处理、函数等功能，更容易阅读。

- Sass 是一个 CSS 预处理器。
- Sass 是 CSS 扩展语言，可以帮助我们减少 CSS 重复的代码，节省开发时间。
- Sass 完全兼容所有版本的 CSS。

Sass 的`缩排语法`，对于写惯 CSS 前端的 Web 开发者来说很不直观，也不能将 CSS 代码加入到 Sass 里面，因此 Sass 语法进行了改良，Sass3 就变成了 Scss(Sassy CSS) 。与原来的语法兼容，只是用 {} 取代了原来的缩进。所以 Sass 包括两套语法，通常情况下，这两套语法通过 `.sass` 和 `.scss` 两个文件扩展名区分开。

>  `缩排语法`（Indentation Syntax）是一种用缩进和换行来表示代码层级和结构的写法，不依赖 `{}` 花括号或 `;` 分号来分隔代码块和语句。在 CSS 预处理器（如 Sass）中，缩排语法主要指 Sass 的 `.sass 格式` 例如：

```sass
nav
  background: #333
  color: white
  ul
    margin: 0
    padding: 0
    list-style: none
```
按照sass编译程序：
```shell
npm install -g sass  // 全局安装
npm install -D sass  // 项目安装
```