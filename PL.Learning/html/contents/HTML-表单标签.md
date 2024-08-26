## [HTML 表单标签](#)
> **介绍**：HTML 5表单基础,表单是一个网页最基础的内容。

-----


### [1. form](#)
HTML **form** 元素表示文档中的一个区域，此区域包含交互控件，用于向 Web 服务器提交信息。

```html
<form action="" method="get" class="form-example">
  <div class="form-example">
    <label for="name">Enter your name: </label>
    <input type="text" name="name" id="name" required />
  </div>
  <div class="form-example">
    <label for="email">Enter your email: </label>
    <input type="email" name="email" id="email" required />
  </div>
  <div class="form-example">
    <input type="submit" value="Subscribe!" />
  </div>
</form>
```

#### [1.1 特有属性](#) 
* **action**
处理表单提交的 URL。这个值可被 `<button>`、`<input type="submit">` 或 `<input type="image">` 元素上的 formaction 属性覆盖。
* **enctype** 当 method 属性值为 post 时，enctype 就是将表单的内容提交给服务器的 MIME 类型 。可能的取值有：
  * application/x-www-form-urlencoded：未指定属性时的默认值。
  * multipart/form-data：当表单包含 type=file 的 `<input>` 元素时使用此值。
  * text/plain：出现于 HTML5，用于调试。这个值可被 `<button>`、`<input type="submit">` 或 `<input type="image">` 元素上的 formenctype 属性覆盖。
* **method** 浏览器使用这种 HTTP 方式来提交 表单。可能的值有：
  * post：指的是 HTTP POST 方法；表单数据会包含在表单体内然后发送给服务器。
  * get：指的是 HTTP GET 方法；表单数据会附加在 action 属性的 URL 中，并以 '?' 作为分隔符，没有副作用 时使用这个方法。
  * dialog：如果表单在 `<dialog>` 元素中，提交时关闭对话框。此值可以被 `<button>`、`<input type="submit">` 或 `<input type="image">` 元素中的 `formmethod` 属性覆盖。


### [5. button](#)
HTML `<button>` 元素表示一个可点击的按钮，可以用在表单或文档其他需要使用简单标准按钮的地方。默认情况下，HTML 按钮的显示样式接近于 user agent 所在的宿主系统平台（用户操作系统）的按钮，但你可以使用 CSS 来改变按钮的样貌。

```html
<button name="button">Click me</button>
```
* **autofocus** 一个布尔属性，用于指定当页面加载时按钮必须有输入焦点，除非用户重写，例如通过不同控件键入。只有一个表单关联元素可以指定该属性。
* **type** button 的类型。可选值：
    * submit: 此按钮将表单数据提交给服务器。如果未指定属性，或者属性动态更改为空值或无效值，则此值为默认值。
    * reset: 此按钮重置所有组件为初始值。
    * button: 此按钮没有默认行为。它可以有与元素事件相关的客户端脚本，当事件出现时可触发。
* **value** button 的初始值。它定义的值与表单数据的提交按钮相关联。当表单中的数据被提交时，这个值便以参数的形式被递送至服务器。
* **name** button 的名称，与表单数据一起提交。
* **disabled** 此布尔属性表示用户不能与 button 交互。
* **form** 表示 button 元素关联的 form 元素（它的表单拥有者）。此属性值必须为同一文档中的一个 `<form>` 元素的id属性。
* **formaction** 表示程序处理 button 提交信息的 URI。如果指定了，将重写 button 表单拥有者的action属性。
* **formenctype** 如果 button 是 submit 类型，此属性值指定提交表单到服务器的内容类型。可选值：
    * `application/x-www-form-urlencoded`: 未指定时的默认值。
    * `multipart/form-data`: 如果使用type属性的 `<input>` 元素设置文件，使用此值。
    * `text/plain` 如果指定此属性，它将重写 button 的表单拥有者的enctype属性。
* **formmethod** 如果 button 是 submit 类型，此属性指定浏览器提交表单使用的 HTTP 方法。可选值：
    * post：来自表单的数据被包含在表单内容中，被发送到服务器。
    * get：来自表单的数据以'?'作为分隔符被附加到 form 的URI属性中，得到的 URI 被发送到服务器。当表单没有副作用，且仅包含 ASCII 字符时使用这种方法。如果指定了，此属性会重写 button 拥有者的method属性。
* **formnovalidate** 如果 button 是 submit 类型，此布尔属性指定当表单被提交时不需要验证。如果指定了，它会重写 button 拥有者的novalidate属性。
* **formtarget** 如果 button 是 submit 类型，此属性指定一个名称或关键字，表示接收提交的表单后在哪里显示响应。这是一个浏览上下文（例如 tab，window 或内联框架）的名称或关键字。如果指定了，它会重写 button 拥有者的target 属性。关键字如下：
    * _self: 在同一个浏览上下文中加载响应作为当前的。未指定时此值为默认值。
    * _blank: 在一个新的不知名浏览上下文中加载响应。
    * _parent: 在当前浏览上下文父级中加载响应。如果没有父级的，此选项将按_self 执行。
    * _top: 在顶级浏览上下文（即当前浏览上下文的祖先，且没有父级）中架加载响应。如果没有顶级的，此选项将按_self 执行。

