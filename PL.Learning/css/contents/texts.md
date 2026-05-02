## [CSS字体](#)
**介绍**设置字体的大小、样式、粗细、大小写、字体、拉伸。

### [1.属性大纲](#)

|属性|版本|继承性|简介|
|:----------------------|:-------|:----|:--------------|
|text-transform|CSS1/3|有|检索或设置对象中的文本的大小写|
|[word-break](https://developer.mozilla.org/zh-CN/docs/Web/CSS/word-break)|CSS3|有|设置或检索对象内文本的字内换行行为|
|[text-align](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-align)|CSS1/3|有|设置或检索对象中内容的水平对齐方式|
|[text-align-last](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-align-last)|CSS3|有|指定一行或者块中的最后一行在被强制换行之前的对齐规则|
| `不推荐使用` [text-justify](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-justify)|CSS3|有|设置或检索对象内调整文本使用的对齐方式 `尚未在主流浏览器中得到支持。`|
|[text-overflow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-overflow)|CSS3|有|属性用于确定如何提示用户存在隐藏的溢出内容。|
|[word-spacing](https://developer.mozilla.org/zh-CN/docs/Web/CSS/word-spacing)|CSS1/3|有|检索或设置对象中的**单词**之间的最小，最大和最佳间隙|
|[letter-spacing](https://developer.mozilla.org/zh-CN/docs/Web/CSS/letter-spacing)|CSS1/3|有|检索或设置对象中的**字符**之间的最小，最大和最佳间隙|
|[text-indent](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-indent)|CSS1/3|有|设置区块元素中文本行前面空格（缩进）的长度。|
|[vertical-align](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)|CSS2.1|无|用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式。|
|[line-height](https://developer.mozilla.org/zh-CN/docs/Web/CSS/line-height)|CSS1|有|检索或设置对象的行高。即字体最底端与字体内部顶端之间的距离|
|white-space|CSS1|有|设置或检索对象内空格的处理方式|
|word-wrap|CSS3|有|请使用overflow-wrap|
|[overflow-wrap](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow-wrap)|CSS3|有|设置或检索当内容超过指定容器(`超长单词`)的边界时是否断行|
|[tab-size](https://developer.mozilla.org/zh-CN/docs/Web/CSS/tab-size)|CSS3|有|检索或设置对象中的制表符的长度|

#### text-transform
CSS 属性指定如何将元素的文本大写。它可以用于使文本显示为全大写或全小写，也可单独对每一个单词进行操作, **兼容性不佳**。

* none： 无转换
* capitalize： 将每个单词的第一个字母转换成大写
* uppercase： 将每个单词转换成大写
* lowercase： 将每个单词转换成小写
* full-width： 将所有字符转换成fullwidth形式。如果字符没有相应的fullwidth形式，将保留原样。这个值通常用于排版拉丁字符和数字等表意符号。（CSS3）

```css
.capitalize span{ text-transform:capitalize;}
.uppercase span{ text-transform:uppercase;}
.lowercase span{ text-transform:lowercase;}
```

#### text-indent
设置区块元素中文本行前面空格（缩进）的长度。

值:
- `<length>` 允许使用负值
- `<percentage>` 缩进是包含区块宽度的 `<percentage>`.
- each-line 缩进会影响区块容器的第一行以及强制换行后的每一行，但不影响软换行后的行。
- hanging 反转缩进行。除第一行外，所有行都将缩进。

```css
/* <length> 值 */
text-indent: 3mm;
text-indent: 40px;

/* <percentage> 值，相对于包含区块的宽度 */
text-indent: 15%;

/* 关键字值 */
text-indent: 5em each-line;
text-indent: 5em hanging;
text-indent: 5em hanging each-line;

/* 全局值 */
text-indent: inherit;
text-indent: initial;
text-indent: revert;
text-indent: revert-layer;
text-indent: unset;
```

#### text-align-last
设置或检索一个块内的最后一行（包括块内仅有一行文本的情况，这时既是第一行也是最后一行）或者被强制打断的行的对齐方式。

**值**:
* auto 受影响的行会根据 text-align 的值来对齐，除非 text-align 的值是 justify，在这种情况下，其效果等同于将 text-align-last 的值设置为 start。
* start 如果内容方向是左至右，则等于 left，反之则为 right。
* end 如果内容方向是左至右，则等于 right，反之则为 left。
* left 行内内容对齐到行框的左边缘。
* right 行内内容对齐到行框的右边缘。
* center 行内内容在行框中居中。
* justify 最后一行文字的开头与内容盒子的左侧对齐，末尾与右侧对齐。

#### line-height
设置多行元素的空间量，如多行文本的间距。对于块级元素，它指定元素行盒（line boxes）的最小高度。对于非可替换的 inline 元素，它用于计算行盒（line box）的高度。

取值
- normal 取决于用户代理。桌面浏览器（包括 Firefox）使用默认值，约为 1.2，这取决于元素的 font-family。
- `<number>`（无单位）该属性的应用值是这个无单位<数字>乘以该元素的字体大小。计算值与指定的 `<number>` 值相同。大多数情况下，这是设置 line-height 的推荐方法，不会在继承时产生不确定的结果。
- `<length>` 指定用于计算行向盒高度的<长度>值。以 em 为单位的值可能会产生不确定的结果（见下面的示例）。
- `<percentage>` 与元素自身的字体大小有关。计算值是给定的百分比值乘以元素计算出的字体大小。`<percentage>` 值可能会带来不确定的结果（见下面第二个示例）。

```css
/* 理论上，以下所有规则拥有相同的行高 */

div {
  line-height: 1.2;
  font-size: 10pt;
} /* 无单位数值 number/unitless */
div {
  line-height: 1.2em;
  font-size: 10pt;
} /* 长度 length */
div {
  line-height: 120%;
  font-size: 10pt;
} /* 百分比 percentage */
div {
  font:
    10pt/1.2 Georgia,
    "Bitstream Charter",
    serif;
} /* font 简写属性 font shorthand */
```

### 2. 控制换行的属性
CSS 提供了多个属性来控制文本的换行和空白处理，其中最常用的是 text-wrap、white-space 和 word-break。

|属性|版本|继承性|简介|
|:----------------------|:-------|:----|:--------------|
|white-space|CSS1|有|设置或检索对象内空格的处理方式|
|[word-break](https://developer.mozilla.org/zh-CN/docs/Web/CSS/word-break)|CSS3|有|设置或检索对象内文本的字内换行行为|
|word-wrap|CSS3|有|请使用overflow-wrap|

#### 2.1 white-space
white-space 属性用于设置如何处理元素内的空白字符。

这个属性指定了两件事：
1. 空白字符是否合并，以及如何合并。
2. 是否换行，以及如何换行。

属性值：
* **normal**： 连续的空白符会被合并。源码中的换行符会被当作空白符来处理。并根据填充行框盒子的需要来换行。
* **pre**： 连续的空白符会被保留。仅在遇到换行符或 `<br>` 元素时才会换行。
* **pre-wrap**： 用等宽字体显示预先格式化的文本，不合并文字间的空白距离，当文字碰到边界时发生换行。
* **nowrap**： 强制在同一行内显示所有文本，合并文本间的多余空白，直到文本结束或者遭遇br对象。
* **pre-line**：保持文本的换行，不保留文字间的空白距离，当文字碰到边界时发生换行。
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

#### 2.2 word-break
word-break 指定了怎样在单词内断行。

- normal 使用默认的断行规则。
- break-all 可在任意字符间断行,允许西文字母中间换行。
- keep-all 换行时一个单词不可中断。

#### 2.3 overflow-wrap
**用于单词本身的长度就已经超出页面的宽度**，但为了保证单词的完整性，同样不会拆分换行。那么如果这个时候我们希望保持word-break: normal;的状态，同时针对于这种超长的单词又能合理拆分换行该怎么办呢？这时我们需要用一个和word-break配合的属性overflow-wrap（别名为word-wrap），它用于控制长度超过一行的单词是否被拆分换行：

> overflow-wrap 是 word-break 的一个补充属性。
```html
<p>workerworkerworkerworkerworkerworkerworker</p>
```

- normal 行只能在正常的单词断点（例如两个单词之间的空格）处换行。
- anywhere 为防止溢出，如果行中没有其他可接受的断点，则不可断的字符串（如长词或 URL）可能会在任何时候换行。在断点处不会插入连字符。在计算最小内容内在大小时，会考虑由单词换行引入的软换行机会。
- break-word 与 anywhere 值相同，如果行中没有其他可接受的断点，则允许在任意点将通常不可断的单词换行，但在计算最小内容内在大小时不考虑断字引入的软换行机会。

### 3. 实现溢出文本省略号 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="keywords" content="救助,社会救助,南充,南充市民政局,南充民政局,低保,果城救助">
    <meta name="description" content="社会救助宣传">
    <meta name="author" content="JX KICKER">
    <meta http-equiv="refresh" content="30">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>实现溢出文本省略号...</title>
    <style>
      .box{
        border: 1px solid gray;
      }
      .singe-line {
        text-overflow: ellipsis;
        overflow: hidden;
        word-break: break-all;
        white-space: nowrap;
      }
      .double-line {
        word-break: break-all;
        overflow: hidden;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
      }
    </style>
</head>
<body>
  <p>单行省略</p>
  <div class="singe-line">
    Learn English with our expert courses, resources and exam support. We use cookies and similar tracking technologies to run our website,
    personalise content, analyse site usage, and tailor advertisements. To manage your choices, visit our preference centre; for further
    information, see our cookie policy.
  </div>
  <p>两行省略</p>
  <div class="box">
    <div class="double-line" >
      Learn English with our expert courses, resources and exam support. We use cookies and similar tracking technologies to run our website,
      personalise content, analyse site usage, and tailor advertisements. To manage your choices, visit our preference centre; for further
      information, see our cookie policy.
    </div>
  </div>
</body>
</html>
```