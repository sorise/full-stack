### [CSS 字体](#)
**介绍** 设置字体的大小、样式、粗细、大小写、字体、拉伸。

### [1. 属性大纲](#)

| 属性                                                                            | 版本	     |Inherit From Parent 继承性|	Description 简介|
|:------------------------------------------------------------------------------|:--------|:----|:----|
| font                                                                          |CSS1/2|有|复合属性。设置或检索对象中的文本特性|
| [font-family](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-family)   |CSS1|有|设置或检索用于对象中文本的字体名称序列|
| [font-style](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-style)     |CSS1|有|设置或检索对象中的字体样式|
| [font-variant](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-variant) |CSS1|有|设置或检索对象中的文本是否为小型的大写字母|
| [font-weight](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-weight)   |CSS1|有|设置或检索对象中的文本字体的粗细|
| [font-size](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-weight)     |CSS1|有|设置或检索对象中的字体尺寸|
| [font-stretch](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-stretch) |CSS3|有|设置或检索对象中的文字是否横向拉伸变形。|
| [@font-face](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face)     |CSS3|有|设置自定义字体|

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

#### [1.1 font-family](#)
font-family 允许您通过给定一个有先后顺序的，由字体名或者字体族名组成的列表来为选定的元素设置字体。

```css
p {
    font-family: "微软雅黑", 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    font-weight: bold;
    font-size: 17px;
}
```

#### [1.2 font-style](#)

```css
.sample {
  font: 2rem 'AmstelvarAlpha', sans-serif;
  font-style: oblique 23deg;
}
```

* normal 选择 font-family 的常规字体。
* italic 选择斜体，如果当前字体没有可用的斜体版本，会选用倾斜体（oblique ）替代。
* oblique 选择倾斜体，如果当前字体没有可用的倾斜体版本，会选用斜体（italic ）替代。

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