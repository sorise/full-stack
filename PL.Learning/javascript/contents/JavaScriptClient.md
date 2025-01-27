## [JavaScript 客户端检测](#)
**介绍**：客户端检测是一种补救措施，也是一种行之有效的开发策略。主要用来规避或者突破不同浏览器之间的差异。

----

 - [1. 能力检测](#)

----

### [1. 能力检测](#)
能力检测（又称特性检测）即在 JavaScript 运行时中使用一套简单的检测逻辑，测试浏览器是否支持某种特性。

```javascript
if (object.propertyInQuestion) {
 // 使用 object.propertyInQuestion
} 
```
比如，IE5 之前的版本中没有 document.getElementById()这个 DOM 方法，但可以通过
document.all 属性实现同样的功能。为此，可以进行如下能力检测：
```javascript
function getElement(id) {
    if (document.getElementById) {
        return document.getElementById(id);
    } else if (document.all) {
        return document.all[id];
    } else {
        throw new Error("No way to retrieve element!");
    }
}
```
这个 getElement()函数的目的是根据给定的 ID 获取元素。因为标准的方式是使用 document.
getElementById()，所以首先测试它。如果这个函数存在（不是 undefined），那就使用这个方法；

其次是必须检测切实需要的特性。某个能力存在并不代表别的能力也存在。比如下面的例子：
```javascript
function getWindowWidth() {
    if (document.all) { // 假设 IE
        return document.documentElement.clientWidth; // 不正确的用法！
    } else {
        return window.innerWidth;
    }
}
```
这个例子展示了不正确的能力检测方式。getWindowWidth()函数首先检测 document.all 是否
存在，如果存在则返回 document.documentElement.clientWidth，理由是 IE8 及更低版本不支持
window.innerWidth。这个例子的问题在于检测到 document.all 存在并不意味着浏览器是 IE。事实，
也可能是某个早期版本的 Opera，既支持 document.all 也支持 windown.innerWidth。

#### [1.1 安全能力检测](#)
能力检测最有效的场景是检测能力是否存在的同时，验证其是否能够展现出预期的行为。

比如我们需要一个排序方法sort,我们就需要sort必须是一个函数，检测方法如下：
```javascript
// 好一些，检测 sort 是不是函数
function isSortable(object) { 
 return typeof object.sort == "function"; 
}
```

#### [1.2 基于能力检测进行浏览器分析](#)
可以按照能力将浏览器归类。如果你的应用程序需要使用特定的浏览器能力，那么最好集中检测所 有能力，而不是等到用的时候再重复检测。

```javascript
// 检测浏览器是否支持 Netscape 式的插件
let hasNSPlugins = !!(navigator.plugins && navigator.plugins.length); 
// 检测浏览器是否具有 DOM Level 1 能力
let hasDOM1 = !!(document.getElementById && document.createElement && 
 document.getElementsByTagName);
```
一项是确定浏览器是否支持 Netscape 式的插件，另一项是检测浏览器是否具有 DOM Level 1 能力。保存在变量中的布尔值可以用在后面的条件语句中，这样比重复检测省事多了。

#### [1.3 检测浏览器](#)
根据不同浏览器独有的行为推断出浏览器的身份。这里故意没有使用 navigator. userAgent 属性，后面会讨论它：
```javascript
class BrowserDetector {
  constructor() {
    // 测试条件编译
    // IE6~10 支持
    this.isIE_Gte6Lte10 = /*@cc_on!@*/ false;
    // 测试 documentMode
    // IE7~11 支持
    this.isIE_Gte7Lte11 = !!document.documentMode;
    // 测试 StyleMedia 构造函数
    // Edge 20 及以上版本支持
    this.isEdge_Gte20 = !!window.StyleMedia;
    // 测试 Firefox 专有扩展安装 API
    // 所有版本的 Firefox 都支持
    this.isFirefox_Gte1 = typeof InstallTrigger !== "undefined";
    // 测试 chrome 对象及其 webstore 属性
    // Opera 的某些版本有 window.chrome，但没有 window.chrome.webstore
    // 所有版本的 Chrome 都支持
    this.isChrome_Gte1 = !!window.chrome && !!window.chrome.webstore;
    // Safari 早期版本会给构造函数的标签符追加"Constructor"字样，如：
    // window.Element.toString(); // [object ElementConstructor]
    // Safari 3~9.1 支持
    this.isSafari_Gte3Lte9_1 = /constructor/i.test(window.Element);
    // 推送通知 API 暴露在 window 对象上
    // 使用默认参数值以避免对 undefined 调用 toString()
    // Safari 7.1 及以上版本支持
    this.isSafari_Gte7_1 = (({ pushNotification = {} } = {}) =>
      pushNotification.toString() == "[object SafariRemoteNotification]")(
      window.safari
    );
    // 测试 addons 属性
    // Opera 20 及以上版本支持
    this.isOpera_Gte20 = !!window.opr && !!window.opr.addons;
  }
  isIE() {
    return this.isIE_Gte6Lte10 || this.isIE_Gte7Lte11;
  }
  isEdge() {
    return this.isEdge_Gte20 && !this.isIE();
  }
  isFirefox() {
    return this.isFirefox_Gte1;
  }
  isChrome() {
    return this.isChrome_Gte1;
  }
  isSafari() {
    return this.isSafari_Gte3Lte9_1 || this.isSafari_Gte7_1;
  }
  isOpera() {
    return this.isOpera_Gte20;
  }
}
```

#### [1.4 能力检测的局限](#)
通过检测一种或一组能力，并不总能确定使用的是哪种浏览器。以下“浏览器检测”代码（或其他 类似代码）经常出现在很多网站中，但都没有正确使用能力检测：
```javascript
// 不要这样做！不够特殊
let isFirefox = !!(navigator.vendor && navigator.vendorSub); 
// 不要这样做！假设太多
let isIE = !!(document.all && document.uniqueID);
```

### [2. 用户代理检测](#)
用户代理检测通过浏览器的用户代理字符串确定使用的是什么浏览器。用户代理字符串包含在每个
HTTP 请求的头部，在 JavaScript 中可以通过 navigator.userAgent 访问。


### [3. 软件与硬件检测](#)
现代浏览器提供了一组与页面执行环境相关的信息，包括浏览器、操作系统、硬件和周边设备信息。 这些属性可以通过暴露在 window.navigator 上的一组 API 获得。不过，这些 API 的跨浏览器支持还不够好，远未达到标准化的程度。

#### [3.1 识别浏览器与操作系统](#)
特性检测和用户代理字符串解析是当前常用的两种识别浏览器的方式。而 navigator 和 screen对象也提供了关于页面所在软件环境的信息。

**navigator.oscpu** navigator.oscpu 属性是一个字符串，通常对应用户代理字符串中操作系统/系统架构相关信息。

比如，Windows 10 上的 Firefox 的 oscpu 属性应该对应于以下部分：
```javascript
console.log(navigator.userAgent);
//"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:58.0) Gecko/20100101 Firefox/58.0"
console.log(navigator.oscpu);
//"Windows NT 10.0; Win64; x64"
```

**navigator.vendor** 通常包含浏览器开发商信息。返回这个字符串是浏览器
```javascript
//例如，Chrome 中的这个 navigator.vendor 属性返回下面的字符串：
console.log(navigator.vendor); // "Google Inc." 
```
**navigator.platform** 通常表示浏览器所在的操作系统。
```javascript
console.log(navigator.platform); // "Win64"
```
**screen.orientation** 属性返回一个 ScreenOrientation 对象，其中包含 Screen Orientation API
定义的屏幕信息。