### [CSS 字体](#)
**介绍** 设置字体的大小、样式、粗细、大小写、字体、拉伸。

### [1. 属性大纲](#)

| 属性| 版本 |Inherit From Parent 继承性|Description 简介|
|:---------|:--------|:----|:----|
|font| CSS1/2|有|复合属性。设置或检索对象中的文本特性|
|[font-family](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-family)|CSS1|有|设置或检索用于对象中文本的字体名称序列|
|[font-style](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-style)|CSS1|有|设置或检索对象中的字体样式|
|[font-variant](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-variant) |CSS1|有|设置或检索对象中的文本是否为小型的大写字母|
|[font-weight](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-weight)|CSS1|有|设置或检索对象中的文本字体的粗细|
|[font-size](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-weight)|CSS1|有|设置或检索对象中的字体尺寸|
|[font-stretch](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-stretch)|CSS3|有|设置或检索对象中的文字是否横向拉伸变形。|
|color|CSS1|有|设置字体颜色|
|[@font-face](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face)|CSS3|有|设置自定义字体|


#### [1.1 font](#)
CSS 简写属性 font 可设置某元素字体的不同属性，或将元素的字体设置为系统字体。

该属性是以下 CSS 属性的简写：

* font-family
* font-size
* font-stretch
* font-style
* font-variant
* font-weight
* line-height

#### [1.2 font-family](#)
font-family 允许您通过给定一个有先后顺序的，由字体名或者字体族名组成的列表来为选定的元素设置字体。

```css
/* 一个字体族名和一个通用字体族名 */
font-family: "Gill Sans Extrabold", sans-serif;
font-family: "Goudy Bookletter 1911", sans-serif;

/* 仅有一个通用字体族名 */
font-family: serif;
font-family: sans-serif;
font-family: monospace;
font-family: cursive;
font-family: fantasy;
font-family: system-ui;
font-family: ui-serif;
font-family: ui-sans-serif;
font-family: ui-monospace;
font-family: ui-rounded;
font-family: emoji;
font-family: math;
font-family: fangsong;

/* 全局值 */
font-family: inherit;
font-family: initial;
font-family: revert;
font-family: revert-layer;
font-family: unset;c
```

```css
p {
    font-family: "微软雅黑", 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    font-weight: bold;
    font-size: 17px;
}
```

#### [1.3 font-style](#)
CSS 属性允许你选择 font-family 字体下的 italic 或 oblique 样式。

```css
.sample {
  font: 2rem 'AmstelvarAlpha', sans-serif;
  font-style: oblique 23deg;
}
```

* normal 选择 font-family 的常规字体。
* italic 选择斜体，如果当前字体没有可用的斜体版本，会选用倾斜体（oblique ）替代。
* oblique 选择倾斜体，如果当前字体没有可用的倾斜体版本，会选用斜体（italic ）替代。

#### [1.4 font-variant](#)
font-variant 属性设置小型大写字母的字体显示文本，这意味着所有的小写字母均会被转换为大写，但是所有使用小型大写字体的字母与其余文本相比，其字体尺寸更小

```html
<style>
    .vari-normal{
        font-family: AlibabaPuHuiTi, sans-serif;
        font-variant: normal;
    }
    .vari-small{
        font-family: AlibabaPuHuiTi, sans-serif;
        font-variant: small-caps;
    }
</style>

<p class="vari-small">
    Learn English with our expert courses, resources and exam support.
</p>
<p class="vari-normal">
    Learn English with our expert courses, resources and exam support.
</p>
```

<p style="font-variant: small-caps">Learn English with our expert courses, resources and exam support.</p>

<p style="font-variant: normal">Learn English with our expert courses, resources and exam support.</p>

#### [1.5 font-weight](#)
属性指定了字体的粗细程度。一些字体只提供 normal 和 bold 两种值。

