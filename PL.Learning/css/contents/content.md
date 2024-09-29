### [CSS 内容](#)
> **介绍** 设置内容的样式。


### [1. 属性大纲](#)

| 属性                                                                           | 版本	 | 继承性 | 简介 |
|:-----------------------------------------------------------------------------|:----|:----|:---|
| [content](https://developer.mozilla.org/zh-CN/docs/Web/CSS/content)	         |CSS2	|无|	用来和:after及:before伪元素一起使用，在对象前或后显示内容|
| [counter-increment]()                                                        |	CSS2|	无|	设定当一个selector发生时计数器增加的值|
| [counter-reset]()	                                                           |CSS2|	无|	将指定selector的计数器复位|
| [counter-set](https://developer.mozilla.org/zh-CN/docs/Web/CSS/counter-set)	 |CSS2|	无|	将 CSS 计数器设置为给定值。|
| [quotes](https://developer.mozilla.org/zh-CN/docs/Web/CSS/quotes)|CSS2	|有	|设置或检索对象内使用的嵌套标记|

counter-increment
counter-reset


#### [1.1 content](#)
CSS 的 content CSS 属性用于在元素的 ::before 和 ::after 伪元素中插入内容。使用 content 属性插入的内容都是匿名的可替换元素。

```
/* 不能与其他值组合的关键字 */
content: normal;
content: none;

/* <content-replacement>：<image> 值 */
content: url("http://www.example.com/test.png");
content: linear-gradient(#e66465, #9198e5);
content: image-set("image1x.png" 1x, "image2x.png" 2x);

/* 语音输出：“/”后为替代文本  */
content: url("../img/test.png") / "这是替代文本";

/* <string> 值 */
content: "unparsed text";

/* <counter> 值，后跟可选的 <list-style-type> */
content: counter(chapter_counter);
content: counter(chapter_counter, upper-roman);
content: counters(section_counter, ".");
content: counters(section_counter, ".", decimal-leading-zero);

/* attr() 值会链接到 HTML 属性值 */
content: attr(href);

/* <quote> 值 */
content: open-quote;
content: close-quote;
content: no-open-quote;
content: no-close-quote;

/* <content-list>：content 值的列表。
可以同时使用多个值 */
content: "prefix" url(http://www.example.com/test.png);
content: "prefix" url("/img/test.png") "suffix" / "Alt text";
content: open-quote counter(chapter_counter);

/* 全局值 */
content: inherit;
content: initial;
content: revert;
content: revert-layer;
content: unset;
```


### [2. counter*](#)
counter-increment、counter-reset 和 counter-set 是 CSS 中用于处理计数器的三个属性，主要用于生成自动编号的内容。它们通常与伪元素（如 ::before 或 ::after）以及 content 属性配合使用。下面是它们的具体用法：

#### [2.1 counter-reset 属性](#)
counter-reset 用于创建或重置计数器的初始值。

语法：
```css
element {
    counter-reset: counter-name [initial-value];
}
```
* counter-name: 计数器的名称（自定义）。
* initial-value: 可选，指定计数器的初始值，默认为 0。

* 示例：
```css
section {
    counter-reset: section-counter; /* 创建名为 section-counter 的计数器，初始值为 0 */
}

h2::before {
    counter-increment: section-counter; /* 计数器每次使用时递增 */
    content: "Section " counter(section-counter) ". "; /* 使用计数器的值 */
}
```
此示例会给每个 h2 元素前添加 "Section 1. ", "Section 2." 等文本。

#### [2.2 counter-increment 属性](#)
counter-increment 用于指定计数器在元素上使用时增加的步数。

语法：
```css
element {
    counter-increment: counter-name [increment-value];
}
```
* counter-name: 计数器的名称。
* increment-value: 可选，计数器每次递增的值，默认为 1。
示例：
```css
li::before {
    counter-increment: list-counter; /* 每个 li 元素都会使计数器递增 1 */
    content: counter(list-counter) ". "; /* 显示计数器值 */
}
```
在这个例子中，每个 li 元素前都会显示递增的编号。

#### [2.3 counter-set 属性](#)
counter-set 用于将计数器的值直接设置为一个特定的值，而不是递增。

语法：
```css
element {
    counter-set: counter-name value;
}
```
* counter-name: 计数器的名称。
* value: 计数器的具体值。
示例：
```css
h2 {
    counter-reset: section-counter; /* 初始化计数器 */
}

h2.special::before {
    counter-set: section-counter 10; /* 将计数器值直接设置为 10 */
    content: "Special Section " counter(section-counter) ". ";
}
```
在这个例子中，带有 special 类的 h2 元素的计数器值将被设置为 10，而不是递增的值。

总结：
* counter-reset: 初始化或重置计数器。
* counter-increment: 每次使用时增加计数器的值。
* counter-set: 将计数器设置为特定值。
这些属性结合伪元素与 content 属性可以实现自动编号、列表或章节的编号系统。