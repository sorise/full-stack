## [Node 全局环境](#)
**介绍**： Node.js 中的全局对象是 global,它及其所有属性都可以在程序的任何地方访问，即全局变量。

----

- [1. Node.js 全局对象](#1-nodejs-全局对象)
- [2. global](#2-global)
- [3. process](#3-process)
- [4. console](#4-console)

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

- 在最外层定义的变量。
- 全局对象的属性。
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

#### [1.1 顶层全局对象global的核心特性](#)

- **自指性**：`global.global === global`，与window类似，是Node.js的顶层全局对象。
- **显式挂载规则**：全局变量不会自动挂载到global，需显式赋值才能成为全局共享变量。
- **服务器能力集成**：包含进程控制、模块管理、文件操作等服务器端特有API。

#### [1.2 global的常用属性与方法](#)

|功能类别|典型成员|说明|
|:---|:---|:---|
|全局变量载体|`global.xxx`|需显式赋值才会成为全局共享变量|
|模块信息|`__dirname、__filename`|当前模块的目录路径、文件路径（模块级伪全局）|
|进程控制|`process`|Node.js进程对象，包含环境变量（process.env）、退出控制等|
|定时器|`setTimeout()`、`setInterval()`|行为与浏览器类似，但绑定到global|
|控制台输出|`console.log()`|向终端输出信息|
|错误处理|`Error、global.Error`|错误构造函数，用于抛出异常|




### [3. process](#)
**process 是一个全局变量，即 global 对象的属性**。process 对象提供了当前 Node.js 进程的信息，包括环境变量、命令行参数等。可以通过 process.env 访问环境变量，通过 process.argv 访问命令行参数。


|序号|事件| 描述|
|:---|:---|:---|
|1|exit |当进程准备退出时触发。 |
|2|beforeExit|当 node 清空事件循环，并且没有其他安排时触发这个事件。通常来说，当没有进程安排时 node 退出，但是 'beforeExit' 的监听器可以异步调用，这样 node 就会继续执行。|
|3|uncaughtException|当一个异常冒泡回到事件循环，触发这个事件。如果给异常添加了监视器，默认的操作（打印堆栈跟踪信息并退出）就不会发生。|
|4|Signal| 事件当进程接收到信号时就触发。信号列表详见标准的 POSIX 信号名，如 SIGINT、SIGUSR1 等。|

```javascript
process.on('beforeExit', (code) => {
  console.log('Process beforeExit event with code: ', code);
});

process.on('exit', (code) => {
  console.log('Process exit event with code: ', code);
});

console.log('This message is displayed first.');

// Prints:
// This message is displayed first.
// Process beforeExit event with code: 0
// Process exit event with code: 0
```

#### [3.1 退出状态码](#)
process 退出状态码如下所示：

|状态码	|名称 | 描述|
|:---|:---|:---|
|1 |Uncaught Fatal Exception | 有未捕获异常，并且没有被域或 uncaughtException 处理函数处理。|
|2 |Unused| 保留，由 Bash 预留用于内置误用|
|3 |Internal JavaScript Parse Error | JavaScript的源码启动 Node 进程时引起解析错误。非常罕见，仅会在开发 Node 时才会有。|
|4 |Internal JavaScript Evaluation Failure | JavaScript 的源码启动 Node 进程，评估时返回函数失败。非常罕见，仅会在开发 Node 时才会有。|
|5|	Fatal Error| V8 里致命的不可恢复的错误。通常会打印到 stderr ，内容为： FATAL ERROR|
|6|	Non-function Internal Exception Handler|未捕获异常，内部异常处理函数不知为何设置为on-function，并且不能被调用。|
|7|	Internal Exception Handler Run-Time Failure|未捕获的异常， 并且异常处理函数处理时自己抛出了异常。例如，如果 process.on('uncaughtException') 或 domain.on('error') 抛出了异常。|
|8|	Unused| 保留，在以前版本的 Node.js 中，退出码 8 有时表示未捕获的异常。|
|9|	Invalid Argument|可能是给了未知的参数，或者给的参数没有值。|
|10| Internal JavaScript Run-Time Failure|JavaScript的源码启动 Node 进程时抛出错误，非常罕见，仅会在开发 Node 时才会有。|
|12| Invalid Debug Argument| 设置了参数--debug 和/或 --debug-brk，但是选择了错误端口。|
|128| Signal Exits|如果 Node 接收到致命信号，比如SIGKILL 或 SIGHUP，那么退出代码就是128 加信号代码。这是标准的 Unix 做法，退出信号代码放在高位。|

#### [3.2 Process 属性](#)
Process 提供了很多有用的属性，便于我们更好的控制系统的交互：

|序号|属性 | 描述|
|:---|:---|:---|
|1|	stdout|标准输出流。|
|2|	stderr|标准错误流。|
|3|	stdin|标准输入流。|
|4|	argv|argv 属性返回一个数组，由命令行执行脚本时的各个参数组成。它的第一个成员总是node，第二个成员是脚本文件名，其余成员是脚本文件的参数。|
|5|	execPath|返回执行当前脚本的 Node 二进制文件的绝对路径。|
|6|	execArgv|返回一个数组，成员是命令行下执行脚本时，在Node可执行文件与脚本文件之间的命令行参数。|
|7|	env|返回一个对象，成员为当前 shell 的环境变量|
|8|	exitCode|进程退出时的代码，如果进程优通过 process.exit() 退出，不需要指定退出码。|
|9| version|Node 的版本，比如v0.10.18。|
|10|versions|一个属性，包含了 node 的版本和依赖.|
|11|config|一个包含用来编译当前 node 执行文件的 javascript 配置选项的对象。它与运行 ./configure 脚本生成的 "config.gypi" 文件相同。|
|12|pid|当前进程的进程号。|
|13|title|进程名，默认值为"node"，可以自定义该值。|
|14|arch|当前 CPU 的架构：'arm'、'ia32' 或者 'x64'。|
|15|platform|运行程序所在的平台系统 'darwin', 'freebsd', 'linux', 'sunos' 或 'win32'|
|16|mainModule|require.main 的备选方法。不同点，如果主模块在运行时改变，require.main可能会继续返回老的模块。可以认为，这两者引用了同一个模块。|

#### [3.3 process 方法](#)
Process 提供了很多有用的方法，便于我们更好的控制系统的交互：

|序号|属性 | 描述|
|:---|:---|:---|
|1|abort()|这将导致 node 触发 abort 事件。会让 node 退出并生成一个核心文件。|
|2|chdir(directory)|改变当前工作进程的目录，如果操作失败抛出异常。|
|3|cwd()|返回当前进程的工作目录|
|4|exit([code])|使用指定的 code 结束进程。如果忽略，将会使用 code 0。|
|5|getgid()|获取进程的群组标识（参见 getgid(2)）。获取到的是群组的数字 id，而不是名字。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。|
|6|setgid(id)|设置进程的群组标识（参见 setgid(2)）。可以接收数字 ID 或者群组名。如果指定了群组名，会阻塞等待解析为数字 ID 。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。|
|7|getuid()|获取进程的用户标识(参见 getuid(2))。这是数字的用户 id，不是用户名。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。|
|8|setuid(id)|设置进程的用户标识（参见setuid(2)）。接收数字 ID或字符串名字。如果指定了群组名，会阻塞等待解析为数字 ID 。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。|
|9|getgroups()|返回进程的群组 ID 数组。POSIX 系统没有保证一定有，但是 node.js 保证有。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。|
|10|setgroups(groups)|设置进程的群组 ID。这是授权操作，所以你需要有 root 权限，或者有 CAP_SETGID 能力。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。|
|11|initgroups(user, extra_group)|读取 /etc/group ，并初始化群组访问列表，使用成员所在的所有群组。这是授权操作，所以你需要有 root 权限，或者有 CAP_SETGID 能力。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。|
|12|kill(pid[, signal])|发送信号给进程. pid 是进程id，并且 signal 是发送的信号的字符串描述。信号名是字符串，比如 'SIGINT' 或 'SIGHUP'。如果忽略，信号会是 'SIGTERM'。|
|13|memoryUsage()|返回一个对象，描述了 Node 进程所用的内存状况，单位为字节。|
|14|nextTick(callback)|一旦当前事件循环结束，调用回调函数。|
|15|`umask([mask])`|设置或读取进程文件的掩码。子进程从父进程继承掩码。如果mask 参数有效，返回旧的掩码。否则，返回当前掩码。|
|16|uptime()|返回 Node 已经运行的秒数。|
|17|hrtime()|返回当前进程的高分辨时间，形式为 [seconds, nanoseconds]数组。它是相对于过去的任意事件。该值与日期无关，因此不受时钟漂移的影响。主要用途是可以通过精确的时间间隔，来衡量程序的性能。你可以将之前的结果传递给当前的 process.hrtime() ，会返回两者间的时间差，用来基准和测量时间间隔。|

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

