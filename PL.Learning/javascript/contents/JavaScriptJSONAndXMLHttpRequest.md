## [JavaScriptJSON和网络请求](#)
**介绍**：JSON是JavaScript的严格子集，利用JavaScript中的几种模式来表示结构化数据，XMLHttpRequest网络请求。

---

- [1. JSON 序列化选项](#1-json-序列化选项)
- [2. Ajax XmlHttpRequest](#2-ajax-xmlhttprequest)

---

### [1. JSON 序列化选项](#)
早期的JSON解析器基本上就相当于JavaScript的eval()函数。JSON对象有两个方法：**stringify**()和**parse**()。在简单的情况下，这两个方法分别可以将
JavaScript序列化为JSON 字符串，以及将JSON解析为原生JavaScript值。

```javascript
let book = {
 title: "Professional JavaScript",
 authors: [
 "Nicholas C. Zakas",
 "Matt Frisbie"
 ],
 edition: 4,
 year: 2017
};
let jsonText = JSON.stringify(book); 

//{"title":"Professional JavaScript","authors":["Nicholas C. Zakas","Matt Frisbie"],"edition":4,"year":2017}
```
这个例子使用 JSON.stringify()把一个 JavaScript 对象序列化为一个 JSON 字符串，保存在变量jsonText 中。
默认情况下，JSON.stringify()会输出不包含空格或缩进的 JSON 字符串，因此jsonText 的值是这样的：

JSON 字符串可以直接传给 JSON.parse()，然后得到相应的 JavaScript 值。比如，可以使用以下代码创建与 book 对象类似的新对象：
```javascript
let bookCopy = JSON.parse(jsonText); 
```
注意，book 和 bookCopy 是两个完全不同的对象，没有任何关系。但是它们拥有相同的属性和值。
如果给 JSON.parse()传入的 JSON 字符串无效，则会导致抛出错误。

#### [1.1 过滤结果](#)
如果第二个参数是一个数组，那么 JSON.stringify()返回的结果只会包含该数组中列出的对象属性。比如下面的例子：
```javascript
let book = {
    title: "Professional JavaScript",
    authors: [
        "Nicholas C. Zakas",
        "Matt Frisbie"
    ],
    edition: 4,
    year: 2017
};
let jsonText = JSON.stringify(book, ["title", "edition"]);

console.log(jsonText);
//{"title":"Professional JavaScript","edition":4}
```
如果第二个参数是一个函数，则行为又有不同。提供的函数接收两个参数：属性名（key）和属性值（value）。
可以根据这个 key 决定要对相应属性执行什么操作。这个 key 始终是字符串，只是在值不属于某个键/值对时会是空字符串。

为了改变对象的序列化，返回的值就是相应 key 应该包含的结果。注意，返回 undefined 会导致属性被忽略。下面看一个例子：
```javascript
let book = {
    title: "Professional JavaScript",
    authors: [
        "Nicholas C. Zakas",
        "Matt Frisbie"
    ],
    edition: 4,
    year: 2017
};
let jsonText = JSON.stringify(book, (key, value) => {
    switch(key) {
        case "authors":
            return value.join(",")
        case "year":
            return 5000;
        case "edition":
            return undefined;
        default:
            return value;
    }
});

console.log(jsonText);
//{"title":"Professional JavaScript","authors":"Nicholas C. Zakas,Matt Frisbie","year":5000}
```

#### [1.2 字符串缩进](#)
JSON.stringify()方法的第三个参数控制缩进和空格。在这个参数是数值时，表示每一级缩进的
空格数。例如，每级缩进 4 个空格，可以这样：

```javascript
let book = {
    title: "Professional JavaScript",
    authors: [
        "Nicholas C. Zakas",
        "Matt Frisbie"
    ],
    edition: 4,
    year: 2017
};
let jsonText = JSON.stringify(book, null, 4);

console.log(jsonText);
/*
{
    "title": "Professional JavaScript",
    "authors": [
        "Nicholas C. Zakas",
        "Matt Frisbie"
    ],
    "edition": 4,
    "year": 2017
}
* */
```
也可以将缩进字符设置为 Tab 或任意字符，如两个连字符 `--`。
```javascript
let jsonText = JSON.stringify(book, null, "--" );

/*
{
    --"title": "Professional JavaScript",
    --"authors": [
    ----"Nicholas C. Zakas",
    ----"Matt Frisbie"
    --],
    --"edition": 4,
    --"year": 2017
} */
```

#### [1.3 toJSON()方法](#)
有时候，对象需要在 JSON.stringify()之上自定义 JSON 序列化。此时，可以在要序列化的对象
中添加 toJSON()方法，序列化时会基于这个方法返回适当的 JSON 表示。事实上，原生 Date 对象就
有一个 toJSON()方法，能够自动将 JavaScript 的 Date 对象转换为 ISO 8601 日期字符串（本质上与在
Date 对象上调用 toISOString()方法一样）。

```javascript

let book = {
    title: "Professional JavaScript",
    authors: [
        "Nicholas C. Zakas",
        "Matt Frisbie"
    ],
    edition: 4,
    year: 2017,
    toJSON: function() {
        return {
            me: this.authors[0],
            year: this.year,
        };
    }
};
let jsonText = JSON.stringify(book, null, 2);

console.log(jsonText);
/*
{
  "me": "Nicholas C. Zakas",
  "year": 2017
}
* */
```
toJSON()方法可以与过滤函数一起使用，因此理解不同序列化流程的顺序非常重要。在把对象传
给 JSON.stringify()时会执行如下步骤。
* (1) 如果可以获取实际的值，则调用 toJSON()方法获取实际的值，否则使用默认的序列化。
* (2) 如果提供了第二个参数，则应用过滤。传入过滤函数的值就是第(1)步返回的值。
* (3) 第(2)步返回的每个值都会相应地进行序列化。
* 如果提供了第三个参数，则相应地进行缩进。

理解这个顺序有助于决定是创建 toJSON()方法，还是使用过滤函数，抑或是两者都用

#### [1.4 解析选项](#)
JSON.parse()方法也可以接收一个额外的参数，这个函数会针对每个键/值对都调用一次。为区别
于传给 JSON.stringify()的起过滤作用的替代函数（replacer），这个函数被称为还原函数（reviver）。
实际上它们的格式完全一样，即还原函数也接收两个参数，属性名（key）和属性值（value），另外也
需要返回值。

如果还原函数返回 undefined，则结果中就会删除相应的键。如果返回了其他任何值，则该值就
会成为相应键的值插入到结果中。还原函数经常被用于把日期字符串转换为 Date 对象。例如：

```javascript
let book = {
 title: "Professional JavaScript",
 authors: [
 "Nicholas C. Zakas",
 "Matt Frisbie"
 ],
 edition: 4,
 year: 2017,
 releaseDate: new Date(2017, 11, 1)
};
let jsonText = JSON.stringify(book);
let bookCopy = JSON.parse(jsonText,
 (key, value) => key == "releaseDate" ? new Date(value) : value);
```

### [2. Ajax XMLHttpRequest](#)
IE5 是第一个引入 XHR 对象的浏览器。这个对象是通过 ActiveX 对象实现并包含在 MSXML 库中的。
为此，XHR 对象的 3 个版本在浏览器中分别被暴露为 MSXML2.XMLHttp、MSXML2.XMLHttp.3.0 和 MXSML2.XMLHttp.6.0。

所有现代浏览器都通过 [XMLHttpRequest](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 构造函数原生支持 XHR 对象：
```javascript
let xhr = new XMLHttpRequest(); 
```

#### [2.1 使用 XHR](#)
使用 XHR 对象首先要调用 open()方法，这个方法接收 3 个参数：**请求类型**（"get"、"post"等）、**请求 URL**，
**以及表示请求是否异步的布尔值**。下面是一个例子：
```javascript
xhr.open("get", "example.php", false); 
```
这行代码就可以向 example.php 发送一个同步的 GET 请求。关于这行代码需要说明几点。首先，这
里的 URL 是相对于代码所在页面的，当然也可以使用绝对 URL。其次，调用 open()不会实际发送请
求，只是为发送请求做好准备。

要发送定义好的请求，必须像下面这样调用 send()方法：
```javascript
xhr.open("get", "example.txt", false);
xhr.send(null); 
```
send()方法接收一个参数，是作为请求体发送的数据。如果不需要发送请求体，则必须传 null，因为这个参数在某些浏览器中是必需的。调用 send()之后，请求就会发送到服务器。

因为这个请求是同步的，所以 JavaScript 代码会等待服务器响应之后再继续执行。收到响应后，XHR对象的以下属性会被填充上数据。
- responseText：作为响应体返回的文本。
- responseXML：如果响应的内容类型是"text/xml"或"application/xml"，那就是包含响应数据的 XML DOM 文档。
- status：响应的 HTTP 状态。
- statusText：响应的 HTTP 状态描述。

收到响应后，第一步要检查 status 属性以确保响应成功返回。一般来说，HTTP 状态码为 2xx 表示成功。
```javascript
xhr.open("get", "example.txt", false);
xhr.send(null);
if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
    alert(xhr.responseText);
} else {
    alert("Request was unsuccessful: " + xhr.status);
} 
```
以上代码可能显示服务器返回的内容，也可能显示错误消息，取决于 HTTP 响应的状态码。为确定下一步该执
行什么操作，最好检查 status 而不是 statusText 属性，因为后者已经被证明在跨浏览器的情况下不可靠。
无论是什么响应内容类型，responseText 属性始终会保存响应体，而 responseXML则对于非 XML 数据是 null。

虽然可以像前面的例子一样发送同步请求，但多数情况下最好使用异步请求，这样可以不阻塞
JavaScript 代码继续执行。XHR 对象有一个 readyState 属性，表示当前处在请求/响应过程的哪个阶段。
这个属性有如下可能的值。

|值| 	状态	              |描述|
|:---|:------------------|:---|
|0	| `UNSENT`          |	代理被创建，但尚未调用 open() 方法。|
|1	| `OPENED`            |	open() 方法已经被调用。|
|2	| `HEADERS_RECEIVED`  |	send() 方法已经被调用，并且头部和状态已经可获得。|
|3	| `LOADING`           |	下载中；responseText 属性已经包含部分数据。|
|4	| `DONE`              |	下载操作已完成。|

```javascript
var xhr = new XMLHttpRequest();
console.log("UNSENT", xhr.readyState); // readyState 为 0

xhr.open("GET", "/api", true);
console.log("OPENED", xhr.readyState); // readyState 为 1

xhr.onprogress = function () {
  console.log("LOADING", xhr.readyState); // readyState 为 3
};

xhr.onload = function () {
  console.log("DONE", xhr.readyState); // readyState 为 4
};

xhr.send(null);
```

每次 readyState 从一个值变成另一个值，都会触发 readystatechange 事件。可以借此机会检
查 readyState 的值。一般来说，我们唯一关心的 readyState 值是 4，表示数据已就绪。为保证跨浏
览器兼容，onreadystatechange 事件处理程序应该在调用 open()之前赋值。来看下面的例子：
```javascript
let xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
    if (xhr.readyState == 4) {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
            alert(xhr.responseText);
        } else {
            alert("Request was unsuccessful: " + xhr.status);
        }
    }
};
xhr.open("get", "example.txt", true);
xhr.send(null); 
```
以上代码使用 DOM Level 0 风格为 XHR 对象添加了事件处理程序，因为并不是所有浏览器都支持
DOM Level 2 风格。与其他事件处理程序不同，onreadystatechange 事件处理程序不会收到 event
对象。在事件处理程序中，必须使用 XHR 对象本身来确定接下来该做什么。 

在收到响应之前如果想取消异步请求，可以调用 abort()方法：
```javascript
xhr.abort();
```
调用这个方法后，XHR 对象会停止触发事件，并阻止访问这个对象上任何与响应相关的属性。中
断请求后，应该取消对 XHR 对象的引用。由于内存问题，不推荐重用 XHR 对象。

> 由于 onreadystatechange 事件处理程序的作用域问题，这个例子在 onreadystatechange 事件处理程序中使用了 xhr 对象而不是 this 对象。使用 this 可能导致
> 功能失败或导致错误，取决于用户使用的是什么浏览器。因此还是使用保存 XHR 对象的变量更保险一些。

#### [2.2 HTTP 头部](#)
每个 HTTP 请求和响应都会携带一些头部字段，这些字段可能对开发者有用。XHR 对象会通过一些方法暴露与请求和响应相关的头部字段。

默认情况下，XHR 请求会发送以下头部字段。
- Accept：浏览器可以处理的内容类型。
- Accept-Charset：浏览器可以显示的字符集。
- Accept-Encoding：浏览器可以处理的压缩编码类型。
- Accept-Language：浏览器使用的语言。
- Connection：浏览器与服务器的连接类型。
- Cookie：页面中设置的 Cookie。
- Host：发送请求的页面所在的域。
- Referer：发送请求的页面的 URI。注意，这个字段在 HTTP 规范中就拼错了，所以考虑到兼容性也必须将错就错。（正确的拼写应该是 Referrer。）
- User-Agent：浏览器的用户代理字符串。

虽然不同浏览器发送的确切头部字段可能各不相同，但这些通常都是会发送的。如果需要发送额外
的请求头部，可以使用 **setRequestHeader**(key,value)方法。这个方法接收两个参数：头部字段的名称和值。
```javascript
let xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
if (xhr.readyState == 4) {
  if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
    alert(xhr.responseText);
  } else {
    alert("Request was unsuccessful: " + xhr.status);
  }
}
};
xhr.open("get", "example.php", true);
xhr.setRequestHeader("MyHeader", "MyValue");
xhr.send(null);
```
服务器通过读取自定义头部可以确定适当的操作。自定义头部一定要区别于浏览器正常发送的头部，
否则可能影响服务器正常响应。有些浏览器允许重写默认头部，有些浏览器则不允许。

可以使用 getResponseHeader()方法从 XHR 对象获取响应头部，只要传入要获取头部的名称即
可。如果想取得所有响应头部，可以使用 getAllResponseHeaders()方法，这个方法会返回包含所
有响应头部的字符串。下面是调用这两个方法的例子：

```javascript
let myHeader = xhr.getResponseHeader("MyHeader");
let allHeaders = xhr.getAllResponseHeaders(); 
```
服务器可以使用头部向浏览器传递额外的结构化数据。getAllResponseHeaders()方法通常返回类似如下的字符串：
```
Date: Sun, 14 Nov 2004 18:04:03 GMT
Server: Apache/1.3.29 (Unix)
Vary: Accept
X-Powered-By: PHP/4.3.8
Connection: close
Content-Type: text/html; charset=iso-8859-1 
```
通过解析以上头部字段的输出，就可以知道服务器发送的所有头部，而不需要单独去检查了。

#### [2.3 GET 请求](#)
最常用的请求方法是 GET 请求，用于向服务器查询某些信息。必要时，需要在 GET 请求的 URL
后面添加查询字符串参数。对 XHR 而言，查询字符串必须正确编码后添加到 URL 后面，然后再传给open()方法。

发送 GET 请求最常见的一个错误是查询字符串格式不对。查询字符串中的每个名和值都必须使用
encodeURIComponent()编码，所有 `名`/`值`对必须以和号（`&`）分隔，如下面的例子所示：
```javascript
xhr.open("get", "example.php?name1=value1&name2=value2", true); 
```
可以使用以下函数将查询字符串参数添加到现有的 URL 末尾：
```javascript
function addURLParam(url, name, value) {
    url += (url.indexOf("?") == -1 ? "?" : "&");
    url += encodeURIComponent(name) + "=" + encodeURIComponent(value);
    return url;
}

let url = "example.php";
// 添加参数
url = addURLParam(url, "name", "Nicholas");
url = addURLParam(url, "book", "Professional JavaScript");
// 初始化请求
xhr.open("get", url, false);
```

#### [2.4 POST 请求](#)
POST 请求，用于向服务器发送应该保存的数据。每个 POST 请求都应该在请求体中携带提交的数据，而 GET 请求则不然。POST 请求的请求体可以包含非常多的数据，而且数据
可以是任意格式。要初始化 POST 请求，open()方法的第一个参数要传"post"，比如：
```javascript
xhr.open("post", "example.php", true); 
```
默认情况下，对服务器而言，POST 请求与提交表单是不一样的。服务器逻辑需要读取原始 POST
数据才能取得浏览器发送的数据。不过，可以使用 XHR 模拟表单提交。为此，第一步需要把 ContentType 头部设置为"application/x-www-formurlencoded"，这是提交表单时使用的内容类型。第二
步是创建对应格式的字符串。POST 数据此时使用与查询字符串相同的格式。

如果网页中确实有一个表单需要序列化并通过 XHR 发送到服务器，则可以使用[serialize()](./JavaScriptFormScript.md#6-表单序列化)函数来创建相应的字符串，如下所示：


接下来就是要给 send()方法传入要发送的数据。因为 XHR 最初主要设计用于发送 XML，所以可
以传入序列化之后的 XML DOM 文档作为请求体。当然，也可以传入任意字符串。
```javascript
  function submitData() {
    let xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
          alert(xhr.responseText);
        } else {
          alert("Request was unsuccessful: " + xhr.status);
        }
      }
    };

    xhr.open("post", "https://tr.mt.com/chat/user", true);
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    let form = document.getElementById("user-info");
    xhr.send(serialize(form));
  }
```
在这个函数中，来自 ID 为"user-info"的表单中的数据被序列化之后发送给了服务器。nest.js后段如何使用代码取得 POST 的数据。
```typescript
import { Controller, Post, Body } from '@nestjs/common';

@Controller('user')
export class UserController {
  @Post()
  createUser(@Body() formData: any) {
    // formData 会自动解析为键值对对象
    console.log('Received form data:', formData);
    return { success: true, data: formData };
  }
}
```