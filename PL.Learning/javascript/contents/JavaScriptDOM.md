## [JavaScript BOM](#)
> **介绍**：浏览器对象模型（BOM，Browser Object Model）为是使用 JavaScript 开发 Web 应用程序的核心，主要有window对象、location对象、navigator对象、history对象。

---

- [1. window 对象](#)
- 

---

### [1.window 对象](#)
**BOM 的核心是 window 对象，表示浏览器的实例**。浏览器里面，**window**对象（注意，w为小写）指当前的浏览器窗口。它也是当前页面的**顶层对象**，即最高一层的对象，所有其他对象都是它的下属。一个变量如果未声明，那么默认就是顶层对象的属性。

window 对象在浏览器中**有两重身份**，
* 一个是ECMAScript 中的 **Global** 对象: 这意味着网页中定义的所有对象、变量和函数都以 window 作为其 Global 对象，都可以访问其上定义的 parseInt()等全局方法。
* 一个是浏览器窗口的 **JavaScript** 接口。

**Global 作用域**：因为 window 对象被复用为 ECMAScript 的 Global 对象，所以通过 var 声明的所有全局变量和函 数都会变成 window 对象的属性和方法。

```javascript
user_name = "葱花";
var user_age = 1;

const template = 
`<div>
    <p>姓名: <strong>${window.user_name}</strong></p>
    <p>年龄: <strong>${window.user_age}</strong></p>
</div>`
document.write(template);
```

如果在这里使用 let 或 const 替代 var，则不会把变量添加给全局对象：

```javascript
let age = 29;
const sayAge = () => alert(this.age);
alert(window.age); // undefined
sayAge(); // undefined
window.sayAge(); // TypeError: window.sayAge is not a function 
```

JavaScript 中有很多对象都暴露在全局作用域中，比如 location 和 navigator（本章后面 都会讨论），因而它们也是 window 对象的属性。

> ES11 引入了globalThis，为了和nodejs统一，在浏览器中`globalThis === window`，在nodejs中`glovalThis === global`

#### [1.1 浏览器窗口](#)

window 对象指当前的浏览器窗口。它也是当前页面的**顶层对象**，即最高一层的对象，所有其他对象都是它的下属, 一般而言window对象代表最顶层窗口，但是也存在一项其他情况，例如浏览器弹窗，再打开一个window对象，所有就存在窗口之间的关系。

* window.top 对象始终指向最上层（最外层）窗口。
* window.parent 对象则始终指向当前窗 口的父窗口。**如果当前窗口是最上层窗口，则 parent 等于 top，都等于 window**。
* window.self 和 window 就 是同一个对象

```javascript
let istop = window.top == window.self;
```

#### [1.2 窗口位置与像素比](#)

window 对象的位置可以通过不同的属性和方法来确定。现代浏览器提供了 screenLeft 和 screenTop 属性，用于表示窗口相对于屏幕左侧和顶部的位置 ，返回值的单位是 CSS 像素。

```javascript
let gap_top = window.screenTop; 
let gap_left = window.screenLeft;
```

可以使用 moveTo()和 moveBy()方法移动窗口,这两个方法都接收两个参数,依浏览器而定，这两个方法可能会被部分或全部禁用。

1. moveTo(x,y)接 收要移动到的新位置的绝对坐标 x 和 y；
2.  moveBy(x,y)则接收相对当前位置在两个方向上移动的像素数。 

```javascript
// 把窗口移动到左上角
window.moveTo(0,0);
// 把窗口向下移动 100 像素
window.moveBy(0, 100);
// 把窗口移动到坐标位置(200, 300)
window.moveTo(200, 300);
// 把窗口向左移动 50 像素
window.moveBy(-50, 0);
```

**像素比**：

CSS 像素是 Web 开发中使用的统一像素单位。这个单位的背后其实是一个角度：0.0213°。如果屏幕距离人眼是一臂长，则以这个角度计算的 CSS 像素大小约为 1/96 英寸。这样定义像素大小是为了在 不同设备上统一标准。

**比如，低分辨率平板设备上 12 像素（CSS 像素）的文字应该与高清 4K 屏幕下 12 像素（CSS 像素）的文字具有相同大小**。这就带来了一个问题，不同像素密度的屏幕下就会有不同的**缩放系数**，以便**把物理像素（屏幕实际的分辨率）转换为 CSS 像素（浏览器报告的虚拟分辨率）**。 

>  举个例子，手机屏幕的物理分辨率可能是 1920×1080，但因为其像素可能非常小，所以浏览器就需 要将其分辨率降为较低的逻辑分辨率，比如 640×320。

这个物理像素与 CSS 像素之间的转换比率由 **window.devicePixelRatio** 属性提供。

> 对于分辨率从 1920×1080 转换为 640×320 的设备，window. devicePixelRatio 的值就是 3。

这样一来，12 像素（CSS 像素）的文字实际上就会用 36 像素的物理 像素来显示。 window.devicePixelRatio 实际上与每英寸像素数（DPI，dots per inch）是对应的。DPI 表示单 位像素密度，而 window.devicePixelRatio 表示物理像素与逻辑像素之间的缩放系数。

#### [1.3 窗口大小](#)
在不同浏览器中确定浏览器窗口大小没有想象中那么容易。所有现代浏览器都支持 4 个属性：

**window 的属性**
* innerWidth 返回浏览器窗口的内部宽度，包括滚动条（如果存在）。这个值是视口的宽度。
* innerHeight 返回浏览器窗口的内部高度，包括滚动条（如果存在）。这个值是视口的高度。
* outerWidth 返回整个浏览器窗口的外部宽度，包括工具栏和滚动条。
* outerHeight 返回整个浏览器窗口的外部高度，包括工具栏和滚动条。

> 在移动设备上，window.innerWidth 和 window.innerHeight 返回视口的大小，也就是屏幕上
页面可视区域的大小。

**视口高度**
* document.documentElement.clientWidth  返回文档视口的宽度，不包括垂直滚动条（如果有）。
* document.documentElement.clientHeight 返回文档视口的高度，不包括水平滚动条（如果有）。

> Mobile Internet Explorer 把布局视口的信息保存在
document.body.clientWidth 和 document.body.clientHeight 中。在放大或缩小页面时，这些值也会相应变化。

```javascript
let template = `<p>${window.innerWidth}</p>
<p>${window.innerHeight}</p>
<p>${window.outerWidth}</p>
<p>${window.outerHeight}</p>
<p>${document.documentElement.clientWidth}</p>
<p>${document.documentElement.clientHeight}</p>
<p>width:${document.body?.clientWidth ?? "not found"}</p>
<p>width:${document.body?.clientHeight ?? "not found"}</p>`

document.writeln(template);
```

resizeTo 和 resizeBy 是 JavaScript 中用于调整浏览器窗口大小的方法。它们是 window 对
象的成员，**并且主要用于控制弹出窗口（即通过 window.open 方法创建的新窗口）**。需要注意的是，
出于用户体验和安全性的考虑，现代浏览器对这些方法的应用有一定限制。

window.resizeTo(width, height) 功能: 将窗口调整到指定的宽度和高度。如果窗口已经是全屏模式，则此方法可能不会生效。
* **参数:**
  * width: 设置窗口的新宽度（以像素为单位）。
  * height: 设置窗口的新高度（以像素为单位）。

window.resizeBy(x, y) 功能: 根据当前窗口尺寸，按给定的增量或减量调整窗口大小。
* **参数:**
  * x: 水平方向上调整的像素数。正值表示向右扩展，负值表示向左收缩。
  * y: 垂直方向上调整的像素数。正值表示向下扩展，负值表示向上收缩。


```javascript
// 缩放到 100×100
window.resizeTo(100, 100);
// 缩放到 200×150
window.resizeBy(100, 50);
// 缩放到 300×300
window.resizeTo(300, 300); 
```



#### [1.x document对象](#)

