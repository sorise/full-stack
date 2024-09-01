## [CSS 语法规则](#)
> **介绍** : CSS（Cascading Style Sheets，层叠样式表）是一种用于描述HTML或XML文档如何呈现的样式表语言。CSS语法由选择器、属性和值等部分组成，通过这些规则定义网页元素的外观。以下是CSS语法的详细介绍。

### [1. 语法入门](#)
和 HTML 类似，CSS 也不是真正的编程语言，甚至不是标记语言,CSS 是一门样式表语言，这也就是说人们可以用它来选择性地为 HTML 元素添加样式。

举例来说，以下 CSS 代码选择了所有的段落文字，并将它们设置为红色。
```css
p{
    color: red;
}
```
整个结构称为规则集（规则集通常简称规则），注意各个部分的名称：
* **选择器（Selector）** HTML 元素的名称位于规则集开始。它选择了一个或多个需要添加样式的元素（在这个例子中就是 <p> 元素）。要给不同元素添加样式，只需要更改选择器。
* **声明（Declaration）** 一个单独的规则，如 color: red; 用来指定添加样式元素的属性。
* **属性（Properties）** 改变 HTML 元素样式的途径（本例中 color 就是 <p> 元素的属性）。CSS 中，由编写人员决定修改哪个属性以改变规则。
* **属性的值（Property value）** 在属性的右边，冒号后面即属性的值，它从指定属性的众多外观中选择一个值（我们除了 red 之外还有很多属性值可以用于 color ）。

注意其他重要的语法：
* 除了选择器部分，每个规则集都应该包含在成对的大括号里（{}）。
* 在每个声明里要用冒号（:）将属性与属性值分隔开。
* 在每个规则集里要用分号（;）将各个声明分隔开。

#### [1.1 选择多个元素](#)
也可以选择多种类型的元素并为它们添加一组相同的样式。将不同的选择器用**逗号**（,）分开。例如：

```css
p, li, h1 {
  color: red;
}
```

#### [1.2 不同类型的选择器](#)
选择器有许多不同的类型。上面只介绍了元素选择器，用来选择 HTML 文档中给定的元素。但是选择操作可以更加具体。下面是一些常用的选择器类型：


|选择器名称| 选择的内容                                               | 示例                                                           |
|:---|:----------------------------------------------------|:-------------------------------------------------------------|
|元素选择器（也称作标签或类型选择器）| 所有指定类型的 HTML 元素                                     | 	p 选择 `<p>`, div 选择 `<div>`                                  |
|ID 选择器| 	具有特定 ID 的元素。单一 HTML 页面中，每个 ID 只对应一个元素，一个元素只对应一个 ID | 	#my-id 选择 `<p id="my-id">` 或 `<a id="my-id">`               |
|类选择器| 	具有特定类的元素。单一页面中，一个类可以有多个实例                          | .my-class 选择 `<p class="my-class">` 和 `<a class="my-class">` |
|属性选择器| 拥有特定属性的元素                                           | img[src] 选择 `<img src="myimage.png">` 但不是 `<img>`            |
|伪类选择器| 特定状态下的特定元素（比如鼠标指针悬停于链接之上）                           | a:hover 选择仅在鼠标指针悬停在链接上时的 `<a>` 元素                            |
|`*`|  通配选择器| 它可以匹配任意类型的 HTML 元素。                                          |
|& 嵌套选择器 | 明确指示在使用 CSS 嵌套时，父规则和子规则的关系 | **2023.10 新特性**                                              |
|组合选择器|多个选择器可以组合起来使用。| div p（选择所有位于`<div>`内的`<p>`元素）                                    |

#### [1.3 注释](#)
CSS中使用注释可以帮助开发者理解代码或暂时屏蔽某些代码。注释不会被浏览器渲染。

```css
/* 这是一个注释 */
p {
  color: red; /* 为段落设置红色 */
}
```

#### [1.4 层叠与优先级](#)
CSS中的“层叠”指的是当多个规则应用于同一个元素时，确定哪些样式生效的机制。样式优先级由以下几部分决定：

**优先级（Specificity）**：
* 内联样式（style属性内的样式）优先级最高。
* ID选择器比类选择器和元素选择器的优先级更高。
* 类选择器比元素选择器的优先级更高。
* 通配符选择器（*）优先级最低。

**层叠顺序**：当多个规则具有相同的优先级时，最后声明的样式（即靠近文档底部的样式）会覆盖前面的样式。

**!important** 声明：可以强制提高样式的优先级，覆盖其他所有样式，但应谨慎使用。

```css
p {
  color: red !important;
}
```

#### [1.5  CSS变量](#)
CSS变量，也称为自定义属性，是CSS中的一种强大的特性，可以提高样式表的灵活性和可维护性。
```css
:root {
  --main-color: #3498db;
  --padding: 10px;
}

h1 {
  color: var(--main-color);
  padding: var(--padding);
}
```
:root选择器通常用于定义全局变量，var(--variable-name)用于引用变量。


#### [1.6 CSS中的继承](#)
某些CSS属性是可继承的，这意味着子元素会自动继承父元素的这些属性的值。典型的可继承属性包括字体属性、颜色、文本对齐等。

```css
body {
  color: black;
}
p {
  font-size: 14px;
}
```

#### [1.7  CSS模块化](#)
导入CSS文件：可以使用@import规则将多个CSS文件导入到一个文件中，实现样式的模块化管理。

```css
@import url('styles.css');
```
如Sass、Less等工具，可以帮助编写更模块化、易维护的CSS代码。
### [2. 媒体查询](#)
媒体查询使得网页能够根据设备的不同特性（如屏幕大小、分辨率）应用不同的CSS规则，支持响应式设计。

```css
@media screen and (max-width: 600px) {
  body {
    background-color: lightblue;
  }
}
```
在屏幕宽度小于600像素时，body元素的背景颜色会变为浅蓝色。


### [3. CSS 变量](#)
CSS变量，也称为自定义属性，是CSS中的一种强大的特性，可以提高样式表的灵活性和可维护性。以下是对CSS变量的详细解释：

#### [3.1 CSS变量的定义](#)
CSS变量使用 `--` 作为前缀定义，并且通常定义在 `:root` 选择器下，以便全局使用。

语法:
```css
:root {
  --variable-name: value;
}
```
**例子** : 以上代码定义了两个CSS变量：--main-color和--padding。
```css
:root {
  --main-color: #3498db;
  --padding: 10px;
}
```

#### [3.2 使用CSS变量](#)
CSS变量通过var()函数引用，语法为：
```css
property: var(--variable-name);
```
**例子** : 在这个例子中，body元素的背景颜色和内边距使用了我们定义的--main-color和--padding变量。
```css
body {
  background-color: var(--main-color);
  padding: var(--padding);
}
```

#### [3.3 局部变量](#)
CSS变量不仅可以在:root定义为全局变量，还可以在特定选择器中定义局部变量。局部变量仅在其定义的选择器内有效。

这个例子中，--box-color变量仅在.box类选择器中可用。
```css
.box {
  --box-color: #ff5733;
  background-color: var(--box-color);
}
```

#### 3.4 结合使用
响应式设计：结合媒体查询，CSS变量可以根据不同的屏幕尺寸调整样式。

在这个例子中，字体大小根据屏幕宽度进行调整。
```css
:root {
  --font-size: 16px;
}
@media (max-width: 600px) {
  :root {
    --font-size: 14px;
  }
}
p {
  font-size: var(--font-size);
}
```

#### 3.5 CSS变量的回退值
当引用的变量未定义或无效时，可以提供一个回退值。回退值用作var()函数的第二个参数。

```css
property: var(--variable-name, fallback-value);
```

**例子** : 如果--undefined-color未定义，则文本颜色将回退为黑色。
```css
color: var(--undefined-color, black);
```

#### 3.6 变量与JavaScript结合
CSS变量可以通过JavaScript动态更新。例如，通过修改 document.documentElement.style.setProperty可以改变全局变量的值。

```javascript
document.documentElement.style.setProperty('--main-color', '#e74c3c');
//这段代码会将--main-color变量的值更改为#e74c3c。
```

#### 3.7 嵌套和组合
CSS变量可以与其他CSS功能组合使用，包括嵌套、计算等。

```css
:root {
  --base-size: 10px;
  --large-size: calc(var(--base-size) * 2);
}
h1 {
  font-size: var(--large-size);
}
```
在这个例子中，--large-size是基于--base-size的计算结果。