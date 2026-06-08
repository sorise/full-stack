## [CSS Media Queris 媒体查询](#)

> **媒体查询**（Media Queries） 是 CSS3 中引入的一项功能，它允许我们根据设备的特性（如屏幕宽度、高度、分辨率、方向等）来应用不同的样式规则。


### [1. 基本概念](#)
媒体查询就是根据不同设备的显示条件，展示不同的样式，提升移动端和桌面端用户的体验，动态适应浏览器窗口大小变化。
```css
@media media-type and (media-feature-rule) {
  /* CSS rules go here */
}
/* 翻译一下就是：
  @media [媒体类型] [逻辑操作符] ([媒体特征表达式]) {
      当条件满足时，这里面的样式才会生效 
  }
*/
```

常见媒体类型（media type）：

|类型|	描述|
|:----|:----|
|`all`	|所有设备（默认值）|
|`screen`|	屏幕设备（如电脑、手机）|
|`print`	|打印设备|
|`speech`|	屏幕阅读器等“语音合成器”|

常见媒体特征（Media Features）

|特征名|	描述|
|:----|:----|
|`width/height`|视口的宽度/高度|
|`min-width`/`max-width`|最小/最大视口宽度（超过多宽、不超过多宽）|
|`orientation`|设备方向（portrait 竖屏 / landscape 横屏）|
|`aspect-ratio`|宽高比（如 16/9）|
|`resolution`|分辨率（如 2dppx 表示高清屏）device-width设备物理宽度（`不推荐使用`）|

#### 1.1 媒体查询的使用方式

在 `<style>` 标签中使用（内联样式）
```html
<style>
  @media (max-width: 600px) {
      .facet_sidebar {
          display: none;
      }
  }
</style>
```

通过 `<link>` 引入外部样式表（分离样式）
```html
<link rel="stylesheet" media="(max-width: 800px)" href="mobile.css">
<link rel="stylesheet" media="(min-width: 801px)" href="desktop.css">
```

组合多个媒体特征

```css
@media screen and (min-width: 768px) and (max-width: 1024px) {
    /* 平板设备样式 */
}
```

#### 1.2 典型断点设置（Breakpoints）

|设备类型|推荐断点|
|:----|:----|
|范围移动端（竖屏）|max-width: 576px|
|移动端（横屏）|min-width: 576px 和 max-width: 768px|
|平板|min-width: 768px 和 max-width: 1024px|
|小型桌面|min-width: 1024px 和 max-width: 1200px|
|大型桌面|min-width: 1200px|


```css
/* 移动端样式 */
@media (max-width: 767px) {
    ...
}

/* 平板样式 */
@media (min-width: 768px) and (max-width: 1024px) {
    ...
}

/* 桌面样式 */
@media (min-width: 1025px) {
    ...
}

/* and: 视口宽度在600px到900px之间 */
@media screen and (min-width: 600px) and (max-width: 900px) {
  /* ... */
}

/* not: 非打印设备 */
@media not print {
  /* ... */
}

/* , (or): 视口宽度小于600px或是竖屏设备 */
@media (max-width: 600px), (orientation: portrait) {
  /* ... */
}

```

#### 1.3 not
使用 not 否定查询条件

```css
@media not print and (color) {
    /* 不是打印设备，并且支持颜色的设备才应用此样式 */
}
```

#### 1.4 only
使用 only 避免旧版浏览器误读,only 的作用是防止老旧浏览器加载响应式样式。

```html
<link rel="stylesheet" media="only screen and (max-width: 600px)" href="mobile.css">
```

### [2. 媒体特性（Media Features）](#)
媒体特性是媒体查询中功能最强大的部分，它允许我们检测设备的具体参数。


#### 2.1尺寸相关特性
这是最常用的一类特性，主要用于根据视口或设备的尺寸来调整布局。

- `width / height`: 描述视口的宽度和高度。可以与 `min-` 和 `max-` 前缀结合使用，例如 `min-width、max-height`。
- `aspect-ratio`: 描述视口的宽高比，例如 16/9。
- `orientation`: 描述设备的方向，值为 `portrait`（竖屏）或 `landscape`（横屏）。

#### 2.2 显示相关特性
这类特性与设备的显示能力有关。

- `resolution`: 描述输出设备的分辨率（像素密度），单位通常是 dpi (dots per inch) 或 dppx (dots per pixel unit)。
- `color`: 描述输出设备颜色分量的位深度。例如 (min-color: 8) 表示设备至少支持8位颜色。
- `color-gamut`: 描述设备支持的色域范围。

#### 2.3 交互相关特性
这类特性用于检测用户的输入设备能力。

- `hover`: 检测主输入设备是否支持悬停操作。值为 hover 或 none。
- `pointer`: 检测主输入设备是否为指针设备（如鼠标、触摸笔）。


### [3. 逻辑操作符](#)
逻辑操作符用于创建更复杂的媒体查询，组合多个条件。

- `and`: 用于将多个媒体特性组合在一起，所有条件都必须为真时，查询才为真。
- `not`: 用于否定一个媒体查询。它必须放在查询的开头，并且必须指定媒体类型。
- `,` (逗号): 相当于逻辑 “或”，用于将多个独立的媒体查询组合在一起，只要其中任何一个为真，样式就会被应用。
- `only`: 用于防止老旧的、不支持媒体特性的浏览器错误地应用样式。在现代浏览器中，only 关键字没有实际作用。

```css
/* and: 视口宽度在600px到900px之间 */
@media screen and (min-width: 600px) and (max-width: 900px) {
  /* ... */
}

/* not: 非打印设备 */
@media not print {
  /* ... */
}

/* , (or): 视口宽度小于600px或是竖屏设备 */
@media (max-width: 600px), (orientation: portrait) {
  /* ... */
}
```

### [4. 最佳实践与注意事项](#)
在使用媒体查询时，遵循一些最佳实践可以让您的代码更加高效和可维护。

#### [4.1 移动优先策略](#)
采用"移动优先"的设计方法，先为最小的屏幕设计样式，然后使用 min-width 媒体查询逐步增强：

```css
/* 移动设备样式（默认） */
.container {
  width: 100%;
  padding: 10px;
}

/* 平板及以上 */
@media (min-width: 768px) {
  .container {
    width: 90%;
    padding: 20px;
  }
}

/* 桌面及以上 */
@media (min-width: 1024px) {
  .container {
    width: 80%;
    max-width: 1200px;
    margin: 0 auto;
  }
}
```

#### [4.2 使用相对单位](#)
在媒体查询中使用 `em` 或 `rem` 单位而不是 `px`，这样可以更好地适应用户的字体大小设置：

```css
/* 使用 em 单位，1em ≈ 16px */
@media (min-width: 48em) { /* 768px */
  .sidebar {
    width: 25%;
  }
}
```