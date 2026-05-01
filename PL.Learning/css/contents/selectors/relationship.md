## [关系选择符 Relationship Selectors](#)
> **介绍**: 我们接着来看更加强大的组合选择器，它可以实现对基本选择器的任意组合，这针对于一些特定环境会非常好使, [关系选择器 使用介绍](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Combinators) 。

| 选择器|名称|版本|Description 简介|
|:----|:--------------------------------------|:---|:----|
|`E F`|	包含选择符(Descendant combinator)	|CSS1|	选择所有被E元素包含的F元素。|
|`E>F`|	子选择符(Child combinator)	|CSS2	|选择所有作为E元素的子元素F。|
|`E+F`|	相邻选择符(Adjacent sibling combinator)	|CSS2|	选择紧贴在E元素之后F元素。|
|`E~F`|	兄弟选择符(General sibling combinator)	|CSS3|	选择E元素所有兄弟元素F。|



### 包含选择器
也称为后代组合器（通常用单个空格（" "）字符表示）组合了两个选择器，如果第二个选择器匹配的元素具有与第一个选择器匹配的祖先（父母、父母的父母，父母的父母的父母等）元素，则它们将被选择。利用后代组合器的选择器称为后代选择器。

```css
/* 列出“my-things”列表的子项 */
ul.my-things li {
  margin: 2em;
}
```

### 子选择符
被放在两个 CSS 选择器之间。它只匹配那些被第二个选择器匹配的元素，这些元素是被第一个选择器匹配的元素的直接子元素。

```css
/* 选择属于“my-things”类的无序列表（ul）的直接子列表元素（li） */
ul.my-things > li {
  margin: 2em;
}
```

被第二个选择器匹配的元素必须是被第一个选择器匹配的元素的直接子元素。这比后代组合器更严格，后者匹配所有被第二个选择器匹配的元素，这些元素存在被第一个选择器匹配的祖先元素，无论在 DOM 上有多少“跳”。

### 相邻选择符
接续兄弟选择器（+）介于两个选择器之间，当第二个元素紧跟在第一个元素之后，并且两个元素都是属于同一个父元素的子元素，则第二个元素将被选中。

```css
/* 图片后面紧跟着的段落将被选中 */
img + p {
  font-weight: bold;
}

li:first-of-type + li {
  color: red;
}
```

```html
<ul>
  <li>One</li>
  <li>Two!</li>
  <li>Three</li>
</ul>
```

### 后续兄弟选择器
后续兄弟选择器（~）将两个选择器分开，并匹配第二个选择器的所有迭代元素，位置无须紧邻于第一个元素，只须有相同的父级元素。

```css
/* 在任意图像后的兄弟段落 */
img ~ p {
  color: red;
}
```