## [CSS 盒模型](#)
> **介绍** : CSS盒模型（Box Model）是网页布局的基础概念，它描述了每个元素在页面上的占用空间。理解盒模型对于控制元素的尺寸、边距和排版至关重要。


### [1. 属性大纲](#)
关于盒模型有如下的css属性。


| 属性          | CSS Version/ 值 | 继承性	 | 简介                       |
|:------------|:---------------|:-----|:-------------------------|
| [display](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) | CSS2/3	        | 无    |设置或检索对象是否及如何显示|
| float       | CSS1           | 无    |该属性的值指出了对象是否及如何浮动。请参阅clear属性|
| clear       | CSS1           | 无    |该属性的值指出了不允许有浮动对象的边。请参阅float属性|
| visibility  | CSS2           | 有    |设置或检索是否显示对象。与display属性不同，此属性为隐藏的对象保留其占据的物理空间|
| overflow    | CSS2/3         | 无    |复合属性。检索或设置对象处理溢出内容的方式。|
| overflow-x  | CSS2/3         | 无    |检索或设置对象处理横向溢出内容的方式。|
| overflow-y  | CSS2/3         | 无    |检索或设置对象处理纵向溢出内容的方式。|
| box-sizing  | CSS3           |  无  |   于定义文档如何计算一个元素的总宽度和总高度。  |


#### [1.1 display](#)
CSS display 属性设置元素是否被视为块级或行级盒子以及用于子元素的布局，例如流式布局、网格布局或弹性布局。


* **block** 该元素生成一个块级盒子，在正常的流中，该元素之前和之后产生换行。
* **inline** 该元素生成一个或多个行级盒子，它们之前或者之后并不会产生换行。在正常的流中，如果有空间，下一个元素将会在同一行上。
* **flow** 该元素使用流式布局（块向和行向布局）来排布它的内容。
* **flex** 该元素的行为类似块级元素并且根据弹性盒模型布局它的内容。
* **grid** 该元素的行为类似块级元素并且根据网格模型布局它的内容。

```
/* 预组合值 */
display: block;
display: inline;
display: inline-block;
display: flex;
display: inline-flex;
display: grid;
display: inline-grid;
display: flow-root;

/* 生成盒子 */
display: none;
display: contents;

/* 多关键字语法 */
display: block flex;
display: block flow;
display: block flow-root;
display: block grid;
display: inline flex;
display: inline flow;
display: inline flow-root;
display: inline grid;

/* 其他值 */
display: table;
display: table-row; /* 所有的 table 元素 都有等效的 CSS display 值 */
display: list-item;

/* 全局值 */
display: inherit;
display: initial;
display: revert;
display: revert-layer;
display: unset;
```