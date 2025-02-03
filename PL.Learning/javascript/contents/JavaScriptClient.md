## [JavaScript 客户端检测](#)
**介绍**：客户端检测是一种补救措施，也是一种行之有效的开发策略。主要用来规避或者突破不同浏览器之间的差异。

----

 - [1. JavaScript 能力检测](#1-javascript-能力检测)
 - [2. 用户代理检测](#2-用户代理检测)
 - [3. 软件与硬件检测](#3-软件与硬件检测)

----

### [1. JavaScript 能力检测](#)
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

用户代理字符串检测脚本，包括检测呈现引擎、平台、Window操作系统、移动设备和游戏系统。
```javascript
//客户端检测之
//用户代理检测（这个是最后的手段，优先能力检测和怪癖检测）
var client = function(){
	//呈现引擎
	var engine = {
		ie: 0,
		gecko: 0,
		webkit: 0,
		khtml: 0,
		opera: 0,

		//完整的版本号
		ver: null
	};

	//浏览器
	var browser = {
		//主要浏览器
		ie: 0,
		firefox: 0,
		safari: 0,
		konq: 0,
		opera: 0,
		chrome: 0,
		//具体的版本号
		ver: null
	};

	 //平台、设备和操作系统
	 var system = {
	 	win:false,
	 	mac:false,
	 	x11:false,

	 	//移动设备
	 	iphone:false,
	 	ipod:false,
	 	ipad:false,
	 	ios:false,
	 	android:false,
	 	nokiaN:false,
	 	winMobile:false,

	 	//游戏系统
	 	wii:false,
	 	ps:false
	 };

	 //检测呈现引擎和浏览器
	 var ua = navigator.userAgent;
	 if(window.opera){
	 	engine.ver = browser.ver = window.opera.version();
	 	engine.opera = browser.opera = parseFloat(engine.ver);
	 }else if(/AppleWebKit\/(\S+)/.test(ua)){
	 	engine.ver = RegExp["$1"];
	 	engine.webkit = parseFloat(engine.ver);

	 	//确定是chrome、还是safari
	 	if(/Chrome\/(\S+)/.test(ua)){
	 		browser.ver = RegExp["$1"];
	 		browser.chrome = parseFloat(browser.ver);
	 	}else if(/Version\/(\S+)/.test(ua)){
	 		browser.ver = RegExp["$1"];
	 		browser.safariv = parseFloat(browser.ver);
	 	}else{
	 		//近似地确定版本号
	 		var safariVersion = 1;
	 		if(engine.webkit < 100){
	 			safariVersion = 1;
	 		}else if (engine.webkit < 312){
	 			safariVersion = 1.2;
	 		} else if (engine.webkit < 412){
	 			safariVersion = 1.3;
	 		}else{
	 			safariVersion = 2;
	 		}
	 		browser.safari = browser.ver = safariVersion;
	 	}
	 }else if(/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)){
	 	engine.ver = browser.ver = RegExp["$1"];
	 	engine.khtml = browser.konq = parseFloat(engine.ver);
	 }else if(/rv:([^\)]+)\) Gecko\/\d{8}/.test(ua)){
	 	engine.var = RegExp["$1"];
	 	engine.gecko = parseFloat(engine.ver);

	 	//确认是不是Firefox
	 	if(/Firefox\/(\S+)/.test(ua)){
	 		browser.ver= RegExp["$1"];
	 		browser.firefox = parseFloat(browser.ver);
	 	}
	 }else if(/MSIE ([^;]+)/.test(ua)){
	 	engine.ver = browser.ver = RegExp["$1"];
	 	engine.ie = browser.ie = parseFloat(engine.ver);
	 }

	 //检测浏览器
	 browser.ie = engine.ie;
	 browser.opera = engine.opera;

	 //检测平台
    var p = navigator.platform;
    system.win = p.indexOf("Win") == 0;
    system.mac = p.indexOf("Mac") == 0;
    system.x11 = (p == "X11") || (p.indexOf("Linux") == 0);

	 //检测Window操作系统
	 if (system.win){
	 	if (/Win(?:dows )?([^do]{2})\s?(\d+\.\d+)?/.test(ua)){
	 		if (RegExp["$1"] == "NT"){
	 			switch(RegExp["$2"]){
	 				case "5.0":
	 				system.win = "2000";
	 				break;
	 				case "5.1":
	 				system.win = "XP";
	 				break;
	 				case "6.0":
	 				system.win = "Vista";
	 				break;
	 				case "6.1":
	 				system.win = "7";
	 				break;
	 				default:
	 				system.win = "NT";
	 				break;                
	 			}                            
	 		} else if (RegExp["$1"] == "9x"){
	 			system.win = "ME";
	 		} else {
	 			system.win = RegExp["$1"];
	 		}
	 	}
	 }

	 //移动设备
	 system.iphone = ua.indexOf("iPhone") > -1;
	 system.ipod = ua.indexOf("iPod") > -1;
	 system.ipad = ua.indexOf("iPad") > -1;
	 system.nokiaN = ua.indexOf("NokiaN") > -1;

	 //windo mobile
	 if(system.win =="CE"){
	 	system.winMobile = system.win;
	 }else if(system.win == "Ph"){
	 	if(/Window Phone OS (\d+.\d+)/.test(ua)){;
	 		system.win = "Phone";
	 		system.winMobile = parseFloat(RegExp["$1"]);
	 	}
	 }

	 //检测iOS版本
	 if (system.mac && ua.indexOf("Mobile") > -1) {
	 	if(/CPU (?:iPhone )?OS (\d+_\d)/.test(ua)){
	 		system.ios = parseFloat(RegExp.$1.replace("_","."));
	 	}else{
			system.ios = 2;//不能真正检测出来，所以只能猜测
		}
	}

	//检测Android版本
	if(/Android (\d+\.\d+)/.test(ua)){
		system.android = parseFloat(RegExp.$1);
	}

	//游戏系统
	system.wii = ua.indexOf("Wii") > -1;
	system.ps = /playstation/i.test(ua);

	//返回这些对象
	return {
		engine:engine,
		browser:browser,
		system:system,
	};
}();
```

使用实例，记得先引用上面的js脚本，（因为在html文档中用了很多嵌入式的script脚本，里面的client函数引用是在上面的js脚本中声明初始化的）：

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>UserAgentDetectionExample</title>
	<script type="text/javascript" src=""></script>
</head>
<body>
	<h2>Rendering Engines</h2>
	<ul>
		<li>client.engine.ie = <script>document.write(client.engine.ie);</script></li>
		<li>client.engine.webkit = <script>document.write(client.engine.webkit);</script></li>
		<li>client.engine.gecko = <script>document.write(client.engine.gecko);</script></li>
		<li>client.engine.khtml = <script>document.write(client.engine.khtml);</script></li>
		<li>client.engine.opera = <script>document.write(client.engine.opera);</script></li>
		<li>client.engine.ver = <script>document.write(client.engine.ver);</script></li>

	</ul>
	<h2>Browsers</h2>
	<ul>
		<li>client.browser.ie = <script>document.write(client.browser.ie);</script></li>
		<li>client.browser.safari = <script>document.write(client.browser.safari);</script></li>
		<li>client.browser.firefox = <script>document.write(client.browser.firefox);</script></li>
		<li>client.browser.konq = <script>document.write(client.browser.konq);</script></li>
		<li>client.browser.opera = <script>document.write(client.browser.opera);</script></li>
		<li>client.browser.chrome = <script>document.write(client.browser.chrome);</script></li>
		<li>client.browser.ver = <script>document.write(client.browser.ver);</script></li>
	</ul>
	<h2>System</h2>
	<ul>
		<li>client.system.win = <script>document.write(client.system.win);</script></li>
		<li>client.system.mac = <script>document.write(client.system.mac);</script></li>
		<li>client.system.x11 = <script>document.write(client.system.x11);</script></li>
		<li>client.system.iphone = <script>document.write(client.system.iphone);</script></li>
		<li>client.system.ipod = <script>document.write(client.system.ipod);</script></li>
		<li>client.system.ipad = <script>document.write(client.system.ipad);</script></li>
		<li>client.system.ios = <script>document.write(client.system.ios);</script></li>
		<li>client.system.android = <script>document.write(client.system.android);</script></li>
		<li>client.system.nokiaN = <script>document.write(client.system.nokiaN);</script></li>
		<li>client.system.winMobile = <script>document.write(client.system.winMobile);</script></li>
		<li>client.system.wii = <script>document.write(client.system.wii);</script></li>
		<li>client.system.ps = <script>document.write(client.system.ps);</script></li>    
	</ul>    
</body>
</html>
```

### [3. 软件与硬件检测](#)
现代浏览器提供了一组与页面执行环境相关的信息，包括浏览器、操作系统、硬件和周边设备信息。 这些属性可以通过暴露在 window.navigator 上的一组 API 获得。不过，这些 API 的跨浏览器支持还不够好，远未达到标准化的程度。

这个 API 可以查询宿主系统并尽可能精确地返回设备的位置信息。根据宿主系统的硬件和配置，返回结果的精度可能不一样。手机 GPS 的坐标系统可能具有极高的精度，而 IP 地址的精度就要差很多。
根据 Geolocation API 规范。

> 地理位置信息的主要来源是 GPS 和 IP 地址、射频识别（RFID）、Wi-Fi 及蓝牙 Mac 地址、
GSM/CDMA 蜂窝 ID 以及用户输入等信息。

> 浏览器也可能会利用 Google Location Service（Chrome 和 Firefox）等服务确定位置。
有时候，你可能会发现自己并没有 GPS，但浏览器给出的坐标却非常精确。浏览器会收集
所有可用的无线网络，包括 Wi-Fi 和蜂窝信号。拿到这些信息后，再去查询网络数据库。
这样就可以精确地报告出你的设备位置。

要获取浏览器当前的位置，可以使用 getCurrentPosition()方法。这个方法返回一个
Coordinates 对象，其中包含的信息不一定完全依赖宿主系统的能力：

```javascript
// getCurrentPosition()会以 position 对象为参数调用传入的回调函数
navigator.geolocation.getCurrentPosition((position) => p = position);
```

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

#### [3.2 Geolocation API](#)
navigator.geolocation 属性暴露了 Geolocation API，可以让浏览器脚本感知当前设备的地理位
置。这个 API 只在安全执行环境（通过 HTTPS 获取的脚本）中可用。

#### [3.3 Connection State 和 NetworkInformation API](#)
浏览器会跟踪网络连接状态并以两种方式暴露这些信息：连接事件和 navigator.onLine 属性。
在设备连接到网络时，浏览器会记录这个事实并在 window 对象上触发 online 事件。相应地，当设备断开网络连接后，浏览器会在 window 对象上触发 offline 事件。

```javascript
const connectionStateChange = () => console.log(navigator.onLine);
window.addEventListener('online', connectionStateChange);
window.addEventListener('offline', connectionStateChange);
// 设备联网时：
// true
// 设备断网时：
// false 
```
任何时候，都可以通过 navigator.onLine 属性来确定浏览器的联网状态。这个属性返回一个布尔值，表示浏览器是否联网。

当然，到底怎么才算联网取决于浏览器与系统实现。有些浏览器可能会认为只要连接到局域网就算“在线”，而不管是否真正接入了互联网。

navigator 对象还暴露了 NetworkInformation API，可以通过 navigator.connection 属性使用。
这个 API 提供了一些只读属性，并为连接属性变化事件处理程序定义了一个事件对象。

- downlink：整数，表示当前设备的带宽（以 Mbit/s 为单位），舍入到最接近的 25kbit/s。这个值
可能会根据历史网络吞吐量计算，也可能根据连接技术的能力来计算。
- downlinkMax：整数，表示当前设备最大的下行带宽（以 Mbit/s 为单位），根据网络的第一跳来
确定。因为第一跳不一定反映端到端的网络速度，所以这个值只能用作粗略的上限值。
- effectiveType：字符串枚举值，表示连接速度和质量。这些值对应不同的蜂窝数据网络连接
技术，但也用于分类无线网络，**slow-2g**、**2g**、**3g**、**4g**。 
- rtt：毫秒，表示当前网络实际的往返时间，舍入为最接近的 25 毫秒。这个值可能根据历史网
  络吞吐量计算，也可能根据连接技术的能力来计算。
- type：字符串枚举值，表示网络连接技术。
  - bluetooth：蓝牙。
  - cellular：蜂窝。
  - ethernet：以太网。
  - none：无网络连接。相当于 navigator.onLine === false。
  - mixed：多种网络混合。
  - wifi：Wi-Fi。
  - ...
- saveData：布尔值，表示用户设备是否启用了“节流”（reduced data）模式。
- onchange：事件处理程序，会在任何连接状态变化时激发一个 change 事件。可以通过 navigator.
  connection.addEventListener('change',changeHandler)或 navigator.connection.
  onchange = changeHandler 等方式使用。
 
#### [3.4 电池状态 Battery Status API]g(#)
浏览器可以访问设备电池及充电状态的信息。navigator.getBattery()方法会返回一个期约实
例，解决为一个 BatteryManager 对象。

```javascript
navigator.getBattery().then((b) => console.log(b));
// BatteryManager { ... } 
```
BatteryManager 包含 4 个只读属性，提供了设备电池的相关信息。
* charging：布尔值，表示设备当前是否正接入电源充电。如果设备没有电池，则返回 true。
* chargingTime：整数，表示预计离电池充满还有多少秒。如果电池已充满或设备没有电池，则返回 0。
* dischargingTime：整数，表示预计离电量耗尽还有多少秒。如果设备没有电池，则返回 Infinity。
* level：浮点数，表表示电量百分比。电量完全耗尽返回 0.0，电池充满返回 1.0。如果设备没有电池，则返回 1.0。

这个 API 还提供了 4 个事件属性，可用于设置在相应的电池事件发生时调用的回调函数。可以通过
给 BatteryManager 添加事件监听器，也可以通过给事件属性赋值来使用这些属性。
* onchargingchange
* onchargingtimechange
* ondischargingtimechange
* onlevelchange

```javascript
navigator.getBattery().then((battery) => {
  // 添加充电状态变化时的处理程序
  const chargingChangeHandler = () => console.log('chargingchange');
  battery.onchargingchange = chargingChangeHandler;
  // 或
  battery.addEventListener('chargingchange', chargingChangeHandler);
  // 添加充电时间变化时的处理程序
  const chargingTimeChangeHandler = () => console.log('chargingtimechange');
  battery.onchargingtimechange = chargingTimeChangeHandler;
  // 或
  battery.addEventListener('chargingtimechange', chargingTimeChangeHandler);
  // 添加放电时间变化时的处理程序
  const dischargingTimeChangeHandler = () => console.log('dischargingtimechange');
  battery.ondischargingtimechange = dischargingTimeChangeHandler;
  // 或
  battery.addEventListener('dischargingtimechange', dischargingTimeChangeHandler);
  // 添加电量百分比变化时的处理程序
  const levelChangeHandler = () => console.log('levelchange');
  battery.onlevelchange = levelChangeHandler;
  // 或
  battery.addEventListener('levelchange', levelChangeHandler);
});
```

#### [3.5 硬件](#)
浏览器检测硬件的能力相当有限。不过，navigator 对象还是通过一些属性提供了基本信息。

处理器核心数 **navigator.hardwareConcurrency** 属性返回浏览器支持的逻辑处理器核心数量，包含表示核心
数的一个整数值（如果核心数无法确定，这个值就是 1）。关键在于，这个值表示浏览器可以并行执行的
最大工作线程数量，不一定是实际的 CPU 核心数。

设备内存大小 **navigator.deviceMemory** 属性返回设备大致的系统内存大小，包含单位为 GB 的浮点数（舍入
为最接近的 2 的幂：512MB 返回 0.5，4GB 返回 4）。

最大触点数 **navigator.maxTouchPoints** 属性返回触摸屏支持的最大关联触点数量，包含一个整数值。