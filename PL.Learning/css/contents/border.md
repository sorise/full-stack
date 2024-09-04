## [CSS 边框样式](#)
> **介绍** : 


### [1. 属性大纲](#)
关于边框的css属性。

| 属性                         |CSS Version 版本|继承性|简介|
|:---------------------------|:---|:---|:---|
| border                     |CSS1|无|复合属性。设置对象边框的特性|
| border-width               |CSS1|无|设置或检索对象边框宽度|
| border-style               |CSS1|无|设置或检索对象边框样式|
| border-color               |CSS1|无|设置或检索对象边框颜色|
| border-top                 |CSS1|无|复合属性。设置对象顶边的特性|
| border-top-width           |CSS1|无|设置或检索对象顶边宽度|
| border-top-style           |CSS1|无|设置或检索对象顶边样式|
| border-top-color           |CSS1|无|设置或检索对象顶边颜色|
| border-right               |CSS1|无|复合属性。设置对象右边的特性|
| border-right-width         |CSS1|无|设置或检索对象右边宽度|
| border-right-style         |CSS1|无|设置或检索对象右边样式|
| border-right-color         |CSS1|无|设置或检索对象右边颜色|
| border-bottom              |CSS1|无|复合属性。设置对象底边的特性|
| border-bottom-width        |CSS1|无|设置或检索对象底边宽度|
| border-bottom-style        |CSS1|无|设置或检索对象底边样式|
| border-bottom-color        |CSS1|无|设置或检索对象底边颜色|
| border-left                |CSS1|无|复合属性。置对象左边的特性|
| border-left-width          |CSS1|无|设置或检索对象左边宽度|
| border-left-style          |CSS1|无|设置或检索对象左边样式|
| border-left-color          |CSS1|无|设置或检索对象左边颜色|
| **border-radius**            |CSS3|无|设置或检索对象使用圆角边框|
| border-top-left-radius     |CSS3|无|设置或检索对象左上角圆角边框|
| border-top-right-radius    |CSS3|无|设置或检索对象右上角圆角边框|
| border-bottom-right-radius |CSS3|无|设置或检索对象右下角圆角边框|
| border-bottom-left-radius  |CSS3|无|设置或检索对象左下角圆角边框|
| **box-shadow**                 |CSS3|无|设置或检索对象阴影|
| **border-image**               |CSS3|无|设置或检索对象的边框样式使用图像来填充|
| border-image-source        |CSS3|无|设置或检索对象的边框是否用图像定义样式或图像来源路径|
| border-image-slice         |CSS3|无|设置或检索对象的边框背景图的分割方式|
| border-image-width         |CSS3|无|设置或检索对象的边框厚度|
| border-image-outset        |CSS3|无|设置或检索对象的边框背景图的扩展|
| border-image-repeat        |CSS3|无|设置或检索对象的边框图像的平铺方式|

### [2 border](#)
CSS 的 border 属性是一个用于设置各种单独的边界属性的简写属性。border 可以用于设置一个或多个以下属性的值：**border-width**、**border-style**、**border-color**。

**语法**：
```css
.example{
    border：[ border-width ] || [ border-style ] || [ border-color ]
}

.exp{
    /* width | style | color */
    border: 1px dashed green;
}
```

#### [2.1 border-width](#)
`border-width` 用于设置元素边框的宽度。

* 值 `<line-width>` 定义边框的宽度，可以是明确的非负数 <length> 或关键字。如果是关键字，则必须是以下值之一：
  * thin
  * medium
  * thick

**border-width** 属性可以通过一个、两个、三个或四个值来指定。
* 当指定一个值时，该宽度将应用于四条边。
* 当指定两个值时，第一个宽度应用于顶部和底部，第二个宽度应用于左侧和右侧。
* 当指定三个值时，第一个宽度应用于顶部, 第二个宽度应用于左侧和右侧, 第三个宽度应用于底部.
* 当指定四个值时，这些宽度按照顶部、右侧、底部和左侧的顺序（顺时针）进行应用。

