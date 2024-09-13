## [网格布局 grid](#)
> **介绍**：CSS 网格布局(Grid Layout) 是 CSS 中最强大的布局系统。 这是一个二维系统，这意味着它可以同时处理列和行。

-----

|属性|描述|
|:---|:---|
|column-gap	|指定列之间的间隙
|gap	|row-gap 和 column-gap 的简写属性|
|grid	|grid-template-rows, grid-template-columns, grid-template-areas, grid-auto-rows, grid-auto-columns, 以及 grid-auto-flow 的简写属性|
|grid-area	|指定网格元素的名称，或者也可以是 grid-row-start, grid-column-start, grid-row-end, 和 grid-column-end 的简写属性|
|grid-auto-columns|	指的默认的列尺寸|
|grid-auto-flow	|指定自动布局算法怎样运作，精确指定在网格中被自动布局的元素怎样排列。|
|grid-auto-rows	|指的默认的行尺寸|
|grid-column	|grid-column-start 和 grid-column-end 的简写属性|
|grid-column-end|	指定网格元素列的结束位置|
|grid-column-gap|	指定网格元素的间距大小|
|grid-column-start|	指定网格元素列的开始位置|
|grid-gap	|grid-row-gap 和 grid-column-gap 的简写属性|
|grid-row	|grid-row-start 和 grid-row-end 的简写属性|
|grid-row-end	|指定网格元素行的结束位置|
|grid-row-gap	|指定网格元素的行间距|
|grid-row-start	|指定网格元素行的开始位置|
|grid-template	|grid-template-rows, grid-template-columns 和 grid-areas 的简写属性|
|grid-template-areas|	指定如何显示行和列，使用命名的网格元素|
|grid-template-columns|	指定列的大小，以及网格布局中设置列的数量|
|grid-template-rows	|指定网格布局中行的大小|
|row-gap	|指定两个行之间的间距|

- [grid网格布局真香，比flex方便太多了](https://www.bilibili.com/video/BV1gw41167gh/?spm_id_from=333.337.search-card.all.click&vd_source=a03ca1a86c1e90990c4facd27ae17815)

```css
.container{
    display: grid;
    grid-template-columns: 33.33% 33.33% 33.33%;  /* 三列 */
    grid-template-rows: 50% 50%;  /* 两行 */
}
```
使用 repeat 统一设置值，第一个参数为重复数量，第二个参数是重复值

```css
.box {
    margin: 50px auto;
    display: grid;
    grid-template-rows: repeat(2, 50%);
    grid-template-columns: repeat(2, 50%);
    width: 300px;
    height: 200px;
    border: 5px solid #3333;
}
```