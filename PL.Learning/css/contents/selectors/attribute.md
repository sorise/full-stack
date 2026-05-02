## [属性选择符 Attribute Selectors](#)
> **介绍**：CSS 属性选择器匹配那些具有特定属性或属性值的元素。

---
> 自 2015年7月 起，此特性已在主流浏览器中得到支持，可在大多数设备和浏览器版本中正常使用。

| 选择器              |  版本 |	Description 简介|
|:-----------------|:---|:----|
| `E[att]`        |CSS2|选择具有att属性的E元素。|
| `E[att="val"]`   |CSS2|选择具有att属性且属性值等于val的E元素。|
| `E[att~="val"]`  |CSS2|选择具有att属性且属性值为一用空格分隔的字词列表，其中一个等于val的E元素。|
| `E[att^="val"]`  |CSS3|选择具有att属性且属性值为以val开头的字符串的E元素。|
| `E[att$="val"]`  |	CSS3|	选择具有att属性且属性值为以val结尾的字符串的E元素。|
| `E[att*="val"]`  |	CSS3|	选择具有att属性且属性值为包含val的字符串的E元素。|
| `E[att\|="val"]` |	CSS2|	选择具有att属性且属性值为以val开头并用连接符"-"分隔的字符串的E元素。|


[属性选择器 使用介绍](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Attribute_selectors)

```css
/* 存在 title 属性的 <a> 元素 */
a[title] {
  color: purple;
}

/* 存在 href 属性并且属性值匹配"https://example.org"的 <a> 元素 */
a[href="https://example.org"] {
  color: green;
}

/* 存在 href 属性并且属性值包含"example"的 <a> 元素 */
a[href*="example"] {
  font-size: 2em;
}

/* 存在 href 属性并且属性值结尾是".org"的 <a> 元素 */
a[href$=".org"] {
  font-style: italic;
}

/* 存在 class 属性并且属性值包含单词"logo"的<a>元素 */
a[class~="logo"] {
  padding: 2px;
}
```