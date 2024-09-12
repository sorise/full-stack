## [元素选择符 Element Selectors](#)
> **介绍**：simple

| 选择器 | 名称	| 版本 |	Description 简介|
|:----|:--------------------------------------|:---|:----|
| `*` | 	通配选择符(Universal Selector)|	CSS2|	所有元素对象。|
|E|	类型选择符(Type Selector)|	CSS1	|以文档语言对象类型作为选择符。|
|E#myid|	id选择符(ID Selector)|	CSS1	|以唯一标识符id属性等于myid的E对象作为选择符。|
|E.myclass|	class选择符(Class Selector)|	CSS1	|以class属性包含myclass的E对象作为选择符。|

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