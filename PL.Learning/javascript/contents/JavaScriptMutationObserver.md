## [JavaScript MutationObserver](#)
**介绍**：MutationObserver 是 JavaScript 中用于监听 DOM 树变化的接口。通过它，开发者可以监视一个选定元素的属性变化、子节点的变化（添加或删除）、文本内容的变化等。这在现代 Web 开发中非常有用，尤其是在需要对DOM更新做出响应而无需依赖事件处理器的情况下。

-----

- [1. MutationObserver基本概念](#1-mutationobserver基本概念)
- [2. observe()方法](#2-observe---方法)
- [3. disconnect()方法](#3-disconnect---方法)

-----
### [1. MutationObserver基本概念](#)
不久前添加到 DOM 规范中的 MutationObserver 接口，可以在 DOM 被修改时异步执行回调。使
用 MutationObserver 可以观察整个文档、DOM 树的一部分，或某个元素。此外还可以观察元素属性、子节点、文本，或者前三者任意组合的变化。

> 新引进 MutationObserver 接口是为了取代废弃的 MutationEvent。

**要和事件区分开，事件是同步触发，MutationObserver是异步触发** ,其目的是为了应付变动平频繁的特点。

举例来说，如果文档中连续插入1000个 `<p>` 元素，就会连续触发1000个插入事件，执行每个事件的回调函数，这很
可能造成浏览器的卡顿;而 Mutation Observer 完全不同，只在1000个段落都插入结束后才会触发，而且只触发一次。

Mutation Observer 有以下特点:
- 它等待所有脚本任务完成后，才会运行(即异步触发方式)。
- 它把 DOM 变动记录封装成一个数组进行处理，而不是一条条个别处理 DOM 变动。
- 它既可以观察 DOM 的所有类型变动，也可以指定只观察某一类变动。

#### [1.1 构造函数](#)
DOM 规范中的 MutationObserver() 构造函数——是 MutationObserver 接口内容的一部分——创建并返回一个新的观察器，它会在触发指定 DOM 事件时，调用指定的回调函数。
```javascript
var observer = new MutationObserver(callback);
```
回调函数拥有两个参数：一个是描述所有被触发改动的 MutationRecord 对象数组，另一个是调用该函数的 MutationObserver 对象。
```javascript
function callback(mutationList, observer) {
  mutationList.forEach((mutation) => {
    switch (mutation.type) {
      case "childList":
        /* 从树上添加或移除一个或更多的子节点；参见 mutation.addedNodes 与
           mutation.removedNodes */
        break;
      case "attributes":
        /* mutation.target 中某节点的一个属性值被更改；该属性名称在 mutation.attributeName 中，
           该属性之前的值为 mutation.oldValue */
        break;
    }
  });
}
```
调用 observe() 即可开始观察 DOM。当观察者 observer 发现匹配观察请求中指定的配置项的更改时，callback() 方法便会被调用。

#### [1.2 基本用法](#)
创建并使用 observer 使用以下代码设置一个观察进程。
```javascript
var targetNode = document.querySelector("#someElement");
var observerOptions = {
  childList: true, // 观察目标子节点的变化，是否有添加或者删除
  attributes: true, // 观察属性变动
  subtree: true, // 观察后代节点，默认为 false
};

var observer = new MutationObserver(callback);
observer.observe(targetNode, observerOptions);
```
使用 ID someElement 来获取目标节点树。observerOptions 中设定了观察者的选项，通过设定 childList 和 attributes 为 true 来获取所需信息。

### [2 observe()方法](#)
新创建的 MutationObserver 实例不会关联 DOM 的任何部分。要把这个observer与DOM 关
联起来，需要使用 [observe()](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver/observe) 方法。这个方法接收两个必需的参数：要观察其变化的 DOM 节点，以及一个MutationObserverInit 对象。

```
mutationObserver.observe(target[, options])
```

options 对象用于控制观察哪些方面的变化，是一个键/值对形式配置选项的字典。
例如，下面的代码会创建一个观察者（observer）并配置它观察 `<body>` 元素上的属性变化：

options 的属性如下：
* subtree 可选 当为 true 时，将会监听以 target 为根节点的整个子树。包括子树中所有节点的属性，而不仅仅是针对 target。默认值为 false。
* childList 可选 当为 true 时，监听 target 节点中发生的节点的新增与删除（同时，如果 subtree 为 true，会针对整个子树生效）。默认值为 false。
* attributes 可选 当为 true 时观察所有监听的节点属性值的变化。默认值为 true，当声明了 attributeFilter 或 attributeOldValue，默认值则为 false。
* attributeFilter 可选 一个用于声明哪些属性名会被监听的数组。如果不声明该属性，所有属性的变化都将触发通知。
* attributeOldValue 可选 当为 true 时，记录上一次被监听的节点的属性变化；可查阅监听属性值了解关于观察属性变化和属性值记录的详情。默认值为 false。
* characterData 可选 当为 true 时，监听声明的 target 节点上所有字符的变化。默认值为 true，如果声明了 characterDataOldValue，默认值则为 false
* characterDataOldValue 可选 当为 true 时，记录前一个被监听的节点中发生的文本变化。默认值为 false

#### [2.1 回调与 MutationRecord](#)
每个回调都会收到一个 MutationRecord 实例的数组。MutationRecord 实例包含的信息包括发
生了什么变化，以及 DOM 的哪一部分受到了影响。因为回调执行之前可能同时发生多个满足观察条件
的事件，所以每次执行回调都会传入一个包含按顺序入队的 MutationRecord 实例的数组。
下面展示了反映一个属性变化的 MutationRecord 实例的数组：

```javascript
let observer = new MutationObserver((mutationRecords, observe) => console.log(mutationRecords));

observer.observe(document.body, { attributes: true });
document.body.setAttribute('foo', 'bar');
// [
    // {
        // addedNodes: NodeList [], 
        // attributeName: "foo",
        // attributeNamespace: null,
        // nextSibling: null,
        // oldValue: null,
        // previousSibling: null
        // removedNodes: NodeList [],
        // target: body
        // type: "attributes"
    // }
// ] 
```
下面是一次涉及命名空间的类似变化：
```javascript
let observer = new MutationObserver(
 (mutationRecords) => console.log(mutationRecords));

observer.observe(document.body, { attributes: true });

document.body.setAttributeNS('baz', 'foo', 'bar');
// [
    // {
        // addedNodes: NodeList [],
        // attributeName: "foo",
        // attributeNamespace: "baz",
        // nextSibling: null,
        // oldValue: null,
        // previousSibling: null
        // removedNodes: NodeList [],
        // target: body
        // type: "attributes"
    // }
// ] 
```

| 属 性                                                                             | 说 明                                                                                                                                                              |
|:--------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| target                                                                          | 被修改影响的目标节点                                                                                                                                                       |
| type                                                                            | 字符串，表示变化的类型："attributes"、"characterData"或"childList"oldValue                                                                                                     |
| oldValue                                                                        | 如果在 MutationObserverInit 对象中启用（attributeOldValue 或 characterData OldValue 为 true），"attributes"或"characterData"的变化事件会设置这个属性为被替代的值"childList"类型的变化始终将这个属性设置为 null。 |
| attributeName| 对于"attributes"类型的变化，这里保存被修改属性的名字其他变化事件会将这个属性设置为 null              |
| attributeNamespace| 对于使用了命名空间的"attributes"类型的变化，这里保存被修改属性的名字其他变化事件会将这个属性设置为 null |
| addedNodes |对于"childList"类型的变化，返回包含变化中添加节点的 NodeList默认为空 NodeList                |
| removedNodes| 对于"childList"类型的变化，返回包含变化中删除节点的 NodeList默认为空 NodeList              |
| previousSibling |对于"childList"类型的变化，返回变化节点的前一个同胞 Node默认为 null                    |
| nextSibling  |对于"childList"类型的变化，返回变化节点的后一个同胞 Node默认为 null                       |

传给回调函数的第二个参数是观察变化的 MutationObserver 的实例，演示如下：
```javascript
let observer = new MutationObserver((mutationRecords, mutationObserver) => console.log(mutationRecords, mutationObserver));

observer.observe(document.body, { attributes: true });
document.body.className = 'foo';
// [MutationRecord], MutationObserver 
```

#### [2.2 利用 observe 观察用户输入](#)
MutationObserver 主要用于监听 DOM 树结构的变化，比如元素的添加、移除、属性变化等。但是，它并不能直接监听到 `<input>` 元
素的 value 属性变化，这是因为当用户在文本框中输入内容时，value 的改变不会被视为一个属性变更，因此 MutationObserver 无法捕捉到这种变化。
```html
<input type="text" id="userName" value="" placeholder="请输入anything:" />
```
**但是你会发现用户输入操作检测不到，但是通过定时器的操作却可以被监测到**。
```javascript
window.addEventListener('load', function() {
    console.log("no load");
    let observer = new MutationObserver((mutationRecords) => {
        console.log(mutationRecords);
        console.log("change");
    });

    let target = document.querySelector("#userName");
    observer.observe(target, {
        subtree: true,
        childList: true,
        attributeOldValue: true,
        attributes: true
    });
});

setInterval(()=>{
    let target = document.getElementById("userName");
    target.setAttribute("value", "didi");
},2000);
```

### [3. disconnect()方法](#)
默认情况下，只要被观察的元素不被垃圾回收，MutationObserver 的回调就会响应 DOM 变化事
件，从而被执行。要提前终止执行回调，可以调用 disconnect()方法。

```javascript
let observer = new MutationObserver(() => console.log('<body> attributes changed'));
observer.observe(document.body, { attributes: true });
document.body.className = 'foo';
observer.disconnect();
document.body.className = 'bar';
//（没有日志输出）
```
要想让已经加入任务队列的回调执行，可以使用 setTimeout()让已经入列的回调执行完毕再调用disconnect()：
```javascript
let observer = new MutationObserver(() => console.log('<body> attributes changed'));
observer.observe(document.body, { attributes: true });

document.body.className = 'foo';
setTimeout(() => {
    observer.disconnect();
    document.body.className = 'bar';
}, 0);

// <body> attributes changed 
```