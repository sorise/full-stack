## [属性选择符 Attribute Selectors](#)
> **介绍**：simple

| 选择器              |  版本 |	Description 简介|
|:-----------------|:---|:----|
| `E[att]`         |CSS2	|选择具有att属性的E元素。|
| `E[att="val"]`   |	CSS2|	选择具有att属性且属性值等于val的E元素。|
| `E[att~="val"]`  |	CSS2|	选择具有att属性且属性值为一用空格分隔的字词列表，其中一个等于val的E元素。|
| `E[att^="val"]`  |	CSS3|	选择具有att属性且属性值为以val开头的字符串的E元素。|
| `E[att$="val"]`  |	CSS3|	选择具有att属性且属性值为以val结尾的字符串的E元素。|
| `E[att*="val"]`  |	CSS3|	选择具有att属性且属性值为包含val的字符串的E元素。|
| `E[att\|="val"]` |	CSS2|	选择具有att属性且属性值为以val开头并用连接符"-"分隔的字符串的E元素。|


[属性选择器 使用介绍](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Attribute_selectors)