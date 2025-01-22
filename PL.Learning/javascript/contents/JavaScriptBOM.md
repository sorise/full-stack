## [JavaScript BOM](#)
> **介绍**：浏览器对象模型（BOM，Browser Object Model）为是使用 JavaScript 开发 Web 应用程序的核心，主要有window对象、location对象、navigator对象、history对象。

---

- [1. window 对象](#)
- 

---

### [1.window 对象](#)
**BOM 的核心是 window 对象，表示浏览器的实例**。浏览器里面，**window**对象（注意，w为小写）指当前的浏览器窗口。它也是当前页面的**顶层对象**，即最高一层的对象，所有其他对象都是它的下属。一个变量如果未声明，那么默认就是顶层对象的属性。

![img.png](static/bomasdasdasd3a41321img.png)

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

#### [1.4 window.open](#)
[MDN window.open](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/open) 方法可以用于导航到指定 URL，也可以用于打开新浏览器窗口。这个方法接收4个参数,要加载的`URL`、`目标窗口`、`特性字符串`和`表示新窗口` 在浏览器历史记录中是否替代当前加载页面的布尔值。

```javascript
open()
open(url)
open(url, target)
open(url, target, windowFeatures)
```
**第一个参数**，一个字符串，表示要加载的资源的 URL 或路径。如果指定空字符串（""）或省略此参数，则会在目标浏览上下文中打开一个空白页。

**第二个参数**,也可以是一个特殊的窗口名，比如 `_self`、 `_parent`、 `_top` 或 `_blank`。
```javascript
window.open("http://www.wrox.com/", "topFrame"); 
```
如果 `window.open()` 的第二个参数是一个已经存在的窗口或窗格（`frame`）的名字，则会在对应的窗口或窗格中打开 `URL`。
```html
<a href="http://www.wrox.com" target="topFrame"/>
```
**第三个参数 windowFeatures**：一个字符串，包含以逗号分隔的窗口特性列表，形式为 name=value，布尔特性则仅为 name。这些特性包括窗口的默认大小和位置、是否打开最小弹出窗口等选项。支持以下选项：

* "width=xxx"：指定窗口的宽度，以像素为单位。
* "height=xxx"：指定窗口的高度，以像素为单位。
* "top=xxx"：指定窗口的上边距，以像素为单位。
* "left=xxx"：指定窗口的左边距，以像素为单位。
* "resizable=yes/no"：指定窗口是否可以被用户调整大小。
* "scrollbars=yes/no"：指定窗口是否显示滚动条。
* "toolbar=yes/no"：指定窗口是否显示工具栏。
* "location=yes/no"：指定窗口是否显示地址栏。
* "status=yes/no"：指定窗口是否显示状态栏。

```javascript
// 打开一个新窗口，显示指定的URL
window.open('https://blog.csdn.net/Jin_Xiang123?type=blog', 'NewWindow', 'width=500,height=300');
 
// 打开一个新窗口，不显示URL
window.open('', 'NewWindow', 'width=500,height=300');
 
// 打开一个新窗口，显示指定的HTML内容
window.open('', 'NewWindow', 'width=500,height=300').document.write('<h1>Hello, World!</h1>');
```

在浏览器扩展或其他程序屏蔽弹窗时，window.open()通常会抛出错误。因此要准确检测弹窗是
否被屏蔽，除了检测 window.open()的返回值，还要把它用 try/catch 包装起来，像这样：

```javascript
let blocked = false;
try {
  let wroxWin = window.open("http://www.wrox.com", "_blank");
  if (wroxWin == null){ 
     blocked = true;
  }
}catch (ex){
  blocked = true;
}

if (blocked){
  window.alert("The popup was blocked!");
} 
```
无论弹窗是用什么方法屏蔽的，以上代码都可以准确判断调用 window.open()的弹窗是否被屏蔽了。

#### [1.5 定时器](#)
JavaScript 在浏览器中是单线程执行的，但允许使用定时器指定在某个时间之后或每隔一段时间就执行相应的代码。

```javascript
// 设置超时任务
let timeoutId = setTimeout(() => alert("Hello world!"), 1000);
// 取消超时任务
clearTimeout(timeoutId);
```

* let timer_st = setTimeout(func, time)：循环一次。
* let timer_it = setInterval(func, time)：循环多次。
* clearTimeout(timer_st)：清除一次性定时器。
* clearInterval(timer_it)：清除多次定时器。

```javascript
function boom(){
    console.log('boom');
}

setTimeout(boom, 1000); //1秒后执行代码

function go(){
    console.log('起床了')
}

setInterval(go, 3000); //每隔3秒不断执行代码

//执行一个循环定时器，并将返回值赋给timer这个变量
var timer = setInterval(function(){
    console.log('2');
},1000);

console.log(timer);
//取消定时器需要有索引值，timer保存的是动画队列里的索引值(key) ，clearInterval停止动画需要一个索引值
clearInterval(timer);
```

#### [1.6 弹窗](#)
在JavaScript中，弹窗是一种用于在浏览器窗口中显示信息的小窗口。它可以用来显示警告、确认、输入、提示等类
型的消息给用户。弹窗通常是通过调用window.alert()、window.confirm()和window.prompt()等函数来实现的。

* **window.alert(message)**：弹出一个包含指定消息和一个"确定"按钮的警告框，用于向用户显示一条信息。它只有一个按钮，点击确定后弹窗会自动关闭。
* **window.confirm(message)**：弹出一个包含指定消息、一个"确定"按钮和一个"取消"按钮的确认框，用于向用户显示一条信息并接受用户的确认或取消选择。用户可以点击确定或取消按钮来关闭弹窗，并根据选择返回true或false。
* **window.prompt(message, defaultText)**：弹出一个包含指定消息、一个文本框、一个"确定"按钮和一个"取消"按钮的提示框，用于向用户显示一条信息并接受用户输入。用户可以在文本框中输入内容，并点击确定或取消按钮来关闭弹窗。根据用户的选择返回输入内容或null。

```javascript
let result = window.prompt("your age is what", 25);
window.alert(`your input: ${result}`);
```

JavaScript 还可以显示另外两种对话框：find()和 print()。这两种对话框都是异步显示的，即控
制权会立即返回给脚本。用户在浏览器菜单上选择“查找”（find）和“打印”（print）时显示的就是这
两种对话框。通过在 window 对象上调用 find()和 print()可以显示它们，比如：

```javascript
// 显示打印对话框
window.print();
// 显示查找对话框
window.find(); 
```

#### [1.7 document对象](#)
文档对象(document) 代表浏览器窗口中的文档，该对象是window对象的子对象，由于window对象是DOM对象模型中的默认对象，因此window对象中
的方法和子对象不需要使用window来引用。通过document对象可以访问HTML文档中包含的任何HTML标记，并可以动态
地改变HTML标记中的内容，例如表单、图像、表格和超链接等。

```javascript
// 将背景颜色修改为红色
document.body.style.background = "red";
// 在 1 秒后将其修改回来
setTimeout(() => document.body.style.background = "", 1000);
```

在最顶层：**documentElement** 和 **body** 。
最顶层的树节点可以直接作为 document 的属性来使用：
* `<html> = document.documentElement`

最顶层的 document 节点是 document.documentElement。这是对应 `<html>` 标签的 DOM 节点。
* `<body> = document.body`

另一个被广泛使用的 DOM 节点是 `<body>` 元素 — document.body。
* `<head> = document.head`

`<head>` 标签可以通过 document.head 访问。

**详细内容将会在DOM中解释**。

### [2. location](#)
location 是最有用的 BOM 对象之一，提供了当前窗口中加载文档的信息，以及通常的导航功能。
这个对象独特的地方在于，它既是 window 的属性，也是 document 的属性。

假设浏览器当前加载的URL是 `http://foouser:barpassword@www.wrox.com:80/WileyCDA/?q=javascript#contents` 。

| 属 性                                                                     | 值                                                          | 说 明|
|:------------------------------------------------------------------------|:-----------------------------------------------------------|:---|
| location.hash | `"#contents"`                                              |URL 散列值（井号后跟零或多个字符），如果没有则为空字符串               |
| location.host | `"www.wrox.com:80"`                                        |服务器名及端口号|                               
| location.hostname | `"www.wrox.com"`                                           |服务器名|                                      
| location.href | `"http://www.wrox.com:80/WileyCDA/?q=javascript#contents"` | 当前加载页面的完整URL。 | location 的 toString()方法返回这个值   |
| location.pathname  | `"/WileyCDA/"`                                               | URL 中的路径和（或）文件名                            |
| location.port    | `"80"`                                                       |请求的端口。如果 URL中没有端口，则返回空字符串                          |
| location.protocol    | `"http:"`                                                   |页面使用的协议。通常是"http:"或"https:"                      |
| location.search    | `"?q=javascript"`                                          | URL 的查询字符串。这个字符串以问号开头|                    
| location.username   | `"foouser"`                                                |域名前指定的用户名    |                                  
| location.password | `"barpassword"`                                            |域名前指定的密码|                                   
| location.origin  | `"http://www.wrox.com"`                                    |URL 的源地址。只读|                        

#### [2.1 查询字符串](#)
location 的多数信息都可以通过上面的属性获取。但是 URL 中的查询字符串并不容易使用。虽然location.search 
返回了从问号开始直到 URL 末尾的所有内容，但没有办法逐个访问每个查询参数。

```javascript
let getQueryStringArgs = function() {
  // 取得没有开头问号的查询字符串
  let qs = (location.search.length > 0 ? location.search.substring(1) : ""),
  // 保存数据的对象
  args = {};
  // 把每个参数添加到 args 对象
  for (let item of qs.split("&").map(kv => kv.split("="))) {
    let name = decodeURIComponent(item[0]),
            value = decodeURIComponent(item[1]);
    if (name.length) {
      args[name] = value;
    }
  }
  return args;
}

// 假设查询字符串为?q=javascript&num=10 
let args = getQueryStringArgs();

alert(args["q"]); // "javascript" 
alert(args["num"]); // "10"
```

**URLSearchParams** 提供了一组标准 API 方法，通过它们可以检查和修改查询字符串。给
URLSearchParams 构造函数传入一个查询字符串，就可以创建一个实例。这个实例上暴露了 get()、
set()和 delete()等方法，可以对查询字符串执行相应操作。

```javascript
let qs = "?q=javascript&num=10"; 
let searchParams = new URLSearchParams(qs); 

alert(searchParams.toString()); // " q=javascript&num=10" 

searchParams.has("num"); // true 
searchParams.get("num"); // 10 
searchParams.set("page", "3"); 

alert(searchParams.toString()); // " q=javascript&num=10&page=3" 

searchParams.delete("q"); 
alert(searchParams.toString()); // " num=10&page=3"
```
大多数支持 URLSearchParams 的浏览器也支持将 URLSearchParams 的实例用作可迭代对象：

```javascript
let qs = "?q=javascript&num=10"; 
let searchParams = new URLSearchParams(qs); 

for (let param of searchParams) { 
    console.log(param); 
} 
// ["q", "javascript"] 
// ["num", "10"]
```

#### [2.2 操作地址](#)
可以通过修改 location 对象修改浏览器的地址。首先，最常见的是使用 assign()方法并传入一个 URL，如下所示：
```javascript
location.assign("http://www.wrox.com");
```
这行代码会立即启动导航到新 URL 的操作，同时在浏览器历史记录中增加一条记录。

如果给location.href 或 window.location 设置一个 URL，也会以同一个 URL 值调用 assign()方法。比如，下面两行代码都会执行与显式调用 assign()一样的操作：

```javascript
window.location = "http://www.wrox.com";
location.href = "http://www.wrox.com";
```

修改 location 对象的属性也会修改当前加载的页面。其中，hash、search、hostname、pathname和 port 属性被设置为新值之后都会修改当前 URL，如下面的例子所示：

```javascript
// 假设当前 URL 为 http://www.wrox.com/WileyCDA/ 
// 把 URL 修改为 http://www.wrox.com/WileyCDA/#section1 
location.hash = "#section1"; 
// 把 URL 修改为 http://www.wrox.com/WileyCDA/?q=javascript 
location.search = "?q=javascript"; 
// 把 URL 修改为 http://www.somewhere.com/WileyCDA/ 
location.hostname = "www.somewhere.com"; 
// 把 URL 修改为 http://www.somewhere.com/mydir/ 
location.pathname = "mydir"; 
// 把 URL 修改为 http://www.somewhere.com:8080/WileyCDA/ 
location.port = 8080;
```
除了 hash 之外，只要修改 location 的一个属性，就会导致页面重新加载新 URL。

