### [CSS 字体](#)
**介绍** 设置字体的大小、样式、粗细、大小写、字体、拉伸。

### [1. 属性大纲](#)


| 属性                                                                                 | 版本	     |继承性| 简介                                                         |
|:-----------------------------------------------------------------------------------|:--------|:----|:-----------------------------------------------------------|
| text-transform	                                                                    |CSS1/3|	有	| 检索或设置对象中的文本的大小写                                            |
| white-space	                                                                       |CSS1|	有	| 设置或检索对象内空格的处理方式                                            |
| [tab-size](https://developer.mozilla.org/zh-CN/docs/Web/CSS/tab-size)	             |CSS3|	有	| 检索或设置对象中的制表符的长度                                            |
| word-wrap	                                                                         |CSS3|	有	| 设置或检索当内容超过指定容器的边界时是否断行,CSS3中将 word-wrap 改名为 overflow-wrap                                    |
| overflow-wrap	                                                                     |CSS3|	有| 	设置或检索当内容超过指定容器的边界时是否断行                                    |
| word-break	                                                                        |CSS3|	有	| 设置或检索对象内文本的字内换行行为                                          |
| text-align	                                                                        |CSS1/3|	有	| 设置或检索对象中内容的水平对齐方式                                          |
| text-align-last	                                                                   |CSS3|	有| 	设置或检索一个块内的最后一行（包括块内仅有一行文本的情况，这时既是第一行也是最后一行）或者被强制打断的行的对齐方式 |
| text-justify	                                                                      |CSS3|	有	| 设置或检索对象内调整文本使用的对齐方式                                        |
| word-spacing	                                                                      |CSS1/3|	有| 	检索或设置对象中的单词之间的最小，最大和最佳间隙                                  |
| letter-spacing	                                                                    |CSS1/3|	有| 	检索或设置对象中的字符之间的最小，最大和最佳间隙                                  |
| text-indent	                                                                       |CSS1/3|	有| 	检索或设置对象中的文本的缩进                                            |
| [vertical-align](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)	 |CSS1/2|	无| 	设置或检索对象内容的垂直对其方式                                          |
| [line-height](https://developer.mozilla.org/zh-CN/docs/Web/CSS/line-height)	       |CSS1|	有| 	检索或设置对象的行高。即字体最底端与字体内部顶端之间的距离                             |

#### [1.1 text-transform](#)
CSS 属性指定如何将元素的文本大写。它可以用于使文本显示为全大写或全小写，也可单独对每一个单词进行操作, **兼容性不佳**。

* none： 无转换
* capitalize： 将每个单词的第一个字母转换成大写
* uppercase： 将每个单词转换成大写
* lowercase： 将每个单词转换成小写
* full-width： 将所有字符转换成fullwidth形式。如果字符没有相应的fullwidth形式，将保留原样。这个值通常用于排版拉丁字符和数字等表意符号。（CSS3）


```css
.capitalize span{text-transform:capitalize;}
.uppercase span{text-transform:uppercase;}
.lowercase span{text-transform:lowercase;}
```

#### [1.2 white-space](#)
white-space 属性用于设置如何处理元素内的空白字符。

这个属性指定了两件事：
* 空白字符是否合并，以及如何合并。
* 是否换行，以及如何换行。

* **normal**： 连续的空白符会被合并。源码中的换行符会被当作空白符来处理。并根据填充行框盒子的需要来换行。
* **pre**： 连续的空白符会被保留。仅在遇到换行符或 <br> 元素时才会换行。
* **nowrap**： 强制在同一行内显示所有文本，合并文本间的多余空白，直到文本结束或者遭遇br对象。
* **pre-wrap**： 用等宽字体显示预先格式化的文本，不合并文字间的空白距离，当文字碰到边界时发生换行。
* **pre-line**： 保持文本的换行，不保留文字间的空白距离，当文字碰到边界时发生换行。
* **break-spaces** 与 pre-wrap 的行为相同，除了：
  * 任何保留的空白序列总是占用空间，包括行末的。
  * 每个保留的空白字符后（包括空白字符之间）都可以被截断。
  * 这样保留的空间占用空间而不会挂起，从而影响盒子的固有尺寸（最小内容——min-content——大小和最大内容——max-content——大小）。

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
<meta charset="utf-8" />
<title>white-space_CSS参考手册_web前端开发参考手册系列</title>
<meta name="author" content="Joy Du(飘零雾雨), dooyoe@gmail.com, www.doyoe.com" />
<style>
.test p{width:200px;border:1px solid #000;}
.normal p{word-wrap:normal;}
.pre p{white-space:pre;}
.pre-wrap p{white-space:pre-wrap;}
.pre-line p{white-space:pre-line;}
.nowrap p{white-space:nowrap;}
</style>
</head>
<body>
<ul class="test">
	<li class="normal">
		<strong>normal：</strong>
		<p>轻轻地我走了
	正如我轻轻地来</p>
	</li>
	<li class="pre">
		<strong>pre：</strong>
		<p>轻轻地我走了（这里接很多测试文字）
	正如我轻轻地来</p>
	</li>
	<li class="pre-wrap">
		<strong>pre-wrap：</strong>
		<p>轻轻地    我走了（这里接很多测试文字）
	正如我轻轻地来</p>
	</li>
	<li class="pre-line">
		<strong>pre-line</strong>
		<p>轻轻地    我走了（这里接很多测试文字）
	正如我轻轻地来</p>
	</li>
	<li class="nowrap">
		<strong>nowrap：</strong>
		<p>轻轻地我走了
	正如我轻轻地来</p>
	</li>
</ul>
</body>
</html>
```