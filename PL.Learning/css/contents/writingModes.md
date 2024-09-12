### [CSS 书写模式](#)
**介绍** 设置文本的书写方式。

### [1. 属性大纲](#)

| 属性                                                                            | 版本	     |继承性| 简介                                    |
|:------------------------------------------------------------------------------|:--------|:----|:--------------------------------------|
| [direction](https://developer.mozilla.org/zh-CN/docs/Web/CSS/direction)	      |CSS2	|有|	检索或设置文本流的方向|
| [unicode-bidi](https://developer.mozilla.org/zh-CN/docs/Web/CSS/unicode-bidi) |	CSS2|	无|	用于同一个页面里存在从不同方向读进的文本显示。与direction属性一起使用|
| [writing-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/writing-mode) |	CSS3|	有|	设置或检索对象的内容块固有的书写方向|
| [text-orientation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-orientation)                                                          |	CSS3|	有|	属性设定行中字符的方向。|


#### [1.1 direction](#)
请注意，文本方向通常在文档中定义（例如，使用 HTML 的 dir 属性 属性），而不是通过直接使用 direction 属性来定义。

```
/* 关键字值 */
direction: ltr; 
direction: rtl;
```
**值**：
* **ltr** 默认属性。可设置文本和其他元素的默认方向是从左到右。
* **rtl** 可设置文本和其他元素的默认方向是从右到左。

仅仅设置 direction 属性毫无意义。

#### [1.2 unicode-bidi](#)
CSS unicode-bidi 属性，和 direction 属性，决定如何处理文档中的双书写方向文本（bidirectional text）。

#### [1.3 writing-mode](#)
writing-mode 属性定义了文本水平或垂直排布以及在块级元素中文本的行进方向。

此属性指定块流动方向，即块级容器堆叠的方向，以及行内内容在块级容器中的流动方向。因此，它也确定块级内容的顺序。

**值**：
* **horizontal-tb** 对于左对齐（ltr）文本，内容从左到右水平流动。对于右对齐（rtl）文本，内容从右到左水平流动。下一水平行位于上一行下方。
* **vertical-rl** 对于左对齐（ltr）文本，内容从上到下垂直流动，下一垂直行位于上一行左侧。对于右对齐（rtl）文本，内容从下到上垂直流动，下一垂直行位于上一行右侧。
* **vertical-lr** 对于左对齐（ltr）文本，内容从上到下垂直流动，下一垂直行位于上一行右侧。对于右对齐（rtl）文本，内容从下到上垂直流动，下一垂直行位于上一行左侧。

#### [1.4 text-orientation](#)
text-orientation CSS 属性设定行中字符的方向。但它仅影响纵向模式（当 writing-mode 的值不是horizontal-tb）下的文本。此属性在控制使用竖排文字的语言的显示上很有作用，也可以用来构建垂直的表格头。

