## [元素选择符 Element Selectors](#)
> **介绍**：simple

### [1. 选择器](#)
| 选择器                   | 版本                                                |	Description 简介|
|:-----------------------|:--------------------------------------|:--------------------------------------------------|
| E:link                 	| CSS1                                              |	设置超链接a在未被访问前的样式。|
| E:visited	             |CSS1| 	设置超链接a在其链接地址已被访问过时的样式。                           |
| E:hover	               |CSS2.1| 	设置元素在其鼠标悬停时的样式。                                  |
| E:active	              |CSS2.1| 	设置元素在被用户激活（在鼠标点击与释放之间发生的事件）时的样式。                 |
| E:focus	               |CSS2.1| 	设置元素在成为输入焦点（该元素的onfocus事件发生）时的样式。                |
| E:lang()	              |CSS2| 	匹配使用特殊语言的E元素。                                    |
| E:not(selector)	       |CSS3| 	匹配不含有selector选择符的元素E。                            |
| E:root	                |CSS3| 	匹配E元素在文档的根元素,在HTML中，根元素永远是HTML。                                   |
| E:first-child	         |CSS2| 	匹配父元素的第一个子元素E。                                   |
| E:last-child	          |CSS3| 	匹配父元素的最后一个子元素E。                                  |
| E:only-child	          |CSS3| 	匹配父元素仅有的一个子元素E。                                  |
| E:nth-child(n)	        |CSS3| 	匹配父元素的第n个子元素E。                                   |
| E:nth-last-child(n)	   |CSS3| 	匹配父元素的倒数第n个子元素E。                                 |
| E:first-of-type	       |CSS3| 	匹配同类型中的第一个同级兄弟元素E。                               |
| E:last-of-type	        |CSS3| 	匹配同类型中的最后一个同级兄弟元素E。                              |
| E:only-of-type	        |CSS3| 	匹配同类型中的唯一的一个同级兄弟元素E。                             |
| E:nth-of-type(n)	      |CSS3| 	匹配同类型中的第n个同级兄弟元素E。                               |
| E:nth-last-of-type(n)	 |CSS3| 	匹配同类型中的倒数第n个同级兄弟元素E。                             |
| E:empty	               |CSS3| 	匹配没有任何子元素（包括text节点）的元素E。                         |
| E:checked	             |CSS3| 	匹配用户界面上处于选中状态的元素E。(用于input type为radio与checkbox时) |
| E:enabled	             |CSS3| 	匹配用户界面上处于可用状态的元素E。                               |
| E:disabled	            |CSS3| 	匹配用户界面上处于禁用状态的元素E。                               |
| E:target	              |CSS3| 	匹配相关URL指向的E元素。                                   |
| `@page:first`	           |CSS2| 	设置页面容器第一页使用的样式。仅用于@page规则                        |
| `@page:left`	            |CSS2| 	设置页面容器位于装订线左边的所有页面使用的样式。仅用于@page规则               |
| `@page:right`	           |CSS2| 	设置页面容器位于装订线右边的所有页面使用的样式。仅用于@page规则               |


#### [1.1 反选not 选择器](#)

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
<meta charset="utf-8" />
<title>否定伪类选择符 E:not()_CSS参考手册_web前端开发参考手册系列</title>
<meta name="author" content="Joy Du(飘零雾雨), dooyoe@gmail.com, www.doyoe.com" />
<style>
    p:not(.abc){color:#f00;}
</style>
</head>
<body>
    <p class="abc">否定伪类选择符 E:not()</p>
    <p id="abc">否定伪类选择符 E:not()</p>
    <p class="abcd">否定伪类选择符 E:not()</p>
    <p>否定伪类选择符 E:not()</p>
</body>
</html>
			
```