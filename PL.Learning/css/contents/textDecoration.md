### [CSS 字体修饰](#)
**介绍** 设置文本的强调、下划线。

### [1. 属性大纲](#)

| 属性                                                                                                      | 版本	     |继承性| 简介                                    |
|:--------------------------------------------------------------------------------------------------------|:--------|:----|:--------------------------------------|
| [text-decoration](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration)                     | CSS 2.1 |无| 复合属性。检索或设置对象中的文本的装饰。                  |
| [text-decoration-color](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-color)         | CSS3    |无| 检索或设置对象中的文本装饰线条的颜色。                   |
| [text-decoration-line](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-line)           | CSS3    |无| 检索或设置对象中的文本装饰线条的位置。                   |
| [text-decoration-style](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-style)         | CSS3    |无| 检索或设置对象中的文本装饰线条的形状。                   |
| [text-decoration-skip](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-skip)           | CSS3    |有| 检索或设置对象中的文本装饰线条必须略过内容中的哪些部分。          |
| [text-decoration-thickness](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-thickness) | CSS3    |有| 用于设置元素中文本所使用的装饰线笔触厚度。                 |
| [text-underline-position](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-underline-position)     | CSS3    |有| 检索或设置对象中的下划线的位置。                      |
| [text-underline-offset](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-underline-offset)         | CSS3    |有| 设置文本装饰下划线与其原始位置的偏移距离。                 |
| [text-shadow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-shadow)                             | CSS3    |有| 设置或检索对象中文本的文字是否有阴影及模糊效果               |
| [text-emphasis](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-emphasis)                         | CSS3    |有|  将强调标记应用到除去空格和控制字符的文本。 |
| [text-emphasis-color](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-emphasis-color)             | CSS3    |有| 设置强调色，这个值也可以使用简写属性。 |
| [text-emphasis-style](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-emphasis-style)             | CSS3    |有| 设置强调标记的样式|


#### [1.1 text-decoration](#)
CSS 简写属性设置文本上的装饰性线条的外观。它是 text-decoration-line、text-decoration-color、text-decoration-style 和较新的 text-decoration-thickness 属性的缩写。

```css
p{
    text-decoration: green wavy underline;
}
```

#### [1.2 text-decoration-color](#)
text-decoration-color 用于设置文本修饰线的颜色，文本修饰线是通过 text-decoration-line 属性指定的。

```css
.example{
    /* <color> values */
    text-decoration-color: currentColor;
    text-decoration-color: red;
    text-decoration-color: #00ff00;
    text-decoration-color: rgba(255, 128, 128, 0.5);
    text-decoration-color: transparent;

    /* Global values */
    text-decoration-color: inherit;
    text-decoration-color: initial;
    text-decoration-color: unset;

}
```

#### [1.3 text-decoration-line](#)
用于设置元素中的文本的修饰类型。当要设置多个线修饰属性时，用 text-decoration 简写属性会比分别写多个属性更方便。

```css
.exp{
    /* Keyword values */
    text-decoration-line: none;
    text-decoration-line: underline;
    text-decoration-line: overline;
    text-decoration-line: line-through;
    text-decoration-line: blink;
    text-decoration-line: underline overline; /* Two decoration lines */
    text-decoration-line: overline underline line-through; /* Multiple decoration lines */

    /* Global values */
    text-decoration-line: inherit;
    text-decoration-line: initial;
    text-decoration-line: unset;

}
```

**值** text-decoration-line 属性可以设置为 none, 或者一个及多个用空格分隔的下列值。
* none 表示没有文本修饰效果。
* underline 在文本的下方有一条修饰线。
* overline 在文本的上方有一条修饰线。
* line-through 有一条贯穿文本中间的修饰线。

#### [1.4 text-decoration-style](#)
CSS 属性 text-decoration-style 用于设置由 text-decoration-line 设定的线的样式。

**值**
* solid 画一条实线。
* double 画一条双实线。
* dotted 画一条点划线。
* dashed 画一条虚线。
* wavy 画一条波浪线。

#### [1.5 text-underline-position](#)
当 text-decoration属性的值设置为 underline 之后，可以用 text-underline-position 属性为其设置下划线的位置。

* auto 用户代理 会根据自己的算法来确认下划线是放在字母基线上还是 放在字母基线下方。
* under 强制下划线的位置为字母基线的下方，在这个位置，下划线不会穿过任何字母，这样能确保数学方程式或者化学式的下标不会被下划线阻挡。
* left 在垂直排版模式下，使下划线的位置在文字的左侧，在水平排版模式下，和 under 相同。
* right 在垂直排版模式下，使下划线的位置在文字的右侧，在水平排版模式下，和 under 相同。
```html
<!DOCTYPE html>
<html>
<head>
<style>
div.a {
  text-decoration: underline;
  text-underline-position: auto;
}

div.b {
  text-decoration: underline;
  text-underline-position: under;
}

div.c {
  text-decoration: underline;
  text-underline-position: left;
}

div.d {
  text-decoration: underline;
  text-underline-position: right;
}

</style>
</head>
<body>

<h1>The text-underline-position Property</h1>

<div class="a">Text with underline and position auto: C<sub>6</sub>H<sub>12</sub>O<sub>16</sub> (glucose).</div>
<br>

<div class="b">Text with underline and position under: C<sub>6</sub>H<sub>12</sub>O<sub>16</sub> (glucose).</div>

<div class="c">Text with underline and position under: C<sub>6</sub>H<sub>12</sub>O<sub>16</sub> (glucose).</div>

<div class="d">Text with underline and position under: C<sub>6</sub>H<sub>12</sub>O<sub>16</sub> (glucose).</div>
<br>

</body>
</html>
```

### [2.text-emphasis](#)
这个值是 text-emphasis-style 和 text-emphasis-color 的简写属性。

```
/* 初始值 */
text-emphasis: none; /* 没有强调标记 */

/* <string> 值 */
text-emphasis: "x";
text-emphasis: "点";
text-emphasis: "\25B2";
text-emphasis: "*" #555;
text-emphasis: "foo"; /* 不应使用。它可能被计算或渲染为仅“f” */

/* 关键字值 */
text-emphasis: filled;
text-emphasis: open;
text-emphasis: filled sesame;
text-emphasis: open sesame;

/* 关键字值与色彩值结合 */
text-emphasis: filled sesame #555;

/* 全局值 */
text-emphasis: inherit;
text-emphasis: initial;
text-emphasis: revert;
text-emphasis: revert-layer;
text-emphasis: unset;
```

```css
em {
  text-emphasis-color: green;
  text-emphasis-style: "*";
}
```
