## [Node 全局环境](#)
**介绍**： Node.js 中的全局对象是 global,它及其所有属性都可以在程序的任何地方访问，即全局变量。

----

### [1. Node.js 全局对象](#)
在 Node.js 中，有一些全局对象可以在任何地方访问，而不必引入模块。这些全局对象包括：

- [global](https://nodejs.org/api/globals.html): 全局对象，类似于浏览器环境中的 window 对象。可以在任何地方访问该对象。
- [process](https://nodejs.org/docs/latest/api/process.html): 进程对象，可以访问当前 Node.js 进程的信息。
- [console](https://nodejs.org/docs/latest/api/console.html): 控制台对象，用于在命令行中输出信息。
- [Buffer](https://nodejs.org/docs/latest/api/buffer.html): 缓冲区对象，用于处理二进制数据。


### [2. global](#)
global 对象在 Node.js 中类似于浏览器环境中的 window 对象。可以用来定义全局变量和函数，但要注意不要滥用全局对象，以免造成命名冲突和代码混乱。
```javascript
global.myGlobalVariable = 'Hello, world!';
console.log(myGlobalVariable); // 输出: Hello, world!
```

global 最根本的作用是作为全局变量的宿主。按照 ECMAScript 的定义，满足以下条件的变量是全局变量：

- 在最外层定义的变量；
- 全局对象的属性；
- 隐式定义的变量（未定义直接赋值的变量）。
当你定义一个全局变量时，这个变量同时也会成为全局对象的属性，反之亦然。需要注意的是，在 Node.js 中你不可能在最外层定义变量，因为所有用户代码都是属于当前模块的， 而模块本身不是最外层上下文。

> `注意： 最好不要使用 var 定义变量以避免引入全局变量，因为全局变量会污染命名空间，提高代码的耦合风险。`


- `__filename` 表示当前正在执行的脚本的文件名。它将输出文件所在位置的绝对路径，且和命令行参数所指定的文件名不一定相同。 如果在模块中，返回的值是模块文件的路径。
- `__dirname` 表示当前执行脚本所在的目录。
- `setTimeout(cb, ms)` 全局函数在指定的毫秒(ms)数后执行指定函数(cb)。：setTimeout() 只执行一次指定函数。
- `clearTimeout(t)` 全局函数用于停止一个之前通过 setTimeout() 创建的定时器。 参数 t 是通过 setTimeout() 函数创建的定时器。
- `setInterval(cb, ms)` 全局函数在指定的毫秒(ms)数后执行指定函数(cb)。
- `setTimeout(t)` 清除一个定时器。
```javascript
function printHello(){
    console.log( `${__dirname}/${__filename}`);
}
// 两秒后执行以上函数
let tick = setInterval(printHello, 300);

setTimeout(()=>{
    clearInterval(tick);
}, 3000);
```


### [3. process](#)
process 对象提供了当前 Node.js 进程的信息，包括环境变量、命令行参数等。可以通过 process.env 访问环境变量，通过 process.argv 访问命令行参数。



### [4. console](#)
console 用于提供控制台标准输出，它是由 Internet Explorer 的 JScript 引擎提供的调试工具，后来逐渐成为浏览器的实施标准。
Node.js 沿用了这个标准，提供与习惯行为一致的 console 对象，**用于向标准输出流（stdout）或标准错误流（stderr）输出字符**。

- `console.log([data][, ...])`。向标准输出流打印字符并以换行符结束。该方法接收若干 个参数，如果只有一个参数，则输出这个参数的字符串形式。如果有多个参数，则 以类似于C 语言 printf() 命令的格式输出。
- `console.info([data][, ...])`。该命令的作用是返回信息性消息，这个命令与console.log差别并不大，除了在chrome中只会输出文字外，其余的会显示一个蓝色的惊叹号。
- `console.error([data][, ...])`。输出错误消息的。控制台在出现错误时会显示是红色的叉子。
- `console.warn([data][, ...])`。 输出警告消息。控制台出现有黄色的惊叹号。
- `console.assert(value[, message][, ...])`。 用于判断某个表达式或变量是否为真，接收两个参数，第一个参数是表达式，第二个参数是字符串。只有当第一个参数为false，才会输出第二个参数，否则不会有任何结果。
- `console.trace(message[, ...])`。当前执行的代码在堆栈中的调用路径，这个测试函数运行很有帮助，只要给想测试的函数里面加入console.trace 就行了。


#### [4.1 console.dir()](#)
以交互式、可展开的树状列表显示对象的属性。
```javascript
const person = {
    name: "Alice",
    age: 30,
    address: {
        city: "Beijing",
        country: "China"
    },
    greet: function() {
        console.log("Hello!");
    }
};


console.dir(person);
/*
{
  name: 'Alice',
  age: 30,
  address: { city: 'Beijing', country: 'China' },
  greet: [Function: greet]
}
* */
```

#### [4.2 console.time(label)](#)
console.time和console.timeEnd成对出现，label的值必须也是一致的，不然会报错。

- console.time(label) 输出时间，表示计时开始, label表示字符串，默认值是 `default`。 
- console.timeEnd(label) 结束时间，表示计时结束, label表示字符串，默认值是 `default`。

```javascript
console.info("程序开始执行：");

var counter = 10;
console.log("计数: %d", counter);

console.time("获取数据");
//
// 执行一些代码
// 
console.timeEnd('获取数据');

console.info("程序执行完毕。")
```

### [5. Buffer](#)
详情请看[Buffer](./nodeModule/NodeBufferAndFs.md#1-buffer-基本概念)。

