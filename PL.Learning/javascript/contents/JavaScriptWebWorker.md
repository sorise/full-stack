## [JavaScript Web Worker](#)
**介绍**：Web Worker 是浏览器提供的一种多线程技术，允许 JavaScript 在后台线程中运行脚本，不阻塞主线程，用于处理复杂的计算、大数据处理、循环操作等如果在主线程执行，会导致页面卡顿、动画不流畅、用户交互无响应的任务。


---

- [1. Web Worker](#1-web-worker基本概念)


---
### [1. Web Worker基本概念](#)
Web Worker 可以在单独的线程中运行 JavaScript 代码，而不阻塞主线程。主线程负责处理用户交互、页面渲染和其他短时间的任务。

> 当长时间任务在主线程中执行时，浏览器的响应速度会变慢，用户可能会感觉页面卡顿或无响应，使用 Web Worker 可以将这些计算移到后台线程，不影响主线程的运行。

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