## [Sass 预处理器脚本语言](#)

> **介绍** Sass (英文全称：Syntactically Awesome Stylesheets) 是一个最初由 Hampton Catlin 设计并由 Natalie Weizenbaum 开发的层叠样式表语言。


### [1. 介绍](#)
Sass（英文全称：`Syntactically Awesome Stylesheets`）是一个最初由 Hampton Catlin 设计并由 Natalie Weizenbaum 开发的层叠样式表语言。是一种动态样式语言，Sass 语法属于缩排语法，比 CSS 多出变量、嵌套、运算、混入(Mixin)、继承、颜色处理、函数等功能，更容易阅读。

- Sass 是一个 CSS 预处理器。
- Sass 是 CSS 扩展语言，可以帮助我们减少 CSS 重复的代码，节省开发时间。
- Sass 完全兼容所有版本的 CSS。

Sass 的`缩排语法`，对于写惯 CSS 前端的 Web 开发者来说很不直观，也不能将 CSS 代码加入到 Sass 里面，因此 Sass 语法进行了改良，Sass3 就变成了 Scss(Sassy CSS) 。与原来的语法兼容，只是用 {} 取代了原来的缩进。所以 Sass 包括两套语法，通常情况下，这两套语法通过 `.sass` 和 `.scss` 两个文件扩展名区分开。

>  `缩排语法`（Indentation Syntax）是一种用缩进和换行来表示代码层级和结构的写法，不依赖 `{}` 花括号或 `;` 分号来分隔代码块和语句。在 CSS 预处理器（如 Sass）中，缩排语法主要指 Sass 的 `.sass 格式` 例如：

```scss
nav
  background: #333
  color: white
  ul
    margin: 0
    padding: 0
    list-style: none
```
按照sass编译程序：
```sh
npm install -g sass  // 全局安装
npm install -D sass  // 项目安装
```

#### 1.1 编译文件
一旦你开始修改Sass，它将采用你的预处理Sass文件并将其保存为您可以在您的网站中使用的普通CSS文件。

实现这一目标的最直接方法是在您的终端。安装Sass后，您可以使用该sass命令将Sass编译为CSS 。您需要告诉Sass要构建哪个文件，以及将CSS输出到何处。

例如，sass input.scss output.css从终端运行将获取单个Sass文件input.scss，并将该文件编译为output.css。

您还可以使用该 `--watch` 标志查看单个文件或目录。watch标志告诉Sass要查看源文件的更改，并在每次保存Sass时重新编译CSS。如果您想观看（而不是手动构建）您的input.scss文件，您只需将watch标志添加到您的命令中，如下所示： `sass --watch input.scss output.css`

您可以使用文件夹路径作为输入和输出来观察和输出到目录，并使用冒号分隔它们。在这个例子中： `sass --watch app/sass:public/stylesheets` Sass会查看文件app/sass夹中的所有文件以进行更改，并将CSS编译到该 `public/stylesheets` 文件夹。

```sh
 //单文件转换命令
 sass input.scss output.css

 //单文件监听命令
 sass --watch input.scss:output.css

 //如果你有很多的sass文件的目录，你也可以告诉sass监听整个目录：
 sass --watch app/sass:public/stylesheets
 
 //编译格式
 sass --watch input.scss:output.css --style compact

 //编译添加调试map
 sass --watch input.scss:output.css --sourcemap

 //选择编译格式并添加调试map
 sass --watch input.scss:output.css --style expanded --sourcemap

 //开启debug信息
 sass --watch input.scss:output.css --debug-info
```

**`编译格式`**
```C#
  //未编译样式
 .box {
   width: 300px;
   height: 400px;
   &-title {
     height: 30px;
     line-height: 30px;
   }
 }
```
**`nested 编译排版格式`**
```css
 /*命令行内容*/
 sass style.scss:style.css --style nested

 /*编译过后样式*/
 .box {
   width: 300px;
   height: 400px; }
   .box-title {
     height: 30px;
     line-height: 30px; }
```
**`compressed 编译排版格式`**
```css
 /*命令行内容*/
 sass style.scss:style.css --style compressed

 /*编译过后样式*/
 .box{width:300px;height:400px}.box-title{height:30px;line-height:30px}
```