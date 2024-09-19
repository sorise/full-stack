### [CSS 表格样式](#)
> **介绍** 设置表格的样式。


### [1. 属性大纲](#) 

| 属性                                                                             | 版本	   |继承性| 简介                                                          |
|:-------------------------------------------------------------------------------|:------|:------|:------------------------------------------------------------|
| [table-layout](https://developer.mozilla.org/zh-CN/docs/Web/CSS/table-layout)	 | CSS2  |	无	| 设置或检索表格的布局算法                                                |
| [border-collapse](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-collapse)  | 	CSS2 |	有| 属性是用来决定表格的边框是分开的还是合并的。                                      |
| [border-spacing](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-spacing) | CSS2	 |有| 	设置或检索当表格边框独立时，行和单元格的边框在横向和纵向上的间距                           |
| [caption-side](https://developer.mozilla.org/zh-CN/docs/Web/CSS/caption-side) | CSS2	 |有	| 定义表格标题的位置。通常用于caption元素。                                    |
| [empty-cells](https://developer.mozilla.org/zh-CN/docs/Web/CSS/empty-cells) | CSS2	 |有	| 设置或检索当表格的单元格无内容时，是否显示该单元格的边框(border-collapse: separate;才管用) |
| [vertical-align](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align) | CSS3	 |有	| 用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式。                |

### [2. table-layout](#)
table-layout CSS 属性定义了用于布局表格的单元格、行和列的算法。

**取值**
* **auto** : 默认情况下，大多数浏览器使用自动表格布局算法。表格及其单元格的宽度会根据内容自动调整大小。
* **fixed** : 表格和列的宽度是由 table 和 col 元素的宽度或第一行单元格的宽度来设置的。后续行中的单元格不会影响列的宽度。

### [3. caption-side](#)
CSS 中 caption-side 属性会将表格的标题（`<caption>`）放到规定的位置。但是具体显示的位置与表格的 writing-mode 属性值有关。

**取值:**
* top 标题盒应置于表格上方。
* bottom 标题盒应置于表格下方。
* block-start 标题盒应置于表格的块首一侧。
* block-end 标题盒应置于表格的块末一侧。
* inline-start 标题盒应置于表格的行首一侧。
* inline-end 标题盒应置于表格的行末一侧。

```
/* 方向值 */
caption-side: top;
caption-side: bottom;

/* 逻辑值 */
caption-side: block-start;
caption-side: block-end;
caption-side: inline-start;
caption-side: inline-end;

/* 全局值 */
caption-side: inherit;
caption-side: initial;
caption-side: revert;
caption-side: revert-layer;
caption-side: unset;
```

### [4. border-spacing](#)
border-spacing 属性指定相邻单元格边框之间的距离（只适用于 边框分离模式 ）。相当于 HTML 中的 cellspacing 属性，但是第二个可选的值可以用来设置不同于水平间距的垂直间距。

```
table {
  border-spacing: 10px 5px;
}
```
