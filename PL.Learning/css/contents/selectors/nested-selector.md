## [嵌入式选择符 Nested Selectors](#)
> **介绍**：simple

### [1. 嵌套选择器](#)

[CSS 嵌套](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_nesting)

```css
parentRule {
    /* 父规则样式属性 */
    & childRule {
        /* 子规则样式属性 */
    }
}
```
等同于：
```css
.parent-rule {
  /* 父规则的属性 */
}

.parent-rule .child-rule {
  /* 样式属性：是 .parent-rule 的子元素且具有 .child-rule 的元素 */
}
```