```css
/* Keyword values */
font-weight: normal;
font-weight: bold;

/* Keyword values relative to the parent */
font-weight: lighter;
font-weight: bolder;

/* Numeric keyword values */
font-weight: 1
font-weight: 100;
font-weight: 100.6;
font-weight: 123;
font-weight: 200;
font-weight: 300;
font-weight: 321;
font-weight: 400;
font-weight: 500;
font-weight: 600;
font-weight: 700;
font-weight: 800;
font-weight: 900;
font-weight: 1000;

/* Global values */
font-weight: inherit;
font-weight: initial;
font-weight: unset;
```

#### [1.6 font-stretch]
font-stretch CSS 属性可从字体中选择正常、压缩或扩展的字体外观。

```css
/* <font-stretch-css3> 关键字值 */
font-stretch: normal;
font-stretch: ultra-condensed;
font-stretch: extra-condensed;
font-stretch: condensed;
font-stretch: semi-condensed;
font-stretch: semi-expanded;
font-stretch: expanded;
font-stretch: extra-expanded;
font-stretch: ultra-expanded;

/* 百分比值 */
font-stretch: 50%;
font-stretch: 100%;
font-stretch: 200%;

/* 全局值 */
font-stretch: inherit;
font-stretch: initial;
font-stretch: revert;
font-stretch: revert-layer;
font-stretch: unset;
```


#### [1.7 font-size](#)
CSS 属性设置字体大小。更改字体大小还会更新字体大小相关的 `<length>` 单位，例如 em、ex 等。

在网页设计中，字体大小的设置非常重要，它直接影响到页面的可读性和用户体验。在CSS中有多种设置字体大小的方式，本文将重点介绍最常用的方法。

##### 绝对单位：px
在CSS中，最常用的设置字体大小的方法是使用绝对单位像素（px）。像素是一个绝对单位，它指定了元素的确切大小。通过指定一个像素值，我们可以精确地设置字体大小。

```css
p {
  font-size: 16px;
}
```

##### 相对单位：em 和 rem
除了像素单位外，CSS还提供了相对单位em和rem。相对单位是相对于父元素或根元素字体大小的倍数。

em是相对于父元素字体大小的单位。如果一个段落元素的字体大小设置为1.2em，它将以父元素字体大小的1.2倍显示。

例子：如果父元素的字体大小是16像素，则段落元素的字体大小将为19.2像素（16 * 1.2）。

```css
p {
  font-size: 1.2em;
}
```

使用相对单位em的好处是，它可以根据父元素的字体大小自动调整字体大小。这对于创建具有可伸缩性的网页非常有用，因为它可以根据用户设备和偏好自动适应字体大小。

**rem** 是相对于根元素字体大小的单位。根元素是HTML文档的最顶层元素，通常是`<html>`标签。设置根元素字体大小可以影响整个网页的字体大小。

```css
html {
  font-size: 16px;
}

p {
  font-size: 1.2rem;
}
```

我们将根元素字体大小设置为16像素，并将段落元素的字体大小设置为1.2rem。如果根元素的字体大小是16像素，段落元素的字体大小将为19.2像素（16 * 1.2）。

##### 使用百分比
除了像素和相对单位外，CSS还提供了百分比作为设置字体大小的选项。百分比是相对于父元素字体大小的百分比。
```css
p {
  font-size: 120%;
}
```

#### [1.7 color](#)
color 属性设置元素的文本以及文本装饰的前景色颜色值。

```css
p{
    color: red;
}
```

用于设置字体颜色，值被称为：颜色是由红（RED），绿（GREEN），蓝（BLUE ）光线的显示结合。

CSS中定义颜色使用十六进制（hex）表示法为红，绿，蓝的颜色值结合。可以是最低值是0（十六进制00）到最高值是255（十六进制FF），总额超过1600多万不同的颜色（256 × 256 ×256）。

