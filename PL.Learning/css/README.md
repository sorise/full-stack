## [CSS 层叠样式表](#)
> **介绍:** 层叠样式表（Cascading Style Sheets，缩写为 CSS）是一种样式表语言，用来描述 HTML 或 XML
> （包括如 SVG、MathML 或 XHTML 之类的 XML 分支语言）文档的呈现方式。CSS 描述了在屏幕、纸质、音频等其
> 他媒体上的元素应该如何被渲染的问题。

### 笔记目录

#### 1.选择器

* [基础选择符、选择器交集和并集](./contents/selectors/element.md) 通配选择符`*`、类型选择符ID 选择符、类选择符
* [关系选择符](./contents/selectors/relationship.md) `> + ~`
* [属性选择符](./contents/selectors/attribute.md) `E[attr]`
* [伪类选择符、伪对象选择符](./contents/selectors/pseudo-classes.md) `链接伪类:hover`、`结构性伪类:not`、`UI 伪类:checked、:disabled、:enabled` `::before、::after、::first-letter、::first-line...`
* [嵌套选择器](./contents/selectors/nested-selector.md) `&`

#### 2.字体、文字样式
  
* [字体](./contents/fonts.md) `font-*`
* [文本](./contents/texts.md) `text-*`、`line-height`、`word-*`、`white-space`
* [文本修饰](./contents/textDecoration.md) `text-decoration-*`、`text-shadow`、`文字强调：text-emphasis`
* [书写模式](./contents/writingModes.md) `direction`、`writing-mode` 决定文本行的排列方向。`text-orientation` 决定单个字符在垂直行内的朝向。 可以用于古诗词纵向排列。

#### 3.定位、盒子

* [定位、剪裁](./contents/position.md)  `position`、`z-index`、`clip-path`
* [浮动布局，盒模型](./contents/boxModel.md)  `display`、`overflow`、`float`、`clear`、`visibility`、`box-sizing`。
* [内外边距](./contents/marginPadding.md) 外补白 `margin` 内补白 `padding`
* [尺寸](./contents/dimension.md) 长宽 `resize` `width`、`min-width`
* [边框](./contents/border.md) `border-*`、`border-radius`、`box-shadow`、`border-image`
* [背景](./contents/background.md) `background-*`

#### 内容样式

* [颜色、透明度](./contents/colorOpacity.md) `opacity`、`color：rgba(x,y,z,e)`
* [列表](./contents/list.md) `list-style-*`
* [表格](./contents/table.md) `table-layout`、`border-collapse、border-spacing、caption-side、empty-cells、vertical-align`
* [内容](./contents/content.md) `content、counter-*`
* [用户界面](./contents/userInterface.md) `outline-*`、`cursor`、`zoom`
* [多列](./contents/columns.md) `column-*`

#### 页面布局

* [伸缩盒 flex 布局](./contents/flex.md) `flex-*`、`align-*`、`order`、`justify-content`  `gap`
* [网格布局 grid](./contents/grid.md) `grid-template-*` `grid-row-*、grid-column-*` `gap` `grid-template-areas、grid-areas`

#### 动画

* [过渡](./contents/transition.md)
* [变换](./contents/transform.md)
* [动画](./contents/keyframes)
* [打印](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Guides/Media_queries/Printing)
* 媒体查询
* Only IE
* Only Firefox
* Only Webkit
* 语法与规则
* 优先级
* 取值
  * 文本
  * 函数
  * [图像、渐变](./contents/units/imageGradient.md) `linear-gradient`
  * 数字
  * 其他
  - [颜色](./contents/units/color.md)
* 单位
  * [长度](./contents/units/width.md)
  * [角度](./contents/units/deg.md)
  * 时间
  * 频率
  * 布局
* 附录 Appendix
* CSS Hack
* 问题与经验
* CSS 简介
  * [更新历史](./contents/history.md)
  * [语法规则、变量](./contents/grammar.md)

### 相关链接
- [caniuse](https://caniuse.com/) - CSS 兼容性查询网站
- [cascading style sheets home page](https://www.w3.org/Style/CSS/#specs) - W3C 层叠样式表规范
- [css-guidebook](https://tsejx.github.io/css-guidebook/) - CSS Guidebook
- [掘金 CSS 专栏](https://juejin.cn/tag/CSS)

### 小工具
- [CSS clip-path 生成器](https://www.jiangweishan.com/tool/clippy/)