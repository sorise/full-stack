## [JavaScript RequestAnimationFrame](#)
**介绍**：window.requestAnimationFrame() 方法会告诉浏览器你希望执行一个动画。它要求浏览器在下一次重绘之前，调用用户提供的回调函数。

---

- [1. 基本概念](#1-基本概念)
- [2. requestanimationframe](#2-requestanimationframe)

---

### [1. 基本概念](#)
window.requestAnimationFrame() 方法会告诉浏览器你希望执行一个动画。它要求浏览器在下一次重绘之前，调用用户提供的回调函数。

> 对回调函数的调用频率通常与显示器的刷新率相匹配。虽然 75hz、120hz 和 144hz 也被广泛使用，但是最常见的刷新率还是 60hz（每秒 60 个周期/帧）。
为了提高性能和电池寿命，大多数浏览器都会暂停在后台选项卡或者隐藏的 `<iframe>` 中运行的 **requestAnimationFrame**()。

**前端动画方案**：

css动画
- transition：过渡动画
- animation：直接动画（搭配@keyframes）

js动画
- setInterval或setTimeout定时器（比如不停地更改dom元素的位置，使其运动起来）
- canvas动画，搭配js中的定时器去运动起来（canvas只是一个画笔，然后我们通过定时器会使用这个画笔去画画-动画）
- requestAnimationFrame动画（js动画中的较好方案）


#### [1.1 早期定时动画](#)
以前，在 JavaScript 中创建动画基本上就是使用 setInterval()来控制动画的执行。下面的例子展示了使用 **setInterval**()的基本模式：

```javascript
(function() {
    function updateAnimations() {
        doAnimation1();
        doAnimation2();
        // 其他任务
    };
setInterval(updateAnimations, 100); 
})();
```

作为一个小型动画库的标配，这个 updateAnimations()方法会周期性运行注册的动画任务，并
反映出每个任务的变化（例如，同时更新滚动新闻和进度条）。如果没有动画需要更新，则这个方法既
可以什么也不做，直接退出，也可以停止动画循环，等待其他需要更新的动画。

这种定时动画的问题在于无法准确知晓循环之间的延时。定时间隔必须足够短，这样才能让不同的
动画类型都能平滑顺畅，但又要足够长，以便产生浏览器可以渲染出来的变化。一般计算机显示器的屏
幕刷新率都是 60Hz，基本上意味着每秒需要重绘 60 次。大多数浏览器会限制重绘频率，使其不超出屏
幕的刷新率，这是因为超过刷新率，用户也感知不到。

因此，实现平滑动画最佳的重绘间隔为 1000 毫秒/60，大约 17 毫秒。以这个速度重绘可以实现最平
滑的动画，因为这已经是浏览器的极限了。如果同时运行多个动画，可能需要加以限流，以免 17 毫秒
的重绘间隔过快，导致动画过早运行完。

虽然使用 setInterval()的定时动画比使用多个 setTimeout()实现循环效率更高，但也不是没
有问题。无论 setInterval()还是 setTimeout()都是不能保证时间精度的。作为第二个参数的延时
只能保证何时会把代码添加到浏览器的任务队列，不能保证添加到队列就会立即运行。如果队列前面还
有其他任务，那么就要等这些任务执行完再执行。简单来讲，这里毫秒延时并不是说何时这些代码会执
行，而只是说到时候会把回调加到任务队列。如果添加到队列后，主线程还被其他任务占用，比如正在
处理用户操作，那么回调就不会马上执行。

#### [1.2 时间间隔的问题](#)
知道何时绘制下一帧是创造平滑动画的关键。直到几年前，都没有办法确切保证何时能让浏览器把
下一帧绘制出来。随着 `<canvas>` 的流行和 HTML5 游戏的兴起，开发者发现 setInterval()和
setTimeout()的不精确是个大问题。

浏览器自身计时器的精度让这个问题雪上加霜。浏览器的计时器精度不足毫秒。以下是几个浏览器计时器的精度情况：
- IE8 及更早版本的计时器精度为 15.625 毫秒；
- IE9 及更晚版本的计时器精度为 4 毫秒；
- Firefox 和 Safari 的计时器精度为约 10 毫秒；
- Chrome 的计时器精度为 4 毫秒。

IE9 之前版本的计时器精度是 15.625 毫秒，意味着 0～15 范围内的任何值最终要么是 0，要么是 15，
不可能是别的数。IE9 把计时器精度改进为 4 毫秒，但这对于动画而言还是不够精确。Chrome 计时器精
度是 4 毫秒，而 Firefox 和 Safari 是 10 毫秒。更麻烦的是，浏览器又开始对切换到后台或不活跃标签页
中的计时器执行限流。因此即使将时间间隔设定为最优，也免不了只能得到近似的结果。

Mozilla的 Robert O’Callahan一直在思考这个问题，并提出了一个独特的方案。他指出，浏览器知道 CSS
过渡和动画应该什么时候开始，并据此计算出正确的时间间隔，到时间就去刷新用户界面。但对于 JavaScript
动画，浏览器不知道动画什么时候开始。他给出的方案是创造一个名为 mozRequestAnimationFrame()
的新方法，用以通知浏览器某些 JavaScript 代码要执行动画了。这样浏览器就可以在运行某些代码后进
行适当的优化。目前所有浏览器都支持这个方法不带前缀的版本，即 requestAnimationFrame()。

#### [1.3 什么是requestAnimationFrame](#)
requestAnimationFrame是浏览器用于定时循环操作的一个API,通常用于动画和游戏开发。它会把每一帧中的所有DOM操作集中起来,在重绘之前一次性更新,并且关联到浏览器的重绘操作。

**为什么使用requestAnimationFrame而不是setTimeout或setInterval** 。

与setTimeout或setInterval相比,requestAnimationFrame具有以下优势:
- 通过系统时间间隔来调用回调函数，无需担心系统负载和阻塞问题，系统会自动调整回调频率。
- 由浏览器内部进行调度和优化，性能更高，消耗的CPU和GPU资源更少。
- 避免帧丢失现象，确保回调连续执行，实现更流畅的动画效果。
- 自动合并多个回调，避免不必要的开销。
- 与浏览器的刷新同步，不会在浏览器页面不可见时执行回调。

**requestAnimationFrame的优势和适用场景** 

requestAnimationFrame最适用于需要连续高频执行的动画,如游戏开发,数据可视化动画等。它与浏览器刷新周期保持一致,不会因为间隔时间不均匀而导致动画卡顿。

### [2. requestAnimationFrame](#)
requestAnimationFrame()方法接收一个参数，此参数是一个要在重绘屏幕前调用的函数。这个函数就是修改 DOM 样式以反映下一次重绘有什么变化的地方。为了实现动画循环，可以把多个
requestAnimationFrame()调用串联起来，就像以前使用 setTimeout()时一样：

```javascript
function updateProgress() {
    var div = document.getElementById("status");
    div.style.width = (parseInt(div.style.width, 10) + 5) + "%";
    if (div.style.left != "100%") {
        requestAnimationFrame(updateProgress);
    }
}
requestAnimationFrame(updateProgress);
```
因为 requestAnimationFrame()只会调用一次传入的函数，所以每次更新用户界面时需要再手
动调用它一次。同样，也需要控制动画何时停止。结果就会得到非常平滑的动画。

目前为止，requestAnimationFrame()已经解决了浏览器不知道 JavaScript 动画何时开始的问题，
以及最佳间隔是多少的问题，但是，不知道自己的代码何时实际执行的问题呢？这个方案同样也给出了解决方法。

传给 requestAnimationFrame()的函数实际上可以接收一个参数，此参数是一个 DOMHighResTimeStamp 的实例（比如 performance.now()返回的值），表示下次重绘的时间。这一点非常重要：
requestAnimationFrame()实际上把重绘任务安排在了未来一个已知的时间点上，而且通过这个参数
告诉了开发者。基于这个参数，就可以更好地决定如何调优动画了。

#### [2.1 cancelAnimationFrame](#)
与 setTimeout()类似，requestAnimationFrame()也返回一个请求 ID，可以用于通过另一个
方法 cancelAnimationFrame()来取消重绘任务。下面的例子展示了刚把一个任务加入队列又立即将其取消：

```javascript
let requestID = window.requestAnimationFrame(() => { 
 console.log('Repaint!'); 
}); 

window.cancelAnimationFrame(requestID);
```

#### [2.2 通过 requestAnimationFrame 节流](#)
requestAnimationFrame 这个名字有时候会让人误解，因为看不出来它跟排期任务有关。支持这
个方法的浏览器实际上会暴露出作为钩子的回调队列。所谓钩子（hook），就是浏览器在执行下一次重
绘之前的一个点。这个回调队列是一个可修改的函数列表，包含应该在重绘之前调用的函数。每次调用
requestAnimationFrame()都会在队列上推入一个回调函数，队列的长度没有限制。

这个回调队列的行为不一定跟动画有关。不过，通过 requestAnimationFrame()递归地向队列
中加入回调函数，可以保证每次重绘最多只调用一次回调函数。这是一个非常好的节流工具。在频繁执
行影响页面外观的代码时（比如滚动事件监听器），可以利用这个回调队列进行节流。

先来看一个原生实现，其中的滚动事件监听器每次触发都会调用名为 expensiveOperation()（耗
时操作）的函数。当向下滚动网页时，这个事件很快就会被触发并执行成百上千次：

```javascript
function expensiveOperation() {
    console.log('Invoked at', Date.now());
};

window.addEventListener('scroll', () => {
    expensiveOperation();
});
```
如果想把事件处理程序的调用限制在每次重绘前发生，那么可以像这样下面把它封装到 requestAnimationFrame()调用中：

```javascript
function expensiveOperation() {
    console.log('Invoked at', Date.now());
};

window.addEventListener('scroll', () => {
    window.requestAnimationFrame(expensiveOperation);
}); 
```
这样会把所有回调的执行集中在重绘钩子，但不会过滤掉每次重绘的多余调用。此时，定义一个标
志变量，由回调设置其开关状态，就可以将多余的调用屏蔽：
```javascript
let enqueued = false;

function expensiveOperation() {
     console.log('Invoked at', Date.now());
     enqueued = false;
};

window.addEventListener('scroll', () => {
 if (!enqueued) {
     enqueued = true;
     window.requestAnimationFrame(expensiveOperation);
 }
}); 
```
因为重绘是非常频繁的操作，所以这还算不上真正的节流。更好的办法是配合使用一个计时器来限
制操作执行的频率。这样，计时器可以限制实际的操作执行间隔，而 requestAnimationFrame 控制
在浏览器的哪个渲染周期中执行。下面的例子可以将回调限制为不超过 50 毫秒执行一次：

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Document</title>
</head>
<body>
    <div class="container">
        <div class="content" id="cmd_id">

        </div>
    </div>
    <div id="div1">
        <p><b>Hello</b> world!</p>
        <ul>
            <li>List item 1</li>
            <li>List item 2</li>
            <li>List item 3</li>
        </ul>
    </div>
</body>
<style>
    *{
        padding: 0;
        margin: 0;
    }
    :root {
        --containerWidth: 1000px;
        --containerHeight: 500px;
    }
    .container{
        background-color: rgba(0,0,0,0.01);
        height: var(--containerHeight);
        width: var(--containerWidth);
        margin: 15px auto;

        --contentHW: 200px;
        .content{
            height: var(--contentHW);
            width: var(--contentHW);
            background-color: red;
            box-sizing: border-box;
            border: 5px solid green;
            padding: 10px;
            margin-top: 15px;
            margin-left: auto;
            margin-right: auto;
            font-size: 12px;
            color: ghostwhite;
        }
    }
</style>
<script>
    function updateProgress() {
        var div = document.getElementById("cmd_id");
        let t= new Date(Date.now());
        let lt = t.toLocaleString();
        let ltt = t.getMilliseconds();
        div.textContent = lt + " " + ltt;
        requestAnimationFrame(updateProgress);
    }
    requestAnimationFrame(updateProgress);
</script>
```