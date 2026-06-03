## [CSS 尺寸](#)
> **介绍** : 设置长度、宽度、及其限制值。


### [1. 属性大纲](#)
有如下的css属性：

| 属性类别 | 属性名称 | 功能描述 | 常用取值/备注 |
| :--- | :--- | :--- | :--- |
| 控制调整 | `resize` |	设置元素是否可调整尺寸，以及可调整的方向 | `none、both、horizontal、vertical` |
| 基础宽高 | `width` | 设置盒子的宽度 | `px`, `%`, `vw`, `auto` (默认) 等 |
| 基础宽高 | `height` | 设置盒子的高度 | `px`, `%`, `vh`, `auto` (默认) 等 |
| 限制范围 | `min-width` | 设置盒子的最小宽度 | 防止盒子过窄 |
| 限制范围 | `max-width` | 设置盒子的最大宽度 | 防止盒子过宽，常用于响应式布局 |
| 限制范围 | `min-height` | 设置盒子的最小高度 | 防止盒子过矮 |
| 限制范围 | `max-height` | 设置盒子的最大高度 | 防止盒子过高，常配合 overflow 使用 |
| 盒模型计算 | `box-sizing` | 控制盒子尺寸的计算规则 | `content-box` (默认，宽高仅含内容)`border-box` (宽高含内容+padding+border) |
|逻辑属性|`inline-size`|根据书写模式定义了元素区块横向或纵向尺寸|`px`, `%`, `vw`, `auto` (默认) 等|
|逻辑属性|`block-size`|根据书写模式定义了元素区块横向或纵向尺寸|`px`, `%`, `vw`, `auto` (默认) 等|


#### [1.1 resize 属性](#)
resize 是 CSS 中用来控制元素是否可以被用户调整大小的属性，主要应用在具有滚动条的可滚动容器（如 `<textarea>`、`<div>`）上。

属性值
* **none**: 禁止用户调整元素大小。这是默认值。
* **both**: 允许用户在水平和垂直方向上调整元素大小。
* **horizontal**: 只允许用户在水平方向上调整元素的宽度。
* **vertical**: 只允许用户在垂直方向上调整元素的高度。
* **block**: 允许用户根据元素的块方向调整大小。这在不同书写模式下会有所不同。
* **inline**: 允许用户根据元素的内联方向调整大小。这在不同书写模式下会有所不同。

resize 属性通常与 overflow 属性结合使用。例如，当 overflow 被设置为 hidden 时，resize 不会生效，因为没有滚动条。
```html
<style>
    .resizable {
        resize: both;
        overflow: scroll;
        border: 1px solid black;
    }
    div {
        height: 300px;
        width: 300px;
    }

    p {
        height: 200px;
        width: 200px;
    }
</style>

<textarea style="resize: both; width: 200px; height: 100px;">
  可以自由调整大小的文本区域
</textarea>

<div class="resizable">
    <p class="resizable">
        此段落可在各个方向上调整尺寸，这是因为在此元素上 CSS `resize` 属性设置为
        `both`。
    </p>
</div>
```

min-width, max-width, min-height, max-height: 这些属性可以用来限制元素的最小和最大尺寸，避免用户调整元素大小时超出预期的范围。

#### [1.2 其他属性的值](#)
* 无特定值，取决于其它属性值
* `<length>`： 用长度值来定义宽度。不允许负值
* `<percentage>`： 用百分比来定义宽度。百分比参照包含块宽度。不允许负值

```css
.physical {
  width: 200px;
  height: 100px;
}
```


