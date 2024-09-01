## [CSS 定位](#)
> **介绍** : 定位允许你从正常的文档流布局中取出元素，并使它们具有不同的行为，例如放在另一个元素的上面，或者始终保持在浏览器视窗内的同一位置。本文解释的是定位 (position) 的各种不同值，以及如何使用它们。


### [1. 属性大纲](#)
关于定位有如下的css属性。

| 属性        | CSS Version/ 值   | 继承性	 | 简介                       |
|:----------|:-----------------|:-----|:-------------------------|
| position	 | CSS2/3           | 	无	  | 检索对象的定位方式                |
|           | static           | 	无   | 静态定位                     |
|           | relative         | 	无   | 相对定位                     |
|           | fixed            | 	无   | 固定定位                     |
|           | absolute         | 	无   | 绝对定位                     |
|           | sticky           | 	无   | 粘贴定位                     |
| z-index	  | CSS2             | 	无	  | 检索或设置对象的层叠顺序             |
| top	      | CSS2	            | 无	   | 检索或设置对象参照相对物顶边界向下偏移位置。   |
| right	    | CSS2	            | 无	   | 检索或设置对象参照相对物右边界向左偏移位置。   |
| bottom	   | CSS2	            | 无	   | 检索或设置对象参照相对物底边界向上偏移位置。   |
| left	     | CSS2	            | 无	   | 检索或设置对象参照相对物左边界向右偏移位置。   |
| clip	     | CSS2/3, **已弃用**	 | 无    | 	检索或设置对象的可视区域。区域外的部分是透明的 | 
| clip-path	     | CSS3, **已弃用**	 | 无    | 	CSS 的 clip-path 属性是 clip 属性的升级版，它们的作用都是对元素进行 “剪裁” | 

### [2. position](#)
**position** 属性规定应用于元素的定位方法的类型（static、relative、fixed、absolute 或 sticky）。

**HTML 元素默认情况下的定位方式为 static静态定位** ，它始终根据页面的正常流进行定位。

* 有五个不同的位置值：
  * static 静态定位，静态定位的元素不受 top、bottom、left 和 right 属性的影响
  * relative 生成相对定位的元素，相对于 其正常位置 进行定位，不脱离文档流。
  * fixed 生成固定定位的元素，相对于 **浏览器窗口** 进行定位，**脱离文档流**。（老 IE 不支持）
  * absolute 生成绝对定位的元素，**相对于 static 定位以外的第一个父元素 进行定位**，**如果父级不是，会一直往上到 body**，脱离文档流。
  * sticky 粘性定位可以被认为是相对定位和固定定位的混合。元素在跨越特定阈值前为相对定位，之后为固定定位。 **主要用于 scroll 事件的监听上**。

#### 2.1 静态定义
元素不会以任何特殊方式定位；它始终根据页面的正常流进行定位：
```css
div .static {
  position: static;
  border: 3px solid #73AD21;
}
```

#### 2.2 相对定位 
设置相对定位的元素的 top、right、bottom 和 left 属性将导致其偏离其正常位置进行调整。
不会对其余内容进行调整来适应元素留下的任何空间,**不会脱离文档流**。

```html
<style>
  .box {
    display: inline-block;
    width: 100px;
    height: 100px;
    background: red;
    color: white;
  }

  #two {
    position: relative;
    top: 20px;
    left: 20px;
    background: blue;
  }
</style>
<div class="box" id="one">One</div>
<div class="box" id="two">Two</div>
<div class="box" id="three">Three</div>
<div class="box" id="four">Four</div>
```

#### 2.3 绝对定位
元素**相对于最近的已定位祖先元素**（具有position值为relative, absolute或fixed的祖先元素）进行定位。

**脱离文档流，不再占据原来的空间，因此会影响其他元素的位置**。可以使用top, right, bottom, left来精确控制元素的位置。

**用途**：适合创建弹出层、下拉菜单等需要精确控制位置的元素。

