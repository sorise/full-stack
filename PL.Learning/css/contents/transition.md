## [Transition 过渡](#)
> **介绍**：过渡可以为一个元素在不同状态之间切换的时候定义不同的过渡效果。比如在不同的伪元素之间切换，像是 :hover，:active 或者通过 JavaScript 实现的状态变化。

-----

### [1. 属性大纲](#)


| 属性                                                                                     | CSS  | 继承性	 | 简介                       |
|:---------------------------------------------------------------------------------------|:---------------|:-----|:---------|
| [transition](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition)|CSS3|	无|	复合属性。检索或设置对象变换时的过渡效果|
| [transition-property](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-property)	|CSS3	|无|	检索或设置对象中的参与过渡的属性|
| [transition-duration](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-duration)    |CSS3	|无|	检索或设置对象过渡的持续时间|
| [transition-timing-function](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-timing-function)     |	CSS3|	无|	检索或设置对象中过渡的类型|
| [transition-delay](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-delay)	 |CSS3|	无|	检索或设置对象延迟过渡的时间|


```css
.ex{
    transition: margin-right 2s, color 1s;
}
```

- [过渡的基本设置【渡一教育】](https://www.bilibili.com/video/BV1tH4y1U7R3/?spm_id_from=333.788&vd_source=a03ca1a86c1e90990c4facd27ae17815)