```css
/* 关键字值 */
border-width: thin;
border-width: medium;
border-width: thick;

/* <length> 值 */
border-width: 4px;
border-width: 1.2rem;

/* 顶部和底部 | 左侧和右侧 */
border-width: 2px 1.5em;

/* 顶部 | 左侧和右侧 | 底部 */
border-width: 1px 2em 1.5cm;

/* 顶部 | 右侧 | 底部 | 左侧 */
border-width: 1px 2em 0 4rem;
```

#### [2.2 border-style](#)
border-style 用来设定元素所有边框的样式。

> border-style 默认值是 none，这意味着如果你只修改 border-width 和 border-color 是不会出现边框的。

```css
/* Apply to all four sides */
border-style: dashed;

/* horizontal | vertical */
border-style: dotted solid;

/* top | horizontal | bottom */
border-style: hidden double dashed;

/* top | right | bottom | left */
border-style: none solid dotted dashed;

/* Global values */
border-style: inherit;
border-style: initial;
border-style: unset;
```
**取值**
* **none** ：和关键字 hidden 类似，不显示边框。在这种情况下，如果没有设定背景图片，border-width 计算后的值将是 0，即使先前已经指定过它的值。在单元格边框重叠情况下，none 值优先级最低，意味着如果存在其他的重叠边框，则会显示为那个边框。
* **hidden** ：和关键字 none 类似，不显示边框。在这种情况下，如果没有设定背景图片，border-width 计算后的值将是 0，即使先前已经指定过它的值。在单元格边框重叠情况下，hidden 值优先级最高，意味着如果存在其他的重叠边框，边框不会显示。
* **dotted** ：显示为一系列圆点。标准中没有定义两点之间的间隔大小，视不同实现而定。圆点半径是 border-width 计算值的一半。
* **dashed** ：显示为一系列短的方形虚线。标准中没有定义线段的长度和大小，视不同实现而定。
* **solid** ：显示为一条实线。
* **double** ：显示为一条双实线，宽度是 border-width 。
* **groove** ：显示为有雕刻效果的边框，样式与 ridge 相反。
* **ridge** ：显示为有浮雕效果的边框，样式与 groove 相反。
* **inset** ：显示为有陷入效果的边框，样式与 outset 相反。当它指定到 border-collapse 为 collapsed 的单元格时，会显示为 groove 的样式。
* **outset** ：显示为有突出效果的边框，样式与 inset 相反。当它指定到 border-collapse 为 collapsed 的单元格时，会显示为 ridge 的样式。


#### [2.3 border-color](#)
可以确定 border 的颜色。如果这个值没有设置，它的默认值是元素的 color 属性值（是文字颜色而非背景色）。


```css
/* border-color: color; 单值语法 */
border-color: red;

/* border-color: vertical horizontal; 双值语法*/
border-color: red #f015ca;

/* border-color: top horizontal bottom; 三值语法 */
border-color: red yellow green;

/* border-color: top right bottom left; 四值语法 */
border-color: red yellow green blue;

border-color: inherit;
```


### [3. border-radius](#)
允许你设置元素的外边框圆角。当使用一个半径时确定一个圆形，当使用两个半径时确定一个椭圆。这个（椭）圆与边框的交集形成圆角效果。


```css
border-radius: 30px;
border-radius: 25% 10%;
border-radius: 10% 30% 50% 70%;
border-radius: 10% / 50%;  /* 两个半径椭圆 */
border-radius: 10px 100px / 120px;
border-radius: 50% 20% / 10% 40%;
```
该属性是一个 简写属性，是为了将这四个属性 border-top-left-radius、border-top-right-radius、border-bottom-right-radius，和 border-bottom-left-radius 简写为一个属性。