```html
<style>
main{
  position: relative;
  background-color: green;
}
div{
  border: 1px solid gainsboro;
  width: 200px;
  height: 200px;
  background-color: white;
  text-align: center;
  align-content: center;
}
.relative_li:hover{
  position: absolute;
  top: 0px;
  left: 15px;
  background-color: rebeccapurple;
}
</style>
<main>
   <div class="static">1</div>
    <div class="relative_li">2</div>
</main>
```
#### 2.4 固定定位
元素会被移出正常文档流，并不为元素预留空间，而是通过指定元素相对于屏幕视口（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变。打印时，元素会出现在的每页的固定位置。

**意味着即使滚动页面，它也始终位于同一位置**。 top、right、bottom 和 left 属性用于定位此元素。

固定定位的元素不会在页面中通常应放置的位置上留出空隙。

```html
<style>
  body{
    height: 1800px;
  }
  main{
    position: relative;
    background-color: green;
  }
  div{
    border: 1px solid gainsboro;
    width: 200px;
    height: 200px;
    background-color: white;
    text-align: center;
    align-content: center;
  }
  .fixed_test{
    position: fixed;
    bottom: 10px;
  }
</style>

<body>
    <main>
       <div class="static">1</div>
        <div class="relative_li">2</div>
    </main>
    <div class="fixed_test">fix</div>
</body>
```

#### [2.5 粘贴定位](#)
元素根据正常文档流进行定位，然后相对它的最近滚动祖先（nearest scrolling ancestor）和 containing block（最近块级祖先 nearest block-level ancestor），包括 table-related 元素，基于 top、right、bottom 和 left 的值进行偏移。偏移值不会影响任何其他元素的位置。

**元素的行为类似于relative定位，但在特定情况下（如用户滚动到该元素附近时）会变成fixed定位**。

* **特性**：当用户滚动到某个阈值时，元素会“粘”在某个位置上，直到另一个元素占据其位置。
* **用途**：适用于希望在用户滚动页面时暂时固定某个元素（如头部导航）的情况。

Sticky 定位的工作原理
* 初始阶段：元素根据文档流定位。
* 触发条件：当元素进入其父容器（通常是 `<body>`或`<html>`元素）的可视区域边界时，sticky定位开始生效。
* 固定阶段：一旦触发条件满足，元素会变为fixed定位，并固定在指定的位置上。
* 返回原位：当元素离开其固定的边界时，它会回到其原来的文档流位置。
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Sticky Header Example</title>
  <style>
    .sticky-header {
      background-color: lightblue;
      padding: 10px;
      position: sticky;
      top: 0;
    }
    .content {
      height: 2000px; /* 使内容足够长以便滚动 */
      padding-top: 60px; /* 避免覆盖粘性元素 */
    }
  </style>
</head>
<body>
<header class="sticky-header">这是粘性标题</header>
<div class="content">
  这里是一些内容，当您向下滚动时，标题将会固定在顶部。
</div>
</body>
</html>
```
在这个例子中，当页面滚动到标题下方时，标题会固定在页面的顶部，直到再次滚动到标题的原始位置。

* sticky定位要求父元素具有overflow属性非visible的值（如auto、scroll），或者父元素也必须具有position属性非static的值（如relative），否则sticky定位可能不会按预期工作。
* sticky定位在不同的浏览器中支持程度不同。虽然现代浏览器普遍支持sticky定位，但为了确保兼容性，最好检查Can I Use网站上的支持情况。
* 使用sticky定位时，要注意避免元素之间的重叠问题，可以通过调整padding或margin来解决。

> * 如果 position:sticky 元素的任意父节点定位设置为 overflow: hidden，则父容器无法进行滚动，所以 position: sticky 元素也不会有滚动然后固定的情况。
> * 如果 position:sticky 元素的任意父节点定位设置为 position: relative | absolute | fixed，则元素相对父元素进行定位，而不会相对 viewprot 定位。

### [3. z-index](#)
z-index 属性指定元素的堆栈顺序（哪个元素应放置在其他元素的前面或后面）。

> 在 CSS 2.1 中，所有的盒模型元素都处于三维坐标系中。除了我们常用的横坐标和纵坐标，盒模型元素还可以沿着“z 轴”层叠摆放，当他们相互覆盖时，z 轴顺序就变得十分重要。

**属性值**:
* auto	默认。堆叠顺序与父元素相等。
* number	设置元素的堆叠顺序。
* inherit	规定应该从父元素继承 z-index 属性的值。

```html
<style>
  main{
    position: relative;
    border: 1px solid gray;
    padding: 10px;
  }
  div{
    border: 1px solid grey;
    width: 200px;
    height: 200px;
    background-color: gainsboro;
    text-align: center;
    align-content: center;
    z-index: 0;
  }
  .float-layout::after{
    /*清楚浮动*/
    display: block;
    content: "";
    clear: both;
  }
  .float-layout .float-left{
    float: left;
  }
  .float-layout .float-right{
    float: right;
  }
  .f0{
    position: relative;
  }
  .f1{
    position: relative;
    top: 15px;
    left: -30px;
  }
  .f2{
    position: relative;
    top: 30px;
    left: -60px;
  }
  .f3{
    position: relative;
    top: 45px;
    left: -90px;
  }
  .vh{
    transition-timing-function: linear;
    transition-duration: 0.2s;
  }
  .vh:hover{
    z-index: 10;
    width: 210px;
    height: 210px;
  }
