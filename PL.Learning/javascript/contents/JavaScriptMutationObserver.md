## [JavaScript MutationObserver](#)
**介绍**：MutationObserver 是 JavaScript 中用于监听 DOM 树变化的接口。通过它，开发者可以监视一个选定元素的属性变化、子节点的变化（添加或删除）、文本内容的变化等。这在现代 Web 开发中非常有用，尤其是在需要对DOM更新做出响应而无需依赖事件处理器的情况下。

-----

- [1. MutationObserver基本概念](#1-mutationobserver基本概念)
- [2. observe()方法](#2-observe方法)
- [3. 异步回调与记录队列](#3-异步回调与记录队列)
- [4. disconnect方法](#4-disconnect方法)
- [5. 性能、内存与垃圾回收](#5-性能内存与垃圾回收)

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

#### [1.3 应用场景](#)
mutationObserver的应用场景非常广泛，以下是一些常见的例子：

* 自动保存表单：监听表单内容的变化，自动保存表单内容，避免用户手动点击保存按钮；
* 自动加载数据：监听列表滚动条位置的变化，自动加载更多数据；
* 观察者模式：监听一个对象的变化，当变化发生时触发相应的回调函数。
* 自动获取页面高度：监听页面添加元素成员,自动计算高度（微页面组装元素列表）

总之，mutationObserver可以帮助我们在DOM变化时及时更新页面，并提升用户体验。


### [2 observe方法](#)
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

```javascript
observer.observe(target, {
    subtree: true,
    childList: true,
    attributeOldValue: true,
    attributes: true,
    attributeFilter: true,
    characterData: true,
    characterDataOldValue: true
});
```

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

#### [2.3 观察属性](#)
MutationObserver 可以观察节点属性的添加、移除和修改。要为属性变化注册回调，需要在
MutationObserverInit 对象中将 attributes 属性设置为 true，如下所示：
```javascript
let observer = new MutationObserver(
 (mutationRecords, ober) => console.log(mutationRecords));
observer.observe(document.body, { attributes: true });
// 添加属性
document.body.setAttribute('foo', 'bar');
// 修改属性
document.body.setAttribute('foo', 'baz');
// 移除属性
document.body.removeAttribute('foo');
// 以上变化都被记录下来了
// [MutationRecord, MutationRecord, MutationRecord] 
```
把 attributes 设置为 true 的默认行为是观察所有属性，但不会在 MutationRecord 对象中记
录原来的属性值。如果想观察某个或某几个属性，可以使用 attributeFilter 属性来设置白名单，即
一个属性名字符串数组：
```javascript
let observer = new MutationObserver(
 (mutationRecords) => console.log(mutationRecords));
observer.observe(document.body, { attributeFilter: ['foo'] });
// 添加白名单属性
document.body.setAttribute('foo', 'bar');
// 添加被排除的属性
document.body.setAttribute('baz', 'qux');
// 只有 foo 属性的变化被记录了
// [MutationRecord]
```
如果想在变化记录中保存属性原来的值，可以将 attributeOldValue 属性设置为 true：
```javascript
let observer = new MutationObserver(
 (mutationRecords) => console.log(mutationRecords.map((x) => x.oldValue)));
observer.observe(document.body, { attributeOldValue: true });

document.body.setAttribute('foo', 'bar');
document.body.setAttribute('foo', 'baz');
document.body.setAttribute('foo', 'qux');
// 每次变化都保留了上一次的值
// [null, 'bar', 'baz'] 
```

#### [2.4 观察字符数据](#)
MutationObserver 可以观察文本节点（如 Text、Comment 或 ProcessingInstruction 节点）
中字符的添加、删除和修改。要为字符数据注册回调，需要在 MutationObserverInit 对象中将
characterData 属性设置为 true，如下所示：

```javascript
let observer = new MutationObserver(
 (mutationRecords) => console.log(mutationRecords));
// 创建要观察的文本节点
document.body.firstChild.textContent = 'foo';
observer.observe(document.body.firstChild, { characterData: true });
// 赋值为相同的字符串
document.body.firstChild.textContent = 'foo';
// 赋值为新字符串
document.body.firstChild.textContent = 'bar';
// 通过节点设置函数赋值
document.body.firstChild.textContent = 'baz';
// 以上变化都被记录下来了
// [MutationRecord, MutationRecord, MutationRecord] 
```
将 characterData 属性设置为 true 的默认行为不会在 MutationRecord 对象中记录原来的字符
数据。如果想在变化记录中保存原来的字符数据，可以将 characterDataOldValue 属性设置为 true：
```javascript
let observer = new MutationObserver(
 (mutationRecords) => console.log(mutationRecords.map((x) => x.oldValue)));

document.body.innerText = 'foo';
observer.observe(document.body.firstChild, { characterDataOldValue: true });

document.body.innerText = 'foo';
document.body.innerText = 'bar';

document.body.firstChild.textContent = 'baz';
// 每次变化都保留了上一次的值
// ["foo", "foo", "bar"] 
```

#### [2.5 观察子节点](#)
MutationObserver 可以观察目标节点子节点的添加和移除。要观察子节点，需要在 MutationObserverInit 对象中将 childList 属性设置为 true。

下面的例子演示了添加子节点：
```javascript
// 清空主体
document.body.innerHTML = '';
let observer = new MutationObserver(
(mutationRecords) => console.log(mutationRecords));

observer.observe(document.body, { childList: true });
document.body.appendChild(document.createElement('div'));
// [
    // {
        // addedNodes: NodeList[div],
        // attributeName: null,
        // attributeNamespace: null,
        // oldValue: null,
        // nextSibling: null,
        // previousSibling: null,
        // removedNodes: NodeList[],
        // target: body,
        // type: "childList",
    // }
// ]
```
下面的例子演示了移除子节点：
```javascript
// 清空主体
document.body.innerHTML = '';
let observer = new MutationObserver(
 (mutationRecords) => console.log(mutationRecords));
observer.observe(document.body, { childList: true });
document.body.appendChild(document.createElement('div'));
// [
    // {
        // addedNodes: NodeList[],
        // attributeName: null,
        // attributeNamespace: null,
        // oldValue: null,
        // nextSibling: null,
        // previousSibling: null,
        // removedNodes: NodeList[div],
        // target: body,
        // type: "childList",
    // }
// ] 
```
对子节点重新排序（尽管调用一个方法即可实现）会报告两次变化事件，因为从技术上会涉及先移
除和再添加：
```javascript
// 清空主体
document.body.innerHTML = '';
let observer = new MutationObserver(
 (mutationRecords) => console.log(mutationRecords));
// 创建两个初始子节点
document.body.appendChild(document.createElement('div'));
document.body.appendChild(document.createElement('span'));
observer.observe(document.body, { childList: true });
// 交换子节点顺序
document.body.insertBefore(document.body.lastChild, document.body.firstChild);
// 发生了两次变化：第一次是节点被移除，第二次是节点被添加
// [
    // {
        // addedNodes: NodeList[],
        // attributeName: null,
        // attributeNamespace: null,
        // oldValue: null,
        // nextSibling: null,
        // previousSibling: div,
        // removedNodes: NodeList[span],
        // target: body,
        // type: childList,
    // },
    // {
        // addedNodes: NodeList[span],
        // attributeName: null,
        // attributeNamespace: null,
        // oldValue: null,
        // nextSibling: div,
        // previousSibling: null,
        // removedNodes: NodeList[],
        // target: body,
        // type: "childList",
    // }
// ] 
```

#### [2.6 观察子树](#)
默认情况下，MutationObserver 将观察的范围限定为一个元素及其子节点的变化。可以把观察
的范围扩展到这个元素的子树（所有后代节点），这需要在 MutationObserverInit 对象中将 subtree
属性设置为 true。

```javascript
// 清空主体
document.body.innerHTML = '';
let observer = new MutationObserver(
 (mutationRecords) => console.log(mutationRecords));
// 创建一个后代
document.body.appendChild(document.createElement('div'));

// 观察<body>元素及其子树
observer.observe(document.body, { attributes: true, subtree: true });
// 修改<body>元素的子树
document.body.firstChild.setAttribute('foo', 'bar');
// 记录了子树变化的事件
// [
    // {
        // addedNodes: NodeList[],
        // attributeName: "foo",
        // attributeNamespace: null,
        // oldValue: null,
        // nextSibling: null,
        // previousSibling: null,
        // removedNodes: NodeList[],
        // target: div,
        // type: "attributes",
    // }
// ]
```
有意思的是，被观察子树中的节点被移出子树之后仍然能够触发变化事件。这意味着在子树中的节
点离开该子树后，即使严格来讲该节点已经脱离了原来的子树，但它仍然会触发变化事件。
```javascript
// 清空主体
document.body.innerHTML = '';
let observer = new MutationObserver(
 (mutationRecords) => console.log(mutationRecords));
let subtreeRoot = document.createElement('div'),
 subtreeLeaf = document.createElement('span');
// 创建包含两层的子树
document.body.appendChild(subtreeRoot);
subtreeRoot.appendChild(subtreeLeaf);
// 观察子树
observer.observe(subtreeRoot, { attributes: true, subtree: true });
// 把节点转移到其他子树
document.body.insertBefore(subtreeLeaf, subtreeRoot);
subtreeLeaf.setAttribute('foo', 'bar');
// 移出的节点仍然触发变化事件
// [MutationRecord] 
```

### [3. 异步回调与记录队列](#)
MutationObserver 接口是出于性能考虑而设计的，其核心是 **异步回调** 与 **记录队列模型**。为了在
大量变化事件发生时不影响性能，每次变化的信息（由观察者实例决定）会保存在 MutationRecord
实例中，然后添加到记录队列。这个队列对每个 MutationObserver 实例都是唯一的，是所有 DOM
变化事件的有序列表。

#### [3.1 记录队列](#)
每次 MutationRecord 被添加到 MutationObserver 的记录队列时，仅当之前没有已排期的微任
务回调时（队列中微任务长度为 0），才会将观察者注册的回调（在初始化 MutationObserver 时传入）
作为微任务调度到任务队列上。这样可以保证记录队列的内容不会被回调处理两次。 

不过在回调的微任务异步执行期间，有可能又会发生更多变化事件。因此被调用的回调会接收到一
个 MutationRecord 实例的数组，顺序为它们进入记录队列的顺序。回调要负责处理这个数组的每一
个实例，因为函数退出之后这些实现就不存在了。回调执行后，这些 MutationRecord 就用不着了，
因此记录队列会被清空，其内容会被丢弃。

#### [3.2 takeRecords()方法](#)
调用 MutationObserver 实例的 takeRecords()方法可以清空记录队列，取出并返回其中的所有 MutationRecord 实例。看这个例子：
```javascript
let observer = new MutationObserver(
(mutationRecords) => console.log(mutationRecords));

observer.observe(document.body, { attributes: true });
document.body.className = 'foo';
document.body.className = 'bar';
document.body.className = 'baz';
console.log(observer.takeRecords());
console.log(observer.takeRecords());
// [MutationRecord, MutationRecord, MutationRecord]
// []
```
这在希望断开与观察目标的联系，但又希望处理由于调用 disconnect()而被抛弃的记录队列中的
MutationRecord 实例时比较有用。

### [4. disconnect方法](#)
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

### [5. 性能、内存与垃圾回收](#)
DOM Level 2 规范中描述的 **MutationEvent** 定义了一组会在各种 DOM 变化时触发的事件。由于
浏览器事件的实现机制，这个接口出现了严重的性能问题。因此，DOM Level 3 规定废弃了这些事件。
**MutationObserver** 接口就是为替代这些事件而设计的更实用、性能更好的方案。

将变化回调委托给微任务来执行可以保证事件同步触发，同时避免随之而来的混乱。为 **MutationObserver** 而实现的记录队列，可以保证即使变化事件被爆发式地触发，也不会显著地拖慢浏览器。
无论如何，使用 **MutationObserver** 仍然不是没有代价的。因此理解什么时候避免出现这种情况就很重要了。

#### [5.1 MutationObserver 的引用](#)
**MutationObserver** 实例与目标节点之间的引用关系是非对称的。**MutationObserver** 拥有对要观察的目标节点的弱引用。
因为是弱引用，所以不会妨碍垃圾回收程序回收目标节点。

然而，目标节点却拥有对 **MutationObserver** 的强引用。如果目标节点从 DOM 中被移除，随后
被垃圾回收，则关联的 **MutationObserver** 也会被垃圾回收。

#### [5.2 MutationRecord 的引用](#)
记录队列中的每个 MutationRecord 实例至少包含对已有 DOM 节点的一个引用。如果变化是
childList 类型，则会包含多个节点的引用。记录队列和回调处理的默认行为是耗尽这个队列，处理
每个 MutationRecord，然后让它们超出作用域并被垃圾回收。

有时候可能需要保存某个观察者的完整变化记录。保存这些 **MutationRecord** 实例，也就会保存
它们引用的节点，因而会妨碍这些节点被回收。如果需要尽快地释放内存，建议从每个 **MutationRecord**
中抽取出最有用的信息，然后保存到一个新对象中，最后抛弃 **MutationRecord**。