```css
/* 关键字值 */
color: currentcolor;

/* <named-color> 值 */
color: red;
color: orange;
color: tan;
color: rebeccapurple;

/* <hex-color> 值 */
color: #090;
color: #009900;
color: #090a;
color: #009900aa;

/* <rgb()> 值 */
color: rgb(34, 12, 64, 0.6);
color: rgba(34, 12, 64, 0.6);
color: rgb(34 12 64 / 0.6);
color: rgba(34 12 64 / 0.3);
color: rgb(34 12 64 / 60%);
color: rgba(34.6 12 64 / 30%);

/* <hsl()> 值 */
color: hsl(30, 100%, 50%, 0.6);
color: hsla(30, 100%, 50%, 0.6);
color: hsl(30 100% 50% / 0.6);
color: hsla(30 100% 50% / 0.6);
color: hsl(30 100% 50% / 60%);
color: hsla(30.2 100% 50% / 60%);

/* <hwb()> 值 */
color: hwb(90 10% 10%);
color: hwb(90 10% 10% / 0.5);
color: hwb(90deg 10% 10%);
color: hwb(1.5708rad 60% 0%);
color: hwb(0.25turn 0% 40% / 50%);

/* 全局值 */
color: inherit;
color: initial;
color: revert;
color: revert-layer;
color: unset;
```

#### 使用关键字
除了像素、相对单位和百分比外，CSS还提供了一些关键字作为设置字体大小的选项。其中一些常见的关键字包括”small”、”medium”、”large”等。
- xx-small、x-small、small、medium、large、x-large、xx-large、xxx-large

### [2. @font-face](#)
**@font-face** CSS at-rule 指定一个用于显示文本的自定义字体；字体能从远程服务器或者用户本地安装的字体加载。

```css
@font-face {
  font-family: "Open Sans";
  src:
    url("/fonts/OpenSans-Regular-webfont.woff2") format("woff2"),
    url("/fonts/OpenSans-Regular-webfont.woff") format("woff");
}

@font-face {
    font-family: MyHelvetica;
    src: local("Helvetica Neue Bold"), local("HelveticaNeue-Bold"),
    url(./MgOpenModernaBold.ttf);
    font-weight: bold;
}

@font-face {
    font-family: 'Glyphicons Halflings';
    src: url('../static/fonts/iconfont/glyphicons-halflings-regular.eot');
    src: url('../static/fonts/iconfont/glyphicons-halflings-regular.eot?#iefix') format('embedded-opentype'),
    url('../static/fonts/iconfont/glyphicons-halflings-regular.woff') format('woff'),
    url('../static/fonts/iconfont/glyphicons-halflings-regular.ttf') format('truetype'),
    url('../static/fonts/iconfont/glyphicons-halflings-regular.svg#glyphicons_halflingsregular') format('svg');
}

@font-face {
    font-family: 'coling';
    src: url('../static/fonts/yiqiweiweiyixiao.ttf') format('truetype');
}


@font-face {
    font-family: 'relinks';
    src: url('../static/fonts/仓耳天群行楷 W01.ttf') format('truetype');
}

@font-face {
    font-family: 'remix';
    src: url('../static/fonts/义启简圆体.ttf') format('truetype');
}

@font-face {
    font-family: 'han';
    src: url('../static/fonts/汉仪书魂体简.ttf') format('truetype');
}

@font-face {
    font-family: 'survive';
    src: url('../static/fonts/碳纤维正粗黑简体.ttf') format('truetype');
}

body {
    font-family:  "Open Sans", "宋体";
}
```
* **font-family-name** 所指定的字体名字将会被用于 font 或 font-family 属性 ( i.e. font-family: <family-name>; )
* src src 定义了字体文件来源，src 中有几个需要注意的方法：
   * local 指定本地系统字体
   * url 指定文件地址，本地地址和网络地址均可，跟图片链接一样。
   * format 主要是用来帮助浏览器识别字体类型
     * truetype —— .ttf
     * opentype —— .ttf 或者 .otf
     * woff —— .woff
     * woff2 —— .woff2
     * Embedded Open Type(.eot)格式：.eot字体是IE专用字体，可以从TrueType创建此格式字体,支持这种字体的浏览器有【IE4+】；
* font-variant
* font-stretch
* font-weight
* font-style

```css
@font-face {
font-family: MyHelvetica;
src: local("Helvetica Neue Bold"), local("HelveticaNeue-Bold"),
  url(MgOpenModernaBold.ttf);
font-weight: bold;
}
```