</style>
<main class="float-layout">
   <div class="vh float-left f0">1</div>
   <div class="vh float-left f1">2</div>
   <div class="vh float-left f2">3</div>
   <div class="vh float-left f3">4</div>
</main>
```

z-index 属性在下列情况下会**失效**：
* 父元素 position 为 relative 时，子元素的 z-index 失效
  * 解决：父元素 position 改为 absolute 或 static
* 元素没有设置 position 属性为非 static 属性
  * 解决：设置该元素的 position 属性为 relative、absolute 或是 fixed 中的一种
* 元素在设置 z-index 的同时还设置了 float 浮动
  * 解决：float 去除，改为 display：inline-block

### [4. clip](#)
clip 属性定义了元素的哪一部分是可见的。clip 属性只适用于 position:absolute 的元素。

> **已弃用**: 不再推荐使用该特性。虽然一些浏览器仍然支持它，但也许已从相关的 web 标准中移除，也许正准备移除或出于兼容性而保留。

当一幅图像的尺寸大于包含它的元素时会发生什么呢？"clip" 属性允许您规定一个元素的可见尺寸，这样此元素就会被修剪并显示为这个形状。

```css
img
{
  position:absolute;
  clip:rect(0px,60px,200px,0px);
}
```

#### [4.1 clip-path](#)
CSS 的 clip-path 属性是 clip 属性的升级版，它们的作用都是对元素进行 “剪裁”，不同的是 clip 只能作用于 position 为 absolute 和 fixed 的元素且剪裁区域只能是正方形，而 clip-path 更加强大，可以以任意形状去裁剪元素，且对元素的定位方式没有要求。基于这样的特性，clip-path 常用于实现一些炫酷的动画效果。

**非常高级，必须掌握**：
- [clip-path 语法规则](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clip-path)
- [前端小技巧：CSS clip-path 的妙用](https://segmentfault.com/a/1190000023301221)
- [bilibili 视频教程](https://www.bilibili.com/video/BV1be41197iL/?spm_id_from=333.337.search-card.all.click&vd_source=a03ca1a86c1e90990c4facd27ae17815)
- [语法辅助工具](https://tools.jb51.net/code/css3path) CSS3剪贴路径（Clip-path）在线生成器工具
**语法**
```css
/* Keyword values */
clip-path: none;

/* <clip-source> values */
clip-path: url(resources.svg#c1);

/* <geometry-box> values */
clip-path: margin-box;
clip-path: border-box;
clip-path: padding-box;
clip-path: content-box;
clip-path: fill-box;
clip-path: stroke-box;
clip-path: view-box;

/* <basic-shape> values */
clip-path: inset(100px 50px);
clip-path: circle(50px at 0 100px);
clip-path: ellipse(50px 60px at 0 10% 20%);
clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
clip-path: path(
  "M0.5,1 C0.5,1,0,0.7,0,0.3 A0.25,0.25,1,1,1,0.5,0.3 A0.25,0.25,1,1,1,1,0.3 C1,0.7,0.5,1,0.5,1 Z"
);

/* Box and shape values combined */
clip-path: padding-box circle(50px at 0 100px);

/* Global values */
clip-path: inherit;
clip-path: initial;
clip-path: revert;
clip-path: revert-layer;
clip-path: unset;
```

