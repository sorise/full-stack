## [伪类选择符、伪对象选择符](#)
>**介绍**：伪类用于选择元素的特定状态或位置，以冒号 `:` 开头；伪元素用于对某些“特殊位置”添加样式，通常以双冒号 `::` 开头（也可以单冒号兼容旧浏览器）。

### [1. 链接伪类选择器](#)
链接伪类专门用于设置链接在不同状态下的样式。

|选择器|版本|Description 简介|
|:-----------------------|:--------------|:------------|
|E:link|CSS1|`未访问链接` 设置超链接a在未被访问前的样式。|
|E:visited|CSS1|`已访问链接` 设置超链接a在其链接地址已被访问过时的样式。|
|E:hover|CSS2.1|`鼠标悬停时` 设置元素在其鼠标悬停时的样式。|
|E:active|CSS2.1|`active`设置元素在被用户激活（在鼠标点击与释放之间发生的事件）时的样式。|
|E:focus|CSS2.1|`获取焦点时`设置元素在成为输入焦点（该元素的onfocus事件发生）时的样式。|

```css
/* 未访问链接 */
a:link {
  color: brown;
  text-decoration: none;
}

/* 鼠标悬停链接 */
a:hover {
  color: aquamarine;
  text-decoration: underline;
}

/* 被访问的链接 */
a:visited {
  color: pink;
  text-decoration: underline;
}
```

### [2. 结构性伪类选择器](#)

|选择器|版本|Description简介|
|:-----------------------|:--------------|:------------|
|E:not(selector)|CSS3|匹配不含有selector选择符的元素E。|
|:root|CSS3|匹配E元素在文档的根元素,在HTML中，根元素永远是HTML。|
|E:empty|CSS3|匹配没有任何子元素（包括text节点）的元素E。|
|E:first-child|CSS2|匹配父元素的第一个子元素E。|
|E:last-child|CSS3|匹配父元素的最后一个子元素E。|
|E:only-child|CSS3|匹配父元素仅有的一个子元素E。|
|E:nth-child(n)|CSS3|匹配父元素的第n个子元素E。|
|E:nth-last-child(n)|CSS3|匹配父元素的倒数第n个子元素E。|
|E:first-of-type|CSS3|匹配同类型中的第一个同级兄弟元素E。|
|E:last-of-type|CSS3|匹配同类型中的最后一个同级兄弟元素E。|
|E:only-of-type|CSS3|匹配同类型中的唯一的一个同级兄弟元素E。|
|E:nth-of-type(n)|CSS3|匹配同类型中的第n个同级兄弟元素E。|
|E:nth-last-of-type(n)|CSS3|匹配同类型中的倒数第n个同级兄弟元素E。|
|E:has(`<selector>`)|2023.10月以后可用，支持性还不好|根据一个元素的后代或任何后续元素来确定其样式|

**n**的取值关键字**odd**和**even**
- odd 选择奇数位置的子元素。
- even 选择偶数位置的子元素。

```css
/* 选择文档的根元素（HTML 中的 <html>） */
:root {
  background: yellow;
}

li:nth-child(odd) {
  background-color: #f9f9f9;
}

dd:first-of-type {
  border: 2px solid orange;
}

li:first-child {
  border: 2px solid orange;
}

section:has(p) {
  color: red
}

h2:has(+ p) {
  color: yellow;
}

/* 类名不是 `.fancy` 的 <p> 元素*/
p:not(.fancy) {
  color: green;
}

/* 非 <p> 元素 */
body :not(p) {
  text-decoration: underline;
}

/* 既不是 <div> 也不是 <span> 的元素 */
body :not(div):not(span) {
  font-weight: bold;
}

/* 不是 <div> 或 `.fancy` 的元素*/
body :not(div, .fancy) {
  text-decoration: overline underline;
}

/* <h2> 元素中不是有 `.foo` 类名的 <span> 元素 */
h2 :not(span.foo) {
  color: red;
}
```

#### [2.1 反选not选择器](#)

```html
<!DOCTYPEhtml>
<htmllang="zh-cn">
<head>
<metacharset="utf-8"/>
<title>否定伪类选择符E:not()_CSS参考手册_web前端开发参考手册系列</title>
<meta name="author" content="JoyDu(飘零雾雨),dooyoe@gmail.com,www.doyoe.com"/>
<style>
    p:not(.abc){
        color:#f00;
    }
</style>
</head>
<body>
    <p class="abc">否定伪类选择符E:not()</p>
    <p id="abc">否定伪类选择符E:not()</p>
    <p class="abcd">否定伪类选择符E:not()</p>
    <p>否定伪类选择符E:not()</p>
</body>
</html>
```

### 3. UI 伪类选择器

|选择器|版本|Description简介|
|:-----------------------|:--------------|:------------|
|E:lang()|CSS2|匹配使用特殊语言的E元素。|
|E:checked|CSS3|匹配用户界面上处于选中状态的元素E。(用于inputtype为radio与checkbox时)|
|E:enabled|CSS3|匹配用户界面上处于可用状态的元素E。|
|E:disabled|CSS3|匹配用户界面上处于禁用状态的元素E。|
|E:target|CSS3|匹配相关URL指向的E元素。|
|`@page:first`|CSS2|设置页面容器第一页使用的样式。仅用于@page规则|
|`@page:left`|CSS2|设置页面容器位于装订线左边的所有页面使用的样式。仅用于@page规则|
|`@page:right`|CSS2|设置页面容器位于装订线右边的所有页面使用的样式。仅用于@page规则|

:lang() CSS 伪类基于元素语言来匹配页面元素。

```css
*:lang(en-US) {
  outline: 2px solid deeppink;
}
```

```html
<div lang="en">
  <q>This English quote has a <q>nested</q> quote inside.</q>
</div>
<div lang="fr">
  <q>This French quote has a <q>nested</q> quote inside.</q>
</div>
<div lang="de">
  <q>This German quote has a <q>nested</q> quote inside.</q>
</div>
```

### [4. 伪对象选择器](#)

伪元素用于对某些“特殊位置”添加样式，通常以双冒号 :: 开头（也可以单冒号兼容旧浏览器）。

|选择器|版本|Description 简介|
|:-------------|:---------------|:---------------------|
|E:first-letter/E::first-letter|CSS1/3|设置对象内的第一个字符的样式。                          |
|E:first-line/E::first-line|CSS1/3|设置对象内的第一行的样式。                            |
|E:before/E::before|CSS2/3|设置在对象前（依据对象树的逻辑结构）发生的内容。用来和content属性一起使用 |
|E:after/E::after|CSS2/3|设置在对象后（依据对象树的逻辑结构）发生的内容。用来和content属性一起使用 |
|E::selection|CSS3|设置对象被选择时的颜色。                             |
|E::placeholder|CSS3| 表示 `<input>` 或 `<textarea>` 元素中的占位文本。。      |