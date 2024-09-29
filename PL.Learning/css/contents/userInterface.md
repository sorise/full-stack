### [CSS 用户界面](#)
> **介绍** 设置界面的样式。


### [1. 属性大纲](#)

| 属性                                                                                 | 版本	   | 继承性 | 简介                              |
|:-----------------------------------------------------------------------------------|:------|:----|:--------------------------------|
| [text-overflow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-overflow)	   | CSS3  | 	无  | 	设置或检索是否使用一个省略标记（...）标示对象内文本的溢出 |
| [outline](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline)	               | CSS2	 | 无	  | 复合属性。设置或检索对象外的线条轮廓              |
| [outline-color](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline-color)    | 	CSS2 | 	无  | 	设置或检索对象外的线条轮廓的颜色               |
| [outline-style](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline-style)    | 	CSS2 | 	无  | 	设置或检索对象外的线条轮廓的样式               |
| [outline-width](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline-width)    | 	CSS2 | 	无  | 	设置或检索对象外的线条轮廓的宽度               |
| [outline-offset](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline-offset)	 | CSS3  | 	无  | 	设置或检索对象外的线条轮廓偏移位置的数值           |
| [user-select](https://developer.mozilla.org/zh-CN/docs/Web/CSS/user-select)        | CSS3  | 无 | 设置或检索是否允许用户选中文本。|
| [cursor](https://developer.mozilla.org/zh-CN/docs/Web/CSS/cursor)                  |	CSS2|	有|	设置或检索在对象上移动的鼠标指针采用何种系统预定义的光标形状。|
| [box-sizing](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-sizing)          |	CSS2|	有|	如何计算一个元素的总宽度和总高度。|
| [resize](https://developer.mozilla.org/zh-CN/docs/Web/CSS/resize)              |	CSS2|	有|	用于设置元素是否可调整尺寸，以及可调整的方向。|
| [zoom](https://juejin.cn/post/6844903945614147591)              |	CSS2|	有|	设置或检索对象的缩放比例。|


#### [1.1 text-overflow](#)
text-overflow 属性并不会强制“溢出”事件的发生，因此为了能让文本能够溢出容器，你需要在元素上添加几个额外的属性：overflow 和 white-space。例如：

```css
.text-overflow-hidden{
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
}
```
* **clip** 默认值。这个关键字会在内容区域的极限处截断文本，因此可能会在单词的中间发生截断。
* **ellipsis** 这个关键字会用一个省略号（'…'、U+2026 HORIZONTAL ELLIPSIS）来表示被截断的文本。

#### [1.2 outline](#)
CSS 的 outline 属性是在一条声明中设置多个轮廓属性的简写属性 ，例如 **outline-style**, **outline-width** 和 **outline-color**。

```
/* 样式 */
outline: solid;

/* 样式 | 颜色 */
outline: dashed #f66;

/* 宽度 | 样式 */
outline: thick inset;

/* 宽度 | 样式 | 颜色 */
outline: 3px solid green;
```

```css
a {
  border: 1px solid;
  border-radius: 3px;
  display: inline-block;
  margin: 10px;
  padding: 5px;
}

a:focus {
  outline: 4px dotted #e73;
  outline-offset: 4px;
  background: #ffa;
}
```

```html
<a href="#">该链接具有特殊的焦点样式。</a>
```

#### [1.3 outline-color](#)
outline-color CSS 属性 被用于设置一个元素轮廓的颜色。

* `<color>` 轮廓颜色，规则同 `<color>`.

```
outline-color: rgba(170, 50, 220, .6);
```

#### [1.4 outline-style](#)
outline-style CSS 属性被用于设置一个元素轮廓的样式。

* none 无轮廓 (outline-width 为 0).
* dotted 轮廓为一系列点。
* dashed 轮廓为一系列短线。
* solid 轮廓为实线。
* double 轮廓为两根有空隙的线。outline-width 为线与空间的总和。
* groove 轮廓呈凹下状。
* ridge 与 groove 相反：轮廓呈凸起状。
* inset 轮廓呈嵌入状。
* outset 与 inset 相反：轮廓呈突出状。

```css
.dotted {
  outline-style: dotted; /* 于 "outline: dotted"等价 */
}
.dashed {
  outline-style: dashed;
}

/* 让效果更清楚 */
* {
  outline-width: 10px;
  padding: 15px;
}
```
#### [1.5 outline-width](#)
CSS 属性 outline-width 用于设置一个元素的轮廓的厚度。元素轮廓是绘制于元素周围的一条线，位于 border 的外围。

**Values**:
* `<length>` The width of the outline specified as a <length>.
* `thin` 取决于用户代理。通常等同于桌面浏览器的 1px （包括 Firefox）。
* `medium` 取决于用户代理。通常等同于桌面浏览器的 3px （包括 Firefox）。
* `thick` 取决于用户代理。通常等同于桌面浏览器的 5px（包括 Firefox）。

```
/* Keyword values */
outline-width: thin;
outline-width: medium;
outline-width: thick;

/* <length> values */
outline-width: 1px;
outline-width: 0.1em;

/* Global values */
outline-width: inherit;
```

#### [1.6 outline-offset](#)
outline-offset CSS 属性设置轮廓与元素边缘或边框之间的间距。
* `<length>` 元素与其轮廓线之间的间距宽度。负值会将轮廓线置于元素内部。当值为 0 时，轮廓线与元素之间没有间距。

```
/* <length> 值 */
outline-offset: 3px;
outline-offset: 0.2em;
```

#### [1.7 user-select](#)
user-select CSS 属性用于控制用户是否可以选择文本。这不会对作为浏览器用户界面（即 chrome）的一部分的内容加载产生任何影响，除非是在文本框中。

```html
<style>
    .unselectable {
        -webkit-user-select: none; /* Safari */
        user-select: none;
    }

    .all {
        -webkit-user-select: all;
        user-select: all;
    }
</style>


<p>你应该可以选中这段文本。</p>
<p class="unselectable">嘿嘿，你不能选中这段文本！</p>
<p class="all">点击一次就会选中这段文本。</p>
```