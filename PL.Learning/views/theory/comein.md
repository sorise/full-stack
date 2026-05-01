## [CSS：层叠样式表](#)
层叠样式表（Cascading Style Sheets，缩写为 CSS）是一种样式表语言，用来描述 HTML 或 XML（包括如 SVG、MathML 或 XHTML 之类的 XML 分支语言）文档的呈现方式。

---

### 1. CSS引入方式
CSS样式有三种引入方式：行内样式、内部样式\外部样式.

**行内样式**：也可以称为**内联样式** `(Inline Style)`，利用style全局属性，例如：为p标签添加一个CSS样式，让其中的字体改变颜色，标签上添加style属性，所有的CSS属性都要编写到style中。

语法：在元素标签内添加 style 属性，属性值为 CSS 声明（多个声明用分号分隔）。

```html
<p style="color: green">
  我是一段文本，我想请问你要干嘛，为什么要黑我们家鸽鸽
</p>
<p style="background-color: #f0f0f0; padding: 10px;">这是一个段落</p>
```

**注意**：优先级最高（覆盖其他样式）,但是不易维护（样式与内容混杂），不适用于多元素复用。

**内部样式** ：通过**style标签引入** 直接在HTML的head标签内部插入应该style标签。

```html
<html>
<head>
    <title>内部样式示例</title>
    <style>
        body {
            background-color: linen; /* 背景颜色 */
        }
        h1 {
            color: maroon; /* 标题颜色 */
            margin-left: 40px; /* 左边距 */
        }
        p {
            font-family: Arial, sans-serif; /* 字体 */
        }
    </style>
</head>
<body>
    <h1>欢迎使用内部样式</h1>
    <p>这是一个使用内部样式的段落。</p>
</body>
</html>
```

注意：不能跨页面复用，优先级低于行内样式。

**外部样式**：通过**link标签引入**，link标签一般放在head标签内部。

```html
<!DOCTYPE html>
<html>
<head>
    <title>外部样式示例</title>
    <link rel="stylesheet" type="text/css" href="styles.css"> 
    <!-- 链接外部CSS文件 -->
</head>
<body>
    <h1>外部样式标题</h1>
    <p>这是一个使用外部样式的段落。</p>
</body>
</html>
```

CSS 文件（如styles.css）：

```css
body {
    background-color: #ffffff;
}
h1 {
    color: green;
    text-align: center;
}
p {
    padding: 15px;
    border: 1px solid #ccc;
}
```

最佳可维护性（样式与内容完全分离），支持多页面复用，优先级最低（便于全局覆盖）,需要额外文件，加载速度略慢（需 HTTP 请求）。

### CSS的主要功能
1. **控制页面布局**:设置元素的位置、大小、边距、边框等。
2. **定义字体和颜色**:设置文本的字体、大小、颜色、背景色等。
3. **实现响应式设计**:通过媒体查询适配不同设备(如手机、平板、桌面)。
4. **动画与过渡效果**:实现元素的动画、渐变、变换等交互效果。
5. **样式继承和层叠**:多个样式规则可以叠加应用，优先级由层叠规则决定。