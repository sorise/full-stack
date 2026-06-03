### [CSS 书写模式](#)
**介绍** 设置文本的书写方式。

### [1. 属性大纲](#)

| 属性                                                                            | 版本	     |继承性| 简介                                    |
|:------------------------------------------------------------------------------|:--------|:----|:--------------------------------------|
|[direction](https://developer.mozilla.org/zh-CN/docs/Web/CSS/direction)|CSS2|有|检索或设置文本流的方向|
|[unicode-bidi](https://developer.mozilla.org/zh-CN/docs/Web/CSS/unicode-bidi) |CSS2|无|	用于同一个页面里存在从不同方向读进的文本显示。与direction属性一起使用|
|[writing-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/writing-mode) |CSS3|有|负责宏观布局，决定文本行的排列方向。|
|[text-orientation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-orientation) |CSS3|有|负责微观细节，决定单个字符在垂直行内的朝向。|

#### [1.1 direction](#)
请注意，文本方向通常在文档中定义（例如，使用 HTML 的 dir 属性 属性），而不是通过直接使用 direction 属性来定义。

```css
/* 关键字值 */
direction: ltr; 
direction: rtl;
```

**值**：
- **ltr** 默认属性。可设置文本和其他元素的默认方向是从左到右。
- **rtl** 可设置文本和其他元素的默认方向是从右到左。

仅仅设置 direction 属性毫无意义。

#### [1.2 unicode-bidi](#)

unicode-bidi 是一个 CSS 属性，用于控制文本的方向性，特别是在涉及双向文本（如同时包含从左到右和从右到左的文本，如英语和阿拉伯语混合）的情境中。这个属性主要用于确保文本的正确渲染和布局，特别是在复杂的文本环境中。

unicode-bidi 属性的可能值包括：

- normal：这是默认值。文本的方向性由字符本身的 Unicode 双向算法来决定。也就是说，大部分拉丁字母文字会从左到右显示，而像阿拉伯语或希伯来语这样的文字则会从右到左显示。
- embed：此值将创建一个新的双向文本嵌入级别。这意味着，如果一段文本被设置为 embed，它将根据其自身的方向性（由字符决定）来渲染，而不会受到外部文本方向性的影响。这有助于在更大的文本流中嵌入具有不同方向性的文本块。
- isolate：类似于 embed，但提供了更严格的隔离。除了方向性，isolate 还会阻止周围的文本影响嵌入文本的格式设置（如连字符的使用等）。这有助于确保嵌入的文本块在视觉上和格式上都与外部文本完全分离。
- bidi-override：这个值会覆盖 Unicode 双向算法，强制文本按照 HTML 或 CSS 中指定的方向渲染。例如，如果你在一个从右到左的环境中有一段应该是从左到右的文本，使用 bidi-override 可以确保这段文本按照你期望的方式显示。
- plaintext：这个值会使元素中的文本像无格式的纯文本一样进行双向算法处理。这主要用于模拟纯文本编辑器的行为。

在前端开发中，unicode-bidi 属性通常用于处理复杂的文本布局问题，特别是当页面需要同时显示多种语言和脚本时。通过正确使用这个属性，开发者可以确保文本在各种情况下都能正确、清晰地渲染。

#### [1.3 writing-mode](#)
writing-mode 属性定义了文本水平或垂直排布以及在块级元素中文本的行进方向。

此属性指定块流动方向，即块级容器堆叠的方向，以及行内内容在块级容器中的流动方向。因此，它也确定块级内容的顺序。

**值**：

- **horizontal-tb** 对于左对齐（ltr）文本，内容从左到右水平流动。对于右对齐（rtl）文本，内容从右到左水平流动。下一水平行位于上一行下方。
- **vertical-rl** 对于左对齐（ltr）文本，内容从上到下垂直流动，下一垂直行位于上一行左侧。对于右对齐（rtl）文本，内容从下到上垂直流动，下一垂直行位于上一行右侧。
- **vertical-lr** 对于左对齐（ltr）文本，内容从上到下垂直流动，下一垂直行位于上一行右侧。对于右对齐（rtl）文本，内容从下到上垂直流动，下一垂直行位于上一行左侧。

```html
<style>
.vertical-title{
    font-weight: 600;
    writing-mode: vertical-rl;
    color: #0077be;
    text-shadow: 2px 2px 1px rgba(0,0,0,0.1);
}
</style>

<p class="vertical-title">
  高级程序设计语言<br> JavaScript
</p>
```

#### [1.4 text-orientation](#)
text-orientation CSS 属性设定行中字符的方向。但它仅影响纵向模式（当 writing-mode 的值不是horizontal-tb）下的文本。此属性在控制使用竖排文字的语言的显示上很有作用，也可以用来构建垂直的表格头。

