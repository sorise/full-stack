### [JavaScript Web API](#)
**介绍**: 在编写 Web 代码时，有大量可用的 [Web API](https://developer.mozilla.org/zh-CN/docs/Web/API)。

---
- [1. MessagePort](#1-messageport)
- [2. console](#2-console)

---

### [1. MessagePort](#)
[Channel Messaging API](https://developer.mozilla.org/zh-CN/docs/Web/API/Channel_Messaging_API) 的 [MessagePort](https://developer.mozilla.org/zh-CN/docs/Web/API/MessagePort) 接口代表 [MessageChannel](https://developer.mozilla.org/zh-CN/docs/Web/API/MessageChannel) 的两个端口之一，它可以让你从一个端口发送消息，并在消息到达的另一个端口监听它们。

`MessagePort` 是 Web 平台中用于**跨上下文（cross-context）异步通信**的核心机制之一，属于 **Channel Messaging API** 的一部分。它提供了一种安全、高效、可传递（transferable）的方式来在不同的 JavaScript 执行环境之间发送消息。

- **`MessagePort` 总是成对出现**：通过 `new MessageChannel()` 创建，得到两个端口（`port1` 和 `port2`），它们彼此连接。
- 任一端口调用 `postMessage()` 发送的消息，会异步出现在另一端的 `onmessage` 事件中。
- 它支持**结构化克隆算法（Structured Clone Algorithm）**，可以传递对象、数组、Blob、ArrayBuffer 等（但不能传函数或 DOM 节点）。
- 更重要的是：**`MessagePort` 本身是可以被“转移”（transferred）的**，这意味着你可以把一个端口从一个上下文“交给”另一个上下文（如 Worker、iframe、另一个窗口等），实现**链式通信**或**多跳消息路由**。


```js
// 1. 创建通道
const channel = new MessageChannel();
const port1 = channel.port1;
const port2 = channel.port2;

// 2. 监听消息
port1.onmessage = (event) => {
  console.log('Port1 received:', event.data);
};

port2.onmessage = (event) => {
  console.log('Port2 received:', event.data);
};

// 3. 发送消息
port1.postMessage('Hello from port1');
port2.postMessage('Hi from port2');
```

输出：
```
Port2 received: Hello from port1
Port1 received: Hi from port2
```

### [2. console](#)
JavaScript 原生中默认是没有 [Console](https://developer.mozilla.org/zh-CN/docs/Web/API/console) 对象，这是宿主对象（也就是浏览器）提供的内置对象。 用于访问调试控制台, 在不同的浏览器里效果可能不同,console 对象可以从任何全局作用域中访问。

Console 对象常见的两个用途：
- 显示网页代码运行时的错误信息。
- 提供了一个命令行接口，用来与网页代码互动。

|目的|	写法|解释|
|:---|:---|:---|
|打印普通信息|`console.log('你好，编程狮！');`|	最常用的“打招呼”|
|警告黄条|`console.warn('电量低，请充电');`	|不会阻断程序，只是提醒|
|错误红条|`console.error('出错了，快看代码');`|方便一眼定位问题|
|计时|`console.time('循环耗时'); for(let i=0;i<1e6;i++); console.timeEnd('循环耗时');`|测性能，单位毫秒|
|表格展示|`console.table([{name:'张三',age:18},{name:'李四',age:20}]);`|把数组/对象变表格，超直观|
|断言检查|`console.assert(1===2,'1 不等于 2');`|条件为假才报错，写测试很方便|


- console.assert() : 如果第一个参数为 false，则将消息和堆栈跟踪记录到控制台。
- console.clear() : 清空控制台。
- console.count() : 以参数为标识记录调用的次数，调用时在控制台打印标识以及调用次数
- console.countReset() : 重置指定标签的计数器值。
- console.debug() : 在控制台打印一条 "debug" 级别的消息。
- console.error() : 打印一条错误信息，使用方法可以参考使用字符串替换。
- console.table() :  将列表型的数据打印成表格。
- console.time() : 启动一个以入参作为特定名称的定时器，在显示页面中可同时运行的定时器上限为 10,000.
- console.timeEnd() : 结束特定的定时器并以毫秒打印其从开始到结束所用的时间。