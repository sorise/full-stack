## [JavaScript DOM扩展](#)
**介绍**：DOM扩展通常指的是在标准的文档对象模型（Document Object Model, DOM）基础上，由浏览器或JavaScript框架
提供的额外功能。这些扩展增强了开发者对网页文档的操作能力，使得网页开发更加灵活和强大。

----

- [1. DOM 扩展](#1-dom-扩展)
- [2. Selectors API](#2selectors-api)
- [3. 元素遍历](#3-元素遍历)
- [4. CSS 类扩展](#4-css-类扩展)
- [5. 焦点管理](#5-焦点管理)
- [6. HTML5](#6-html5)
- [7. 自定义数据属性](#7-自定义数据属性)
- [8. 插入标签](#8-插入标记)
- [9. 跨站点脚本](#9-跨站点脚本)
- [10. 滚动](#10-滚动)
- [11. 专有扩展](#11-专有扩展)

----
### [1. DOM 扩展](#)
DOM扩展通常指的是在标准的文档对象模型（Document Object Model, DOM）基础上，由浏览器或JavaScript框架提供的额外功能。这些扩展增强了开发者对网页文档的操作能力，使得网页开发更加灵活和强大。以下是一些常见的DOM扩展类型：
1. **HTML5 新增的API**：HTML5引入了多个新的API，如用于处理视频、音频、图形、本地存储等的功能。这些新特性不仅丰富了Web应用的能力，也扩展了DOM的功能。例如，`<canvas>`元素结合JavaScript可以用来绘制图形。
2. **自定义数据属性(data-*)**：HTML5允许在元素中添加以`data-`为前缀的自定义属性，这为页面元素附加了额外的信息。通过JavaScript中的`element.dataset`属性可以访问这些信息。
3. **classList API**：这是一个简化操作元素类名的方法集合，包括添加、删除、切换和检查类名的功能。相比直接操作`className`属性，它提供了更简洁的接口。
4. **querySelector 和 querySelectorAll 方法**：这两个方法提供了一种更方便的方式来选择DOM元素，使用CSS选择器语法，比传统的`getElementById`、`getElementsByClassName`等方法更加直观和强大。
5. **事件监听器的改进**：现代浏览器支持`addEventListener`方法，它提供了一个更加灵活的机制来注册事件处理器，支持事件捕获和冒泡阶段的选择，并且允许向同一元素的同一事件类型注册多个处理器。
6. **Mutation Observers**：用于监视DOM树的变化（如节点的添加、删除、属性的改变等），并在变化发生时执行指定的回调函数。这是对旧版Mutation Events的一个高效替代方案。
7. **Element Traversal**：提供了一些便捷的方法来遍历DOM树，比如获取一个元素的第一个子元素、最后一个子元素、下一个兄弟元素、上一个兄弟元素等。

不同的浏览器可能还会有自己的特定扩展，但上述提到的是广泛支持的标准扩展。随着Web技术的发展，我们可以期待更多有助于提高开发效率和增强Web应用能力的DOM扩展出现。

### [2、Selectors API](#)
JavaScript 库中最流行的一种能力就是根据 CSS 选择符的模式匹配 DOM 元素。比如，jQuery 就完全
以 CSS 选择符查询 DOM 获取元素引用，而不是使用 getElementById()和 getElementsByTagName()。

> Selectors API（参见 W3C 网站上的 Selectors API Level 1）是 W3C 推荐标准，规定了浏览器原生支
持的 CSS 查询 API。

Selectors API Level 1 的核心是两个方法：**querySelector()** 和 **querySelectorAll()**。在兼容浏览器中，Document 类型和 Element 类型的实例上都会暴露这两个方法。

Selectors API Level 2 规范在 Element 类型上新增了更多方法，比如 matches()、find()和findAll()。不过，目前还没有浏览器实现或宣称实现 find()和 findAll()。

#### [2.1 querySelector](#)
querySelector()方法接收 CSS 选择符参数，返回匹配该模式的第一个后代元素，如果没有匹配
项则返回 null。下面是一些例子：

```javascript
// 取得<body>元素
let body = document.querySelector("body");
// 取得 ID 为"myDiv"的元素
let myDiv = document.querySelector("#myDiv");
// 取得类名为"selected"的第一个元素
let selected = document.querySelector(".selected");
// 取得类名为"button"的图片
let img = document.body.querySelector("img.button"); 
```
在 Document 上使用 **querySelector**()方法时，会从文档元素开始搜索；在 Element 上使用
**querySelector**()方法时，则只会从当前元素的后代中查询。

用于查询模式的 CSS 选择符可繁可简，依需求而定。如果选择符有语法错误或碰到不支持的选择符， 则 **querySelector**()方法会抛出错误。

#### [2.2 querySelectorAll](#)
querySelectorAll()方法跟 querySelector()一样，也接收一个用于查询的参数，但它会返回
所有匹配的节点，而不止一个。这个方法返回的是一个 NodeList 的静态实例。

再强调一次，querySelectorAll()返回的 NodeList 实例一个属性和方法都不缺，但它是一个静态的“快照”，而非“实时”的查询。这样的底层实现避免了使用 NodeList 对象可能造成的性能问题。

**querySelector**()一样，**querySelectorAll**()也可以在 Document、DocumentFragment 和
Element 类型上使用。下面是几个例子：
```javascript
// 取得 ID 为"myDiv"的<div>元素中的所有<em>元素
let ems = document.getElementById("myDiv").querySelectorAll("em");
// 取得所有类名中包含"selected"的元素
let selecteds = document.querySelectorAll(".selected");
// 取得所有是<p>元素子元素的<strong>元素
let strongs = document.querySelectorAll("p strong"); 
```
返回的 NodeList 对象可以通过 for-of 循环、item()方法或中括号语法取得个别元素。比如：
```javascript
let strongElements = document.querySelectorAll("p strong"); 
```

```javascript
// 以下 3 个循环的效果一样
for (let strong of strongElements) {
    strong.className = "important";
}

for (let i = 0; i < strongElements.length; ++i) {
    strongElements.item(i).className = "important";
}

for (let i = 0; i < strongElements.length; ++i) {
    strongElements[i].className = "important";
} 
```
与 querySelector()方法一样，如果选择符有语法错误或碰到不支持的选择符，则 querySelectorAll()方法会抛出错误。

#### [2.3 matches](#)
matches()方法（在规范草案中称为 matchesSelector()）接收一个 CSS 选择符参数，如果元素匹配则该选择符返回 true，否则返回 false。

```javascript
if (document.body.matches("body.page1")){
 // true
} 
```
使用这个方法可以方便地检测某个元素会不会被 querySelector()或 querySelectorAll()方法返回。

所有主流浏览器都支持 matches()。

### [3. 元素遍历](#)
IE9 之前的版本不会把元素间的空格当成空白节点，而其他浏览器则会。这样就导致了 childNodes 和 firstChild 等属性上的差异。

Element Traversal API 为 DOM 元素添加了 5 个属性：
- **childElementCount**，返回子元素数量（不包含文本节点和注释）；
- **firstElementChild**，指向第一个 Element 类型的子元素（Element 版 firstChild）；
- **lastElementChild**，指向最后一个 Element 类型的子元素（Element 版 lastChild）；
- **previousElementSibling** ，指向前一个 Element 类型的同胞元素（ Element 版 previousSibling）；
- **nextElementSibling**，指向后一个 Element 类型的同胞元素（Element 版 nextSibling）。

```javascript
let parentElement = document.getElementById('parent');
let currentChildNode = parentElement.firstChild;

// 没有子元素，firstChild 返回 null，跳过循环
while (currentChildNode) {
    if (currentChildNode.nodeType === 1) {
         // 如果有元素节点，则做相应处理
         processChild(currentChildNode);
    }
    if (currentChildNode === parentElement.lastChild) {
        break;
    }
    currentChildNode = currentChildNode.nextSibling;
} 
```
使用 Element Traversal 属性之后，以上代码可以简化如下：
```javascript
let parentElement = document.getElementById('parent');
let currentChildElement = parentElement.firstElementChild;

// 没有子元素，firstElementChild 返回 null，跳过循环
while (currentChildElement) {
    // 这就是元素节点，做相应处理
    processChild(currentChildElement);
    if (currentChildElement === parentElement.lastElementChild) {
        break;
    }
    currentChildElement = currentChildElement.nextElementSibling;
} 
```

### [4. CSS 类扩展](#)
为了适应开发者和他们对 class 属性的认可，HTML5 增加了一些特性以方便使用 CSS 类。

#### [4.1 getElementsByClassName](#)
`getElementsByClassName()` 是 HTML5 新增的最受欢迎的一个方法，暴露在 document 对象和所有 HTML 元素上。
这个方法脱胎于基于原有 DOM 特性实现该功能的 JavaScript 库，提供了性能高好的原生实现。

`getElementsByClassName()` 方法接收一个参数，即包含一个或多个类名的字符串，返回类名中包含相应类的元素的 NodeList。
如果提供了多个类名，则顺序无关紧要。下面是几个示例：
```javascript
// 取得所有类名中包含"username"和"current"元素
// 这两个类名的顺序无关紧要

let allCurrentUsernames = document.getElementsByClassName("username current");
// 取得 ID 为"myDiv"的元素子树中所有包含"selected"类的元素
let selected = document.getElementById("myDiv").getElementsByClassName("selected"); 
```
这个方法只会返回以调用它的对象为根元素的子树中所有匹配的元素。在 document 上调用 getElementsByClassName()
返回文档中所有匹配的元素，而在特定元素上调用 getElementsByClassName()则返回该元素后代中匹配的元素。

如果要给包含特定类（而不是特定 ID 或标签）的元素添加事件处理程序，使用这个方法会很方便。
不过要记住，因为返回值是 NodeList，所以使用这个方法会遇到跟使用 getElementsByTagName()
和其他返回 NodeList 对象的 DOM 方法同样的问题。

IE9 及以上版本，以及所有现代浏览器都支持 getElementsByClassName()方法。

#### [4.2 classList 属性](#)
要操作类名，可以通过 className 属性实现添加、删除和替换。但 className 是一个字符串，
所以每次操作之后都需要重新设置这个值才能生效，即使只改动了部分字符串也一样。
```html
<div class="bd user disabled">...</div>
```
这个`<div>`元素有 3 个类名。要想删除其中一个，就得先把 className 拆开，删除不想要的那个，再把包含剩余类的字符串设置回去。
```javascript
// 要删除"user"类
let targetClass = "user";
// 把类名拆成数组
let classNames = div.className.split(/\s+/);
// 找到要删除类名的索引
let idx = classNames.indexOf(targetClass);
// 如果有则删除
if (idx > -1) {
    classNames.splice(i,1);
}
// 重新设置类名
div.className = classNames.join(" "); 
```
这就是从 `<div>` 元素的类名中删除"user"类要写的代码。替换类名和检测类名也要涉及同样的算法。

HTML5 通过给所有元素增加 classList 属性为这些操作提供了更简单也更安全的实现方式。
classList 是一个新的集合类型 DOMTokenList 的实例。与其他 DOM 集合类型一样，DOMTokenList
也有 length 属性表示自己包含多少项，也可以通过 item()或中括号取得个别的元素。此外，
DOMTokenList 还增加了以下方法。

- **add(value)**，向类名列表中添加指定的字符串值 value。如果这个值已经存在，则什么也不做。
- **contains(value)**，返回布尔值，表示给定的 value 是否存在。
- **remove(value)**，从类名列表中删除指定的字符串值 value。
- **toggle(value)**，如果类名列表中已经存在指定的 value，则删除；如果不存在，则添加。

这样以来，前面的例子中那么多行代码就可以简化成下面的一行：
```javascript
div.classList.remove("user"); 
```
这行代码可以在不影响其他类名的情况下完成删除。其他方法同样极大地简化了操作类名的复杂性，如下面的例子所示：
```javascript
// 删除"disabled"类
div.classList.remove("disabled");
// 添加"current"类
div.classList.add("current");
// 切换"user"类
div.classList.toggle("user");
// 检测类名
if (div.classList.contains("bd") && !div.classList.contains("disabled")){
    // 执行操作
)
// 迭代类名
for (let class of div.classList){
    doStuff(class);
} 
```
添加了 classList 属性之后，除非是完全删除或完全重写元素的 class 属性，否则 className 属性就用不到了。

### [5. 焦点管理](#)
HTML5 增加了辅助 DOM 焦点管理的功能。首先是 document.activeElement，始终包含当前拥有焦点的 DOM 元素。
页面加载时，可以通过用户输入（按 Tab 键或代码中使用 focus()方法）让某个元素自动获得焦点。

```javascript
let button = document.getElementById("myButton");

button.focus();
console.log(document.activeElement === button); // true
```
默认情况下，`document.activeElement` 在页面刚加载完之后会设置为 document.body。而在页面完全加载
之前，`document.activeElement` 的值为 null。

其次是 `document.hasFocus()` 方法，该方法返回布尔值，表示文档是否拥有焦点：
```javascript
let button = document.getElementById("myButton");
button.focus();
console.log(document.hasFocus()); // true 
```
确定文档是否获得了焦点，就可以帮助确定用户是否在操作页面。

### [6. HTML5](#)
HTML5 代表着与以前的 HTML 截然不同的方向。在所有以前的 HTML 规范中，从未出现过描述JavaScript 接口的情形，HTML 就是一个纯标记语言。

#### [6.1 HTMLDocument 扩展](#)
HTML5 扩展了 HTMLDocument 类型，增加了更多功能。与其他 HTML5 定义的 DOM 扩展一样，这些变化同样基于所有浏览器事实上都已经支持的专有扩展。

> 为此，即使这些扩展的标准化相对较晚，很多浏览器也早就实现了相应的功能。

#### [6.2 readyState 属性](#)
readyState 是 IE4 最早添加到 document 对象上的属性，后来其他浏览器也都依葫芦画瓢地支持
这个属性。最终，HTML5 将这个属性写进了标准。document.readyState 属性有两个可能的值：
- loading，表示文档正在加载；
- complete，表示文档加载完成。

实际开发中，最好是把 document.readState 当成一个指示器，以判断文档是否加载完毕。在这个属性得到广泛支持以前，通常
要依赖 onload 事件处理程序设置一个标记，表示文档加载完了。这个属性的基本用法如下：

```javascript
if (document.readyState == "complete"){
// 执行操作
} 
```

#### [6.3 compatMode 属性](#)
自从 IE6 提供了以标准或混杂模式渲染页面的能力之后，检测页面渲染模式成为一个必要的需求。
IE 为 document 添加了 compatMode 属性，这个属性唯一的任务是指示浏览器当前处于什么渲染模式。

```javascript
if (document.compatMode == "CSS1Compat"){
    console.log("Standards mode");
} else {
    console.log("Quirks mode");
} 
```
HTML5 最终也把 compatMode 属性的实现标准化了。

#### [6.4  head 属性](#)
作为对 document.body（指向文档的 `<body>` 元素）的补充，HTML5 增加了 document.head 属
性，指向文档的 `<head>` 元素。可以像下面这样直接取得 `<head>` 元素：

```javascript
let head = document.head;
```
HTML5 增加了几个与文档字符集有关的新属性。其中，characterSet 属性表示文档实际使用的字符集，也可以用来指定新字符集。
这个属性的默认值是"UTF-16"，但可以通过 `<meta>` 元素或响应头，以及新增的 characterSeet 属性来修改。下面是一个例子：
```javascript
console.log(document.characterSet); // "UTF-16"
document.characterSet = "UTF-8"; 
```

#### [6.5 字符集属性](#)
HTML5 增加了几个与文档字符集有关的新属性。其中，characterSet 属性表示文档实际使用的
字符集，也可以用来指定新字符集。这个属性的默认值是"UTF-16"，但可以通过 `<meta>` 元素或响应头，以及新增
的 characterSeet 属性来修改。下面是一个例子：

```javascript
console.log(document.characterSet); // "UTF-16"
document.characterSet = "UTF-8";
```

### [7. 自定义数据属性](#)
HTML5 允许给元素指定非标准的属性，但要使用前缀 data-以便告诉浏览器，这些属性既不包含与渲染有关的信息，也不包含
元素的语义信息。除了前缀，自定义属性对命名是没有限制的，data-后面跟什么都可以。
```html
<div id="myDiv" data-appId="12345" data-myname="Nicholas"></div> 
```
定义了自定义数据属性后，可以通过元素的 dataset 属性来访问。dataset 属性是一个DOMStringMap 的实例，包含一组键/值对映射。
```javascript
// 本例中使用的方法仅用于示范
let div = document.getElementById("myDiv");

// 取得自定义数据属性的值
let appId = div.dataset.appId;
let myName = div.dataset.myname;

// 设置自定义数据属性的值
div.dataset.appId = 23456;
div.dataset.myname = "Michael";

// 有"myname"吗？
if (div.dataset.myname){
    console.log(`Hello, ${div.dataset.myname}`);
} 
```
自定义数据属性非常适合需要给元素附加某些数据的场景，比如链接追踪和在聚合应用程序中标识页面的不同部分。

### [8. 插入标记](#)
DOM 虽然已经为操纵节点提供了很多 API，但向文档中一次性插入大量 HTML 时还是比较麻烦。
相比先创建一堆节点，再把它们以正确的顺序连接起来，直接插入一个 HTML 字符串要简单（快速）得多。

#### [8.1 innerHTML 属性](#)
**innerHTML 属性** :在读取 innerHTML 属性时，会返回元素所有后代的 HTML 字符串，包括元素、注释和文本节点。
而在写入 innerHTML 时，则会根据提供的字符串值以新的 DOM 子树替代元素中原来包含的所有节点。
比如下面的 HTML 代码：
```html
<div id="content">
 <p>This is a <strong>paragraph</strong> with a list following it.</p>
 <ul>
     <li>Item 1</li>
     <li>Item 2</li>
     <li>Item 3</li>
 </ul>
</div> 
```
对于这里的 `<div>` 元素而言，其 innerHTML 属性会返回以下字符串：
```html
<p>This is a <strong>paragraph</strong> with a list following it.</p>
<ul>
     <li>Item 1</li>
     <li>Item 2</li>
     <li>Item 3</li>
</ul> 
```
实际返回的文本内容会因浏览器而不同。IE 和 Opera 会把所有元素标签转换为大写，而 Safari、
Chrome 和 Firefox 则会按照文档源代码的格式返回，包含空格和缩进。因此不要指望不同浏览器的
innerHTML 会返回完全一样的值。
```javascript
div.innerHTML = "Hello world!"; 
```
因为浏览器会解析设置的值，所以给 innerHTML 设置包含 HTML 的字符串时，结果会大不一样，来看下面的例子：
```javascript
div.innerHTML = "Hello & welcome, <b>\"reader\"!</b>"; 
```
这个操作的结果相当于：
```html
<div id="content">Hello &amp; welcome, <b>&quot;reader&quot;!</b></div> 
```
设置完 innerHTML，马上就可以像访问其他节点一样访问这些新节点。

#### [8.2 旧IE中的innerHTML](#)
在所有现代浏览器中，通过 innerHTML 插入的 `<script>` 标签是不会执行的。而在 IE8 及之前的版
本中，只要这样插入的 `<script>`元素指定了 defer 属性，且`<script>`之前是“受控元素”（scoped
element），那就是可以执行的。`<script>`元素与`<style>`或注释一样，都是“非受控元素”（NoScope
element），也就是在页面上看不到它们。

```html
// 行不通
div.innerHTML = "<script defer>console.log('hi');<\/script>";
```
在这个例子中，innerHTML 字符串以一个非受控元素开始，因此整个字符串都会被清空。
```javascript
// 以下都可行
div.innerHTML = "_<script defer>console.log('hi');<\/script>";
div.innerHTML = "<div>&nbsp;</div><script defer>console.log('hi');<\/script>";
div.innerHTML = "<input type=\"hidden\"><script defer>console.log('hi');<\/script>";
```
为了达到目的，必须在 `<script>` 前面加上一个受控元素，例如文本节点或没有结束标签的元素（如`<input>`）。因此，下面的代码就是可行的：
```javascript
div.innerHTML = "<style type=\"text/css\">body {background-color: red; }</style>"; 
```
但在 IE8 及之前的版本中，`<style>` 也被认为是非受控元素，所以必须前置一个受控元素：
```javascript
div.innerHTML = "_<style type=\"text/css\">body {background-color: red; }</style>";
div.removeChild(div.firstChild);
```
> Firefox 在内容类型为 application/xhtml+xml 的 XHTML 文档中对 innerHTML
> 更加严格。在 XHTML 文档中使用 innerHTML，必须使用格式良好的 XHTML 代码。否
> 则，在 Firefox 中会静默失败。

#### [8.3 outerHTML 属性](#)
读取 outerHTML 属性时，会返回调用它的元素（及所有后代元素）的 HTML 字符串。在写入
outerHTML 属性时，调用它的元素会被传入的 HTML 字符串经解释之后生成的 DOM 子树取代。

```html
<div id="content">
 <p>This is a <strong>paragraph</strong> with a list following it.</p>
 <ul>
     <li>Item 1</li>
     <li>Item 2</li>
     <li>Item 3</li>
 </ul>
</div>
```
在这个 `<div>` 元素上调用 outerHTML 会返回相同的字符串，包括 `<div>` 本身。注意，浏览器因解
析和解释 HTML 代码的机制不同，返回的字符串也可能不同。

如果使用 outerHTML 设置 HTML，比如：
```javascript
div.outerHTML = "<p>This is a paragraph.</p>";
```
则会得到与执行以下脚本相同的结果：
```javascript
let p = document.createElement("p");
p.appendChild(document.createTextNode("This is a paragraph."));
div.parentNode.replaceChild(p, div);
```
新的`<p>`元素会取代 DOM 树中原来的`<div>`元素。

#### [8.4 insertAdjacentHTML()与 insertAdjacentText()](#)
关于插入标签的最后两个新增方法是 insertAdjacentHTML()和 insertAdjacentText()。
这两个方法最早源自IE，它们都接收两个参数，要插入标记的位置和要插入的 HTML 或文本，第一个参数必须是下列值中的一个：

- "beforebegin"，插入当前元素前面，作为前一个同胞节点；
- "afterbegin"，插入当前元素内部，作为新的子节点或放在第一个子节点前面；
- "beforeend"，插入当前元素内部，作为新的子节点或放在最后一个子节点后面；
- "afterend"，插入当前元素后面，作为下一个同胞节点。

注意这几个值是不区分大小写的。第二个参数会作为 HTML 字符串解析（与 innerHTML 和
outerHTML 相同）或者作为纯文本解析（与 **innerText** 和 **outerText** 相同）。如果是 HTML，则会
在解析出错时抛出错误。下面展示了基本用法：
```javascript
// 作为前一个同胞节点插入
element.insertAdjacentHTML("beforebegin", "<p>Hello world!</p>");
element.insertAdjacentText("beforebegin", "Hello world!");
// 作为第一个子节点插入
element.insertAdjacentHTML("afterbegin", "<p>Hello world!</p>");
element.insertAdjacentText("afterbegin", "Hello world!");
// 作为最后一个子节点插入
element.insertAdjacentHTML("beforeend", "<p>Hello world!</p>");
element.insertAdjacentText("beforeend", "Hello world!");
// 作为下一个同胞节点插入
element.insertAdjacentHTML("afterend", "<p>Hello world!</p>"); element.
insertAdjacentText("afterend", "Hello world!"); 
```

#### [8.5 内存与性能问题](#)
使用这些属性当然有其方便之处，特别是 innerHTML。一般来讲，插入大量的新 HTML 使用
innerHTML 比使用多次 DOM 操作创建节点再插入来得更便捷。
```javascript
for (let value of values){
    ul.innerHTML += '<li>${value}</li>'; // 别这样做！
} 
```
这段代码效率低，因为每次迭代都要设置一次 innerHTML。不仅如此，每次循环还要先读取innerHTML，也就是说循环一次要访问两次 innerHTML。
```javascript
let itemsHtml = "";
for (let value of values){
    itemsHtml += '<li>${value}</li>';
}
ul.innerHTML = itemsHtml; 
```
这样修改之后效率就高多了，因为只有对 innerHTML 的一次赋值。当然，像下面这样一行代码也可以搞定：
```javascript
ul.innerHTML = values.map(value => '<li>${value}</li>').join(''); 
```

### [9. 跨站点脚本](#)
尽管 innerHTML 不会执行自己创建的 `<script>` 标签，但仍然向恶意用户暴露了很大的攻击面，因
为通过它可以毫不费力地创建元素并执行 onclick 之类的属性。

如果页面中要使用用户提供的信息，则不建议使用 innerHTML。与使用 innerHTML 获得的方便相
比，防止 XSS 攻击更让人头疼。此时一定要隔离要插入的数据，在插入页面前必须毫不犹豫地使用相关的库对它们进行转义。

### [10. 滚动](#)


#### [10.1 scrollIntoView](#)
DOM 规范中没有涉及的一个问题是如何滚动页面中的某个区域。为填充这方面的缺失，不同浏览
器实现了不同的控制滚动的方式。在所有这些专有方法中，HTML5 选择了标准化 scrollIntoView()。

scrollIntoView()方法存在于所有 HTML 元素上，可以滚动浏览器窗口或容器元素以便包含元素进入视口。这个方法的参数如下：

- **alignToTop** 是一个布尔值。
  - true：窗口滚动后元素的顶部与视口顶部对齐。
  - false：窗口滚动后元素的底部与视口底部对齐。
- **scrollIntoViewOptions** 是一个选项对象。
  - behavior：定义过渡动画，可取的值为"smooth"和"auto"，默认为"auto"。
  - block：定义垂直方向的对齐，可取的值为"start"、"center"、"end"和"nearest"，默认为 "start"。
  - inline：定义水平方向的对齐，可取的值为"start"、"center"、"end"和"nearest"，默认为 "nearest"。
- 不传参数等同于 **alignToTop** 为 true。

```javascript
// 确保元素可见
document.forms[0].scrollIntoView();
// 同上
document.forms[0].scrollIntoView(true);
document.forms[0].scrollIntoView({block: 'start'});
// 尝试将元素平滑地滚入视口
document.forms[0].scrollIntoView({behavior: 'smooth', block: 'start'}); 
```
这个方法可以用来在页面上发生某个事件时引起用户关注, 把 `焦点` 设置到一个元素上也会导致浏览器将元素滚动到可见位置。

如前所述，滚动是 HTML5 之前 DOM 标准没有涉及的领域。
虽然 HTML5 把 scrollIntoView()标准化了，但不同浏览器中仍然有其他专有方法。

比如，scrollIntoViewIfNeeded()作为HTMLElement 类型的扩展可以在所有元素上调用。

scrollIntoViewIfNeeded(alingCenter)会在 元素不可见的情况下，将其滚动到窗口或包含窗口中，使其可见；

如果已经在视口中可见，则这个方法什么也不做。如果将可选的参数 alingCenter 设置为 true，则浏览器会尝试将其放在视口中央。Safari、Chrome 和 Opera 实现了这个方法。

下面使用 scrollIntoViewIfNeeded()方法的一个例子：
```javascript
// 如果不可见，则将元素可见
document.images[0].scrollIntoViewIfNeeded();
```
考虑到 scrollIntoView()是唯一一个所有浏览器都支持的方法，所以只用它就可以了。

### [11. 专有扩展](#)
除了已经标准化的，各家浏览器还有很多未被标准化的专有扩展。这并不意味着它们将来不会被纳入标准，只不过在本书编写时，它们还只是由部分浏览器专有和采用。

#### [11.1 children 属性](#)
IE9 之前的版本与其他浏览器在处理空白文本节点上的差异导致了 children 属性的出现。
children 属性是一个 HTMLCollection，只包含元素的 Element 类型的子节点。

```javascript
let childCount = element.children.length;
let firstChild = element.children[0]; 
```

#### [11.2 contains() 方法](#)
DOM 编程中经常需要确定一个元素是不是另一个元素的后代。contains()方法应该在要搜索的祖先元素上调
用，参数是待确定的目标节点。

如果目标节点是被搜索节点的后代，contains()返回 true，否则返回 false。
```javascript
console.log(document.documentElement.contains(document.body)); // true 
```
这个例子测试 `<html>` 元素中是否包含 `<body>` 元素，在格式正确的 HTML 中会返回 true。

另外，使用 DOM Level 3 的 **compareDocumentPosition**()方法也可以确定节点间的关系,这个
方法会返回表示两个节点关系的位掩码。

| 掩 码   | 节点关系                       |
|:------|:---------------------------|
| 0x1   | 断开（传入的节点不在文档中）             |
| 0x2   | 领先（传入的节点在 DOM 树中位于参考节点之前）  |
| 0x4   | 随后（传入的节点在 DOM 树中位于参考节点之后）  |
| 0x8   | 包含（传入的节点是参考节点的祖先）          |
| 0x10  | 被包含（传入的节点是参考节点的后代）         |

要模仿 contains()方法，就需要用到掩码 16（0x10）。**compareDocumentPosition**()方法的结
果可以通过按位与来确定参考节点是否包含传入的节点，比如：
```javascript
let result = document.documentElement.compareDocumentPosition(document.body);
console.log(!!(result & 0x10));
```
以上代码执行后 result 的值为 20（或 0x14，其中 0x4 表示“随后”，加上 0x10“被包含”）。对
result 和 0x10 应用按位与会返回非零值，而两个叹号将这个值转换成对应的布尔值。
IE9 及之后的版本，以及所有现代浏览器都支持 **contains**() 和 **compareDocumentPosition**()方法。
