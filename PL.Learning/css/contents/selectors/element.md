## [基础选择符 Selectors、选择器交集和并集](#)
> **介绍** : CSS 选择器是 CSS 规则中定位并选取特定元素以应用样式的模式。

|选择器|名称|版本|Description 简介|
|:----|:--------------------------------------|:---|:----|
|`*`|通配选择符(Universal Selector)|CSS2|所有元素对象。|
|E|类型选择符(Type Selector)|CSS1|以文档语言对象类型作为选择符。|
|#myid|id选择符(ID Selector)|CSS1|以唯一标识符id属性等于myid的E对象作为选择符。|
|.myclass|class选择符(Class Selector)|CSS1|以class属性包含myclass的E对象作为选择符。|
|交集选择器|要求同时满足N个选择条件的元素才能被选中|CSS1||
|并集选择器|并集选择器适用于针对多种选择条件同时进行选中|CSS1||
|`\|`|命名空间分隔符|2015以后可以使用|命名空间不怎么使用|

```css
*{
    padding: 0px;
    margin: 0px;
}

div{
    color: aquamarine;
}

h1#content{
    font-size:13px;
}
p.content{
    font-size:13px;
}
```

```html
<div> block </div>
<h1 id="title">标题</h1>
<p class="content">正文内容</p>
```

### 通配选择符(Universal Selector)
在 CSS 中，一个星号 (*) 就是一个通配选择器。它可以匹配任意类型的 HTML 元素。

由于css有一些默认样式，这些默认样式我们需要去除。有很多人想到可以用通配符*来解决问题，但是这样实际开发中效率非常低，因为有些标签的边距我们是不希望去掉的比如h标签、p标签等，行业内就形成了重置样式表，这一个基本规范。

```css
/*

Time:2023.12.15
reset.css

*/

html, body, div, span, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
abbr, address, cite, code,
del, dfn, em, img, ins, kbd, q, samp,
small, strong, sub, sup, var,
b, i,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, figcaption, figure,
footer, header, hgroup, menu, nav, section, summary,
time, mark, audio, video {
    margin:0;
    padding:0;
    border:0;
    outline:0;
    font-size:100%;
    vertical-align:baseline;
    background:transparent;
    box-sizing:border-box;
}

body {
    line-height:1;
}

:focus {
    outline: 1;
}

article,aside,canvas,details,figcaption,figure,

footer,header,hgroup,menu,nav,section,summary {
    display:block;
}

ul {
    list-style:none;
}

blockquote, q {
    quotes:none;
}

blockquote:before, blockquote:after,

q:before, q:after {
    content:'';
    content:none;
}

a {
    margin:0;
    padding:0;
    border:0;
    font-size:100%;
    vertical-align:baseline;
    background:transparent;
}

ins {
    background-color:#ff9;
    color:#000;
    text-decoration:none;
}

mark {
    background-color:#ff9;
    color:#000;
    font-style:italic;
    font-weight:bold;
}

del {
    text-decoration: line-through;
}

abbr[title], dfn[title] {
    border-bottom:1px dotted #000;
    cursor:help;
}

table {
    border-collapse:collapse;
    border-spacing:0;
}

hr {
    display:block;
    height:1px;
    border:0; 
    border-top:1px solid #cccccc;
    margin:1em 0;
    padding:0;
}

input, select {
    vertical-align:middle;
}
```

### 类型选择符(Type Selector)
也叫标签选择器，通过节点名称匹配元素。换句话说，它选择一个文件中所有给定类型的元素。

```css
/* 所有 <a> 元素。*/
a {
  color: red;
}
span {
  background-color: skyblue;
}
```

### ID 选择符(ID Selector)
ID 选择器会根据该元素的 id 属性中的内容匹配元素。为了使该元素被选中，它的 id 属性必须与选择器中给出的值完全匹配。 `不怎么常用！`

```css
/* id 为“demo”的元素会被选中 */
#demo {
  border: red 2px solid;
}
```

### class 选择符(Class Selector)
根据 class 属性的内容匹配元素。

```css
/* 所有含有 class="spacious" 类的元素 */
.spacious {
  margin: 2em;
}

/* 所有含有 class="spacious" 类的 <li> 元素 */
li .spacious {
  margin: 2em;
}

/* 所有同时含有“spacious”和“elegant”类的 <li> 元素 */
/* 例如 class="elegant retro spacious" */
li .spacious .elegant {
  margin: 2em;
}
```

### 交集选择器
适用于要求同时满足N个选择条件的元素才能被选中，我们可以将前面介绍的四种选择器进行直接连接，**多个选择器中间不要用空格**。

```css
p.intersection{
    font-size: 15px; !important;
    color: blueviolet;
    font-weight: 600;
    font-stretch:90%; /* 字体更加紧凑 */
}
```

```html
<p class="intersection">一段文字</p>
<div class="intersection">一段文字</div>
```

此时，只有同时为p标签并且class中存在test类的元素才能被选中，再比如下面这种情况：

```css
p.test#p {
  color: blueviolet;
}
```

### 并集选择器
并集选择器（选择器列表），并集选择器适用于针对多种选择条件同时进行选中，一次性统一编写样式而不需要分开进行编写，比如我们以前是像这样写的：

```css
p { color: blueviolet; }
a { color: blueviolet }
div { color: blueviolet }
```

这里要使用CSS 选择器列表（`,`） 选择所有匹配的节点。选择器列表是以逗号分隔的多个选择器所组成的列表。
```css
span, div {
  border: red 2px solid;
}

:is(span, div) {
  border: red 2px solid;
}
```

实际上这三个选择器配置的样式都是一样的，我们可以直接使用并集选择器来为这三个选择器一次性编写样式，只需使用,隔开每一种选择器即可：

```css
p, a, div {  /* 与上面的代码效果等价 */
  color: blueviolet;
}
```

这样编写同样可以实现上面分开编写的效果，并且可以统一进行样式修改，同样适用于前面介绍的四种选择器。

### 命名空间分隔符
命名空间分隔符（`|`）将选择器与命名空间分隔开，用于标识类型选择器是否包含命名空间。

```css
/* 在 myNameSpace 命名空间中的链接 */
myNameSpace|a {
  font-weight: bold;
}
/* 任何命名空间的段落（包含无命名空间） */
*|p {
  color: darkblue;
}
/* 无命名空间的二级标题 */
|h2 {
  margin-bottom: 0;
}
```

- `ns|h1` ——匹配命名空间 ns 中的 `<h1>` 元素
- `*|h1` ——匹配所有 `<h1>` 元素
- `|h1` ——匹配所有未声明或隐含命名空间外的 `<h1>` 元素