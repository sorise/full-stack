## [JavaScript Web Worker](#)
**介绍**：Web Worker 是浏览器提供的一种多线程技术，允许 JavaScript 在后台线程中运行脚本，不阻塞主线程，用于处理复杂的计算、大数据处理、循环操作等如果在主线程执行，会导致页面卡顿、动画不流畅、用户交互无响应的任务。


---

- [1. Web Worker](#1-web-worker基本概念)
- [2. SharedWorker]

---
### [1. Web Worker基本概念](#)
[Web Worker](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers)为 Web 内容在后台线程中运行脚本提供了一种简单的方法。线程可以执行任务而不干扰用户界面。此外，它们可以使用 XMLHttpRequest（尽管 responseXML 和 channel 属性总是为空）或 fetch（没有这些限制）执行 I/O。一旦创建，一个 worker 可以将消息发送到创建它的 JavaScript 代码，通过将消息发布到该代码指定的事件处理器（反之亦然）

> 可以在单独的线程中运行 JavaScript 代码，而不阻塞主线程。主线程负责处理用户交互、页面渲染和其他短时间的任务。

**适合场景**
- 大量CPU密集型任务（大文件上传、图标计算）
- 任务可以独立拆分

> 当长时间任务在主线程中执行时，浏览器的响应速度会变慢，用户可能会感觉页面卡顿或无响应，使用 Web Worker 可以将这些计算移到后台线程，不影响主线程的运行。

一个 worker 是使用一个构造函数创建的一个对象（例如 Worker()）运行一个命名的 JavaScript 文件——这个文件包含将在 worker 线程中运行的代码; worker 运行在另一个全局上下文中，不同于当前的window。因此，在 Worker 内通过 window 获取全局作用域（而不是self）将返回错误。

在 worker 内，不能直接操作 DOM 节点，也不能使用 window 对象的默认方法和属性。

**worker 特性检测**:
为了更好的错误处理控制以及向下兼容，将你的 worker 运行代码包裹在以下代码中是一个很好的想法  
```js
if (window.Worker) {
  // …
}
```
#### [1.1 生成一个专用 worker](#)
创建一个新的 worker 很简单。你需要做的是调用 Worker() 构造器，指定一个脚本的 URI 来执行 worker 线程。
```js
const myWorker = new Worker("worker.js");
```

#### [1.2 专用 worker 中消息的接收和发送](#)
你可以通过 postMessage() 方法和 onmessage 事件处理函数触发 worker 的方法。当你想要向一个 worker 发送消息时，你只需要这样做（main.js）：

```js
let cok_element = document.querySelector("#cok");
let worker = new Worker("./src/worker_example.js");
worker.onmessage = function(e) {
    //等待结果
    let message = e.data;
    cok_element.textContent = message;
    worker.terminate();
}

worker.postMessage(40); //发送数据
```
在 worker 中接收到消息后，我们可以写这样一个事件处理函数代码作为响应（worker_example.js）：
```js
function fib(n){
    if ( n == 0) return 0;
    if ( n == 1) return 1;
    return fib(n - 1) + fib(n - 2);
}

self.addEventListener('message', (event) => {
    const message = event;
    self.postMessage(fib(event.data));
})
```

#### [1.3 终止 worker](#)
如果你需要从主线程中立刻终止一个运行中的 worker，可以调用 worker 的 terminate 方法：
```js
myWorker.terminate();
```

#### [1.4 处理错误](#)
当 worker 出现运行中错误时，它的 onerror 事件处理函数会被调用。它会收到一个扩展了 ErrorEvent 接口的名为 error 的事件。

该事件不会冒泡并且可以被取消；为了防止触发默认动作，worker 可以调用错误事件的 preventDefault() 方法。

错误事件有以下三个用户关心的字段：
- message 可读性良好的错误消息。
- filename 发生错误的脚本文件名。
- lineno 发生错误时所在脚本文件的行号。

#### [1.5 生成 subworker](#)
如果需要的话，worker 能够生成更多的 worker，这就是所谓的 subworker，它们必须托管在同源的父页面内。而且，subworker 解析 URI 时会相对于父 worker 的地址而不是自身页面的地址。这使得 worker 更容易记录它们之间的依赖关系。 

#### [1.6 引入脚本与库](#)
Worker 线程能够访问一个全局函数 importScripts() 来引入脚本，该函数接受 0 个或者多个 URI 作为参数来引入资源；以下例子都是合法的：

```js
importScripts(); /* 什么都不引入 */
importScripts("foo.js"); /* 只引入 "foo.js" */
importScripts("foo.js", "bar.js"); /* 引入两个脚本 */
importScripts("//example.com/hello.js"); /* 你可以从其他来源导入脚本 */
```

#### [1.7 例子](#)
```javascript
// main.js
const worker = new Worker('worker.js');
worker.onmessage = function(event) {
  console.log('长时间任务完成:', event.data);
};
worker.postMessage('开始任务');

// worker.js
self.onmessage = function(event) {
  let sum = 0;
  for (let i = 0; i < 1e9; i++) {
    sum += i;
  }
  self.postMessage(sum);
};
```
Web Worker 运行在独立的线程中，可以利用多核 CPU 提高性能。对于 CPU 密集型任务，使用多线程可以显著提升计算效率。

```javascript
// main.js
const worker1 = new Worker('worker.js');
const worker2 = new Worker('worker.js');

worker1.onmessage = function(event) {
  console.log('Worker 1 任务完成:', event.data);
};

worker2.onmessage = function(event) {
  console.log('Worker 2 任务完成:', event.data);
};

worker1.postMessage('开始任务 1');
worker2.postMessage('开始任务 2');
```
Web Worker 使得长时间任务的代码可以独立于主线程运行，代码更加模块化和清晰。这种方式使得代码更易于维护和调试。
```javascript
// worker.js
self.onmessage = function(event) {
  // 执行长时间任务
  let result = performLongTask();
  self.postMessage(result);
};

function performLongTask() {
  let sum = 0;
  for (let i = 0; i < 1e9; i++) {
    sum += i;
  }
  return sum;
}
```

```javascript
// main.js
const worker = new Worker('worker.js');

worker.onmessage = function(event) {
  document.getElementById('result').textContent = '计算结果: ' + event.data;
};

document.getElementById('startButton').addEventListener('click', () => {
  worker.postMessage('开始计算');
});
```