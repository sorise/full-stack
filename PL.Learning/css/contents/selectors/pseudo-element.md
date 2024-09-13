## [伪对象选择符 Pseudo-Element Selectors](#)
> **介绍**：simple

### [1. 伪对象选择器](#)

| 选择器                   | 版本                                                | 	Description 简介                           |
|:-----------------------|:--------------------------------------|:------------------------------------------|
|E:first-letter/E::first-letter|	CSS1/3| 	设置对象内的第一个字符的样式。                          |
|E:first-line/E::first-line|	CSS1/3| 	设置对象内的第一行的样式。                            |
|E:before/E::before|	CSS2/3| 	设置在对象前（依据对象树的逻辑结构）发生的内容。用来和content属性一起使用 |
|E:after/E::after|	CSS2/3| 	设置在对象后（依据对象树的逻辑结构）发生的内容。用来和content属性一起使用 |
|E::selection|	CSS3| 	设置对象被选择时的颜色。                             |
|E::placeholder|	CSS3| 	 表示 `<input>` 或 `<textarea>` 元素中的占位文本。。      |