**值**:
* 可以是 `<length>` 或者 `<percentage>`，表示边框四角的圆角半径。
* `<length>` / `<length>` or  `<percentage>` / `<percentage>` 。 定义一个椭圆。

### [4. box-shadow](#)
属性用于在元素的框架上添加阴影效果。你可以在同一个元素上设置多个阴影效果，并用逗号将他们分隔开。该属性可设置的值包括阴影的 X 轴偏移量、Y 轴偏移量、模糊半径、扩散半径和颜色。

**取值**
* inset  如果没有指定inset，默认阴影在边框外，即阴影向外扩散。 使用 inset 关键字会使得阴影落在盒子内部，这样看起来就像是内容被压低了。此时阴影会在边框之内 (即使是透明边框）、背景之上、内容之下。
* `<offset-x>` `<offset-y>` 这是头两个 `<length>` 值，用来设置阴影偏移量。x,y 是按照数学二维坐标系来计算的，
只不过 y 垂直方向向下。 <offset-x> 设置水平偏移量，正值阴影则位于元素右边，负值阴影则位于元素左边。 `<offset-y>` 
设置垂直偏移量，正值阴影则位于元素下方，负值阴影则位于元素上方。可用单位请查看 `<length>` 。 如果两者都是 0，那么阴
影位于元素后面。这时如果设置了`<blur-radius>` 或`<spread-radius>` 则有模糊效果。需要考虑 inset
* `<blur-radius>` 这是第三个 `<length>` 值。值越大，模糊面积越大，阴影就越大越淡。不能为负值。默认为 0，此时阴影边缘锐利。本规范不包括如何计算模糊半径的精确算法，但是，它详细说明如下：
对于长而直的阴影边缘，它会创建一个过渡颜色用于模糊 以阴影边缘为中心、模糊半径为半径的局域，过渡颜色的范围在完整的阴影颜色到它最外面的终点的透明之间。 （译者注：对此有兴趣的可以了解下数字图像处理的模糊算法。）
* `<spread-radius>` 这是第四个 `<length>` 值。取正值时，阴影扩大；取负值时，阴影收缩。默认为 0，此时阴影与元素同样大。需要考虑 inset
* `<color>` : 相关事项查看 `<color>` 。如果没有指定，则由浏览器决定——通常是color的值，不过目前 Safari 取透明。

```css
/* x 偏移量 | y 偏移量 | 阴影颜色 */
box-shadow: 60px -16px teal;

/* x 偏移量 | y 偏移量 | 阴影模糊半径 | 阴影颜色 */
box-shadow: 10px 5px 5px black;

/* x 偏移量 | y 偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);

/* 插页 (阴影向内) | x 偏移量 | y 偏移量 | 阴影颜色 */
box-shadow: inset 5em 1em gold;

/* 任意数量的阴影，以逗号分隔 */
box-shadow:
  3px 3px red,
  -1em 0 0.4em olive;

/* 全局关键字 */
box-shadow: inherit;
box-shadow: initial;
box-shadow: unset;
```


向元素添加单个 box-shadow 效果时使用以下规则：
* 当给出两个、三个或四个 `<length>` 值时。
  * 如果只给出两个值，那么这两个值将会被当作 `<offset-x><offset-y>` 来解释。
  * 如果给出了第三个值，那么第三个值将会被当作 `<blur-radius>` 解释。
  * 如果给出了第四个值，那么第四个值将会被当作 `<spread-radius>` 来解释。
  * 可选，inset关键字。
  * 可选，<color>值。

若要对同一个元素添加多个阴影效果，请使用逗号将每个阴影规则分隔开。

```css
blockquote {
  padding: 20px;
  box-shadow:
    inset 0 -3em 3em rgba(0, 0, 0, 0.1),
    0 0 0 2px rgb(255, 255, 255),
    0.3em 0.3em 1em rgba(0, 0, 0, 0.3);
}
```


### [5. border-image](#)