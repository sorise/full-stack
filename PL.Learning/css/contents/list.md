### [CSS 列表样式](#)
> **介绍** 设置文本的书写方式。


### [1. 属性大纲](#)

| 属性                                                                                          | 版本	 | 继承性 | 简介 |
|:--------------------------------------------------------------------------------------------|:----|:----|:---|
| list-style                                                                                  |	CSS1|	有|	复合属性。设置列表项目相关内容|
| [list-style-image](https://developer.mozilla.org/zh-CN/docs/Web/CSS/list-style-image)	      |CSS1	|有|	设置或检索作为对象的列表项标记的图像|
| [list-style-position](https://developer.mozilla.org/zh-CN/docs/Web/CSS/list-style-position) |	CSS1|	有|	设置或检索作为对象的列表项标记如何根据文本排列|
| [list-style-type](https://developer.mozilla.org/zh-CN/docs/Web/CSS/list-style-type)	                                                                        |CSS1/2|	有|	设置或检索对象的列表项所使用的预设标记|

#### [1.1 list-style](#)
list-style CSS 属性是一个简写对属性集合，包括list-style-type, list-style-image, 和 list-style-position。



#### [1.1 list-style-image](#)
list-style-image 属性用来指定一个能用来作为列表元素标记的图片。

```
/* Keyword values */
list-style-image: none;

/* <url> values */
list-style-image: url("starsolid.gif");

/* Global values */
list-style-image: inherit;
list-style-image: initial;
list-style-image: unset;
```

#### [1.2 list-style-position](#)
list-style-position 属性指定标记框在主体块框中的位置。

* outside 标记盒在主块盒的外面。
* inside 标记盒是主要块盒中的第一个行内盒，处于元素的内容流之后。