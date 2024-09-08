### [CSS 背景](#)
**介绍** CSS 背景属性用于定义HTML元素的背景。

### [1. 属性大纲](#)
背景图像使用 background-image 定义。图像使用 background-position 居中。为多个背景图像使用 background-clip
属性的不同值，使背景图像保持在内容框内。背景颜色被裁剪到填充框，防止背景通过 border-image 和 dotted border
的透明部分显示出来。右侧元素中的圆角使用 border-radius 属性创建。使用单个 box-shadow 声明设置所有阴影，包括
内阴影和外阴影。

| 属性                                                                                              | 版本	     |Inherit From Parent 继承性|	Description 简介|
|:------------------------------------------------------------------------------------------------|:--------|:----|:----|
| background	                                                                                     | CSS 2.1 |无|复合属性。设置或检索对象的背景特性|
| background-color                                                                                | CSS 2.1 |无|设置或检索对象的背景颜色|
| background-image                                                                                | CSS 2.1 |无|设置或检索对象的背景图像|
| [background-repeat](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-repeat)         | CSS 2.1 |无|设置或检索对象的背景图像如何铺排填充|
| [background-attachment](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-attachment) | CSS 2.1 |无|设置或检索对象的背景图像是随对象内容滚动还是固定的|
| [background-position](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-position)     | CSS 2.1 |无|设置或检索对象的背景图像位置|
| [background-origin](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-origin)         | CSS3    |无|设置或检索对象的背景图像显示的原点|
| [background-clip](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)             | CSS3    |无|检索或设置对象的背景向外裁剪的区域|
| [background-size](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-size)             | CSS3    |无|检索或设置对象的背景图像的尺寸大小|
| [background-blend-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-blend-mode) | CSS3    |无|定义该元素的背景图片，以及背景色如何混合。|


#### [1.1 background](#)
background 是一种 CSS 简写属性，用于一次性集中定义各种背景属性，包括 color, image, origin 与 size, repeat 方式等等。

```css
.div1 {
    width: 100px;
    height: 100px;
    background-color: black;
    background-image: url('img1');
    background-size: 50%;
    background-repeat: no-repeat;
}

.grammar{
    /* 使用 <background-color> */
    background: green;

    /* 使用 <bg-image> 和 <repeat-style> */
    background: url("test.jpg") repeat-y;

    /* 使用 <box> 和 <background-color> */
    background: border-box red;

    /* 将背景设为一张居中放大的图片 */
    background: no-repeat center/80% url("../img/image.png");
}
```
Background 可以使用简写或者单独设置其中一项：

简写语法
* 官方推荐顺序为： background: background-color，background-image，background-repeat，background-attachment，background-position;
不强制要求书写顺序。
* 单独设置样式

#### [1.2 background-color](#)
CSS属性中的 background-color 会设置元素的背景色，属性的值为颜色值或关键字"transparent"二者选其一。

* `<color>` 一个描述背景统一颜色的 CSS <color> 值。即使一个或几个的 background-image 被定义，如果图像
* 是不透明的，通过透明度该颜色也能影响到渲染。在 CSS 中，**transparent** 是一种颜色。

```
/* Keyword values */
background-color: red;

/* Hexadecimal value */
background-color: #bbff00;

/* RGB value */
background-color: rgb(255, 255, 128);

/* HSLA value */
background-color: hsla(50, 33%, 25%, 0.75);

/* Special keyword values */
background-color: currentColor;
background-color: transparent;

/* Global values */
background-color: inherit;
background-color: initial;
background-color: unset;
```

#### [1.3 background-clip](#)
background-clip 设置元素的背景（背景图片或颜色）是否延伸到边框、内边距盒子、内容盒子下面。

**值**
* border-box 背景延伸至边框外沿（但是在边框下层）。
* padding-box 背景延伸至内边距（padding）外沿。不会绘制到边框处。
* content-box 背景被裁剪至内容区（content box）外沿。
* **text 实验性** 背景被裁剪成文字的前景色。

```
/* Keyword values */
background-clip: border-box;
background-clip: padding-box;
background-clip: content-box;
background-clip: text;

/* Global values */
background-clip: inherit;
background-clip: initial;
background-clip: unset;
```

#### [1.4 background-image](#)
background-image 属性用于为一个元素设置一个或者多个背景图像。


```
background-image: url("../../media/examples/lizard.png"),
                  url("../../media/examples/star.png");
```
在绘制时，图像以 z 方向堆叠的方式进行。先指定的图像会在之后指定的图像上面绘制。因此指定的第一个图像“最接近用户”。

**可以提供由逗号分隔的多个值来指定多个背景图像**：
```css
div{
    background-image: linear-gradient(
            to bottom,
            rgba(255, 255, 0, 0.5),
            rgba(0, 0, 255, 0.5)
    ),
    url("catfront.png");
}
```

#### [1.5 background-attachment](#)

* fixed 此关键属性值表示背景相对于视口固定。即使一个元素拥有滚动机制，背景也不会随着元素的内容滚动。
* local 此关键属性值表示背景相对于元素的内容固定。如果一个元素拥有滚动机制，背景将会随着元素的内容滚动，并且背景的绘制区域和定位区域是相对于可滚动的区域而不是包含他们的边框。
* scroll 此关键属性值表示背景相对于元素本身固定，而不是随着它的内容滚动（对元素边框是有效的）。