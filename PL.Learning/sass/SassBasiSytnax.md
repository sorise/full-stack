<a id="top" href="#top">Sass 基本语法 </a> 
----
**介绍** ：Sass语法及其简单,让我们来进入Sass的世界  注释 `/* */ 与 //` 都支持.

- [1. Sass 变量](#1-sass-变量)
- [x]  <a href="#SassQianTao">`Sass 嵌套`</a>
  - <a href="#CengciProperty" >`sass 层次嵌套`</a>
  - <a href="#Property" >`sass 属性嵌套`</a>
- [x] <a href="#PartialSass">`@import scss `</a>
- [x] <a href="#mixinSupport">`供应商前缀 mixin `</a>
- [x] <a href="#kuozhanhejicheng">`继承 @extend`</a>



### [1. Sass 变量和嵌套](#)
Sass 变量可以存储颜色，字体堆栈或您认为要重用的任何CSS值等内容 Sass 变量必须以`$`开头。**如果变量需要嵌套到字符串之中**，就必须写在 `#{}`中. 
* Sass 变量可以参与运算 `+-*/ ()`都可以`。
* Sass 变量以 `;` 结尾。
* Sass 赋值的方法是使用`:` 左边是变量名称--右边是值。
* 对于定义在全局的变量文件每一个地方都可以访问,如果变量定义在 `{}`那么它 的作用域就在 `{}` 内部 也就是说变量具有作用域.

```scss
  $color-red:"red";
  $width: 500px;
  $nav-outter-width:200px;
  $nav-inner-width: $nav-outter-width*0.8;
  
  .nav-outter{
      width: $nav-outter-width;
  }
  .nav-inner{
      width: $nav-inner-width;
  }
  .nav-bar{
      width: $width;
      border:"1px solid #{$width}" /*使用变量*/
  }
  .font-color{
      color:$color_red;
  }
```
**编译结果:**
```css
.nav-outter {
  width: 200px;
}

.nav-inner {
  width: 140px;
}

.nav-bar {
  width: 500px;
  border: "1px solid 500px";
}

.font-color {
  color: "red";
}
```
**运算：有注意的地方**
* 在乘除运算中, 如果两个变量运算都带上单位,如此则不能够参加乘除运算:建议对于参与运算的两个值或者多个值,其中一个带上单位其他不带单位。
* ruby底层在处理运算的时候由需要符号,来确定最后的值的单位是什么
* 运算支持 `()`
* sass还支持这样的非变量运算: `height: 70px+40px;`

```scss
$zero-up : 20px;
$zero-down : 40px;

.nav-height{
    width:40px+90px;
    height: $zero-down+$zero-up; //支持
}

.nav-title{
    height: $zero-down * $zero-up; //不支持 可以把两个变量定义成
}

/*
00px*px isn't a valid CSS value.
        on line 9 of sass/m:\vscode-workspace\Sass\Arithmatic.scss
>>     height: $zero-down * $zero-up;*/
```
修改方式1: 可以变量都不带单位最后加上一个值为0 的带单位数。
```scss
$zero-up : 20;
$zero-down : 40;

.nav-height{
    height: ($zero-down+$zero-up)+0px ;
}

.nav-title{
    height: ($zero-down * $zero-up) +0px;
}
```
修改方式2: 这种方式修改更加简洁。
```scss
$zero-up : 20;
$zero-down : 40px;

.nav-height{
    height: $zero-down+$zero-up;
}

.nav-title{
    height: $zero-down * $zero-up;
}
```
#### 1.1 Sass 嵌套
sass 支持嵌套以,减少css的工作量,请看下面的。**当然 css也已经支持嵌套**。
```scss
nav{
    ul {
        width: 200px;
        height:50px;
        font-size: 14px;
        line-height: 18px;
    }  
    div{
        padding: 5px;
        a{
            text-decoration: none;
            padding: 5px;
        }
    } 
    
    .full-left{
        float: left;
        padding-left: 5px;   
    } 
    .full-right{
        float: right;
        padding-right: 5px;
    }

    margin: 5px;
    padding: 5px;
}
```
**编译结果:** 它具有清晰的嵌套和可视层次结构，scss的层次结构更加的清晰。
```css
nav {
  margin: 5px;
  padding: 5px;
}

nav ul {
  width: 200px;
  height: 50px;
  font-size: 14px;
  line-height: 18px;
}

nav div {
  padding: 5px;
}

nav div a {
  text-decoration: none;
  padding: 5px;
}

nav .full-left {
  float: left;
  padding-left: 5px;
}

nav .full-right {
  float: right;
  padding-right: 5px;
}
```

### [2. CSS变量与嵌套](#)
CSS 变量，正式名称为 CSS 自定义属性（CSS Custom Properties） ，是现代 Web 开发中一项革命性的特性。它让你能够像在其他编程语言中一样，在 CSS 中定义可复用的值，从而极大地提升了样式的可维护性、灵活性和动态性。


CSS 变量的核心思想非常简单：在一个地方定义一个值，然后在整个样式表中多次使用它。

- **定义**: 使用 `--` 前缀来声明一个变量，例如 `--main-color`: `#3498db;`。
- **使用**: 通过 `var()` 函数来引用变量的值，例如 `color: var(--main-color);`。

```css
:root {
    --bg-color: #ffffff;
}

p {
    background-color: var(--bg-color);
}
```

与 Sass 或 Less 等预处理器的变量不同，CSS 变量是原生的，这意味着它们在浏览器中直接运行，**并且可以被 JavaScript 动态修改**，实现真正的运行时交互。

```javascript
const button = document.querySelector('button');

button.addEventListener('click', () => {
  document.documentElement.style.setProperty('--bg-color', 'red');
});
```

CSS变量(root或元素级定义)，JavaScript可通过以下方法操作：
- getComputedStyle()：获取计算后的样式值
    - 获取变量值：`getComputedStyle(element).getPropertyValue('--var-name')`
- getPropertyValue()：读取特定CSS变量值
- setProperty()：修改CSS变量值
    - 设置变量值：`element.style.setProperty('--var-name', 'new-value')`

```css
:root {
  --primary-color: #4a6fa5;
  --secondary-color: #166088;
  --text-color: #333;
  --bg-color: #f8f9fa;
}

.demo-box {
  background-color: var(--primary-color);
  transition: all 0.3s ease;
}

/* 其他样式... */
```
- `document.documentElement` 直接获取了<html>元素，这是我们声明全局变量的地方。
- `setProperty` 是修改样式的标准方法，第一个参数是变量名（必须包含两个减号），第二个参数是新值。
- `getComputedStyle(element)` 会获取元素最终计算出的所有样式（包括来自样式表、内联样式和CSS变量的），这是一个强大的只读方法。

```javascript
document.addEventListener('DOMContentLoaded', function() {
  // 获取CSS变量
  function getCSSVariables() {
    const root = document.documentElement;
    const styles = getComputedStyle(root);
    return {
      '--primary-color': styles.getPropertyValue('--primary-color').trim(),
      '--secondary-color': styles.getPropertyValue('--secondary-color').trim()
    };
  }

  // 显示变量值
  function displayVariables() {
    const vars = getCSSVariables();
    document.getElementById('varOutput').textContent = 
      `--primary-color: ${vars['--primary-color']}
--secondary-color: ${vars['--secondary-color']}`;
  }

  // 切换主题
  function toggleTheme() {
    const root = document.documentElement;
    const current = getComputedStyle(root).getPropertyValue('--primary-color').trim();
    if(current === '#4a6fa5') {
      root.style.setProperty('--primary-color', '#d64045');
      root.style.setProperty('--secondary-color', '#9e2a2b');
    } else {
      root.style.setProperty('--primary-color', '#4a6fa5');
      root.style.setProperty('--secondary-color', '#166088');
    }
    displayVariables();
  }

  // 事件监听
  document.getElementById('getBtn').addEventListener('click', displayVariables);
  document.getElementById('changeBtn').addEventListener('click', toggleTheme);
  displayVariables();
});
```


### [3. import Sass](#)
* `您可以创建包含很少CSS片段的部分Sass文件，您可以将这些片段包含在其他Sass文件中。这是模块化CSS并帮助保持易于维护的好方法。partial只是一个以前导下划线命名的Sass文件。你可能会说出类似的东西_partial.scss。下划线让Sass知道该文件只是一个部分文件，不应该生成CSS文件。Sass partials与@import指令一起使用。`
* `对于导入的文件 scss文件可以只写文件名`：`@import nav` `全名称：@import nav.scss ` `对于导入的css文件要加上文件后缀名`
* `@import "http://foo.com/bar";`
* `@import url(foo);`
-----
* `nav.scss`
```scss
body{
    a{
        text-decoration: none;
    }
}
```
* `link.scss`
```scss
nav{
    ul {
        width: 200px;
        height:50px;
        font-size: 14px;
        line-height: 18px;
    }  
}
```
* `lab.scss`
```scss
@import "nav";
@import "link";

$height:200px;

.lab-div-info{
    width: $height;
    .funky {
        font: {
          family: fantasy;
          size: 30em;
        }
      }
}
```
**`编译结果:`**
```css
nav ul {
  width: 200px;
  height: 50px;
  font-size: 14px;
  line-height: 18px;
}

body a {
  text-decoration: none;
}

.lab-div-info {
  width: 200px;
}

.lab-div-info .funky {
  font-family: fantasy;
  font-size: 30em;
}

```
#####  <a id="mixinSupport" href="#mixinSupport">供应商前缀 mixin</a>  :star2: <a href="#top"> :arrow_up: </a>
* `mixin`:`类似于宏定义的复用 类似于javascript 中的function 定义`
* `然后 的名称类似于函数吗,然后是参数 {} 内部的属性值就是返回的所有内容`
* `使用` `@include` `调用函数名称 传入方法`
* `其实` `@mixin` `@include` `可以和js函数比较理解`
```scss
@mixin transform($property) {
    -webkit-transform: $property;
        -ms-transform: $property;
            transform: $property;
  }
 
@mixin font-align($align){
    text-align: $align;
}  

.box { 
    @include transform(rotate(30deg)); 
    @include font-align(center);
}
```
**`编译结果:`**
```css
.box {
  -webkit-transform: rotate(30deg);
  -ms-transform: rotate(30deg);
  transform: rotate(30deg);
  text-align: center;
}
```
####  <a id="kuozhanhejicheng" href="#kuozhanhejicheng">继承 @extend</a>  :star2: <a href="#top"> :arrow_up: </a>
`在设计网页的时候常常遇到这种情况：一个元素使用的样式与另一个元素完全相同，但又添加了额外的样式，好一点的设计还会分成两个css类一个通用类一个特殊类,麻烦的是，这样做必须时刻记住使用 .seriousError 时需要参考 .error 的样式，带来了很多不变,sass 引入了基础的概念`
```css
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError {
  border-width: 3px;
}
//html
<div class="error seriousError">
  Oh no! You've been hacked!
</div>
```
**`基础 @extend`**
```scss
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```
`编译结果:`
```css
.error, .seriousError {
  border: 1px #f00;
  background-color: #fdd;
}

.seriousError {
  border-width: 3px;
}
```






