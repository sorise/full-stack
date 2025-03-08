## [JavaScriptJSON和网络请求](#)
**介绍**：JSON是JavaScript的严格子集，利用JavaScript中的几种模式来表示结构化数据，XMLHttpRequest网络请求。

---

-[](#)

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