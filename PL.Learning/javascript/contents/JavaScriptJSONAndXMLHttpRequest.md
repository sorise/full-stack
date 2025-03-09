## [JavaScriptJSON和网络请求](#)
**介绍**：JSON是JavaScript的严格子集，利用JavaScript中的几种模式来表示结构化数据，XMLHttpRequest网络请求。

---

- [1. JSON 序列化选项](#1-json-序列化选项)

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

所有现代浏览器都通过 XMLHttpRequest 构造函数原生支持 XHR 对象：
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
send()方法接收一个参数，是作为请求体发送的数据。如果不需要发送请求体，则必须传 null，
因为这个参数在某些浏览器中是必需的。调用 send()之后，请求就会发送到服务器。

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

- 0：未初始化（Uninitialized）。尚未调用 open()方法。
- 1：已打开（Open）。已调用 open()方法，尚未调用 send()方法。
- 2：已发送（Sent）。已调用 send()方法，尚未收到响应。
- 3：接收中（Receiving）。已经收到部分响应。
- 4：完成（Complete）。已经收到所有响应，可以使用了。

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







