## [Node.JS 架构](#)
Node.js 在浏览器之外运行 V8 JavaScript 引擎，即 Google Chrome 的核心。这使 Node.js 的性能非常出色。

----


- [1. Node.js 简介](#1-nodejs-简介)
- [2. 基本组件](#2-基本组件)
- [3. Node.js工作流程](#3-nodejs工作流程)
- [4. JavaScript是单线程](#4-)


---

### [1. Node.js 简介](#)
Node.js 是一个开源和跨平台的 JavaScript 运行时环境。它是几乎所有类型项目的流行工具！Node.js主要分为四大部分，Node Standard Library，Node Bindings，V8，Libuv，架构图如下:

<img src="./static/LLLLLLL123A156SDimage.png" width="400" />

- `Node Standard Library` 是我们每天都在用的标准库，如Http, Buffer 模块。
- `Node Bindings` 是沟通JS 和 C++的桥梁，封装V8和Libuv的细节，向上层提供基础API服务。
- 这一层是支撑 Node.js 运行的关键，由 C/C++ 实现。
  - V8 是Google开发的JavaScript引擎，提供JavaScript运行环境，可以说它就是 Node.js 的发动机。
  - Libuv 是专门为Node.js开发的一个封装库，提供跨平台的异步I/O能力.
  - C-ares：提供了异步处理 DNS 相关的能力。
  - http_parser、OpenSSL、zlib 等：提供包括 http 解析、SSL、数据压缩等其他的能力。

nodejs v1.0 也就是最早发布的 node 版本下的 deps 文件，也就是 nodejs 所用到的依赖:
- `cares`：用 C-ares 做域名解析
- `gtest`：是 C/C++ 的单元测试框架
- `http-parser`：用来解析 HTTP
- `npm`：包管理工具
- `openssl`：用来解析 HTTPS
- `uv`：一个跨平台的异步 I/O 库
- `v8`：google 开发的js引擎，为 js 提供运行环境
- `zlib`：用来做加密


#### [1.1 Node bindings](#)
C/C++ 实现了一个用来解析 HTTP 的库 http-parser，非常高效，可是对于只会写 js 的程序员非常的不友好，因为没有办法直接去调用这个 C/C++ 的库，这两个语言连最基本的数据类型都不一样。

结论：js 无法直接调用 C++ 的库，需要一个中间的桥梁（调用途径）。

Node.js 的作者 Ryan 做了一个中间层处理
* Node.js 用 C++ 对 http-parse 进行封装，使它符合某些要求（比如统一数据类型），封装好的文件叫做 http_parse_binding.cpp
* 用 Node.js 提供的编译工具将其编译为 .node 文件
* js 代码可以直接通过 require 关键字引入这个 .node 文件

这样 js 就能够调用 C++ 库，这个中间的桥梁就是 bindings，由于 node 提供了很多 binding，所以就叫做 Node bindings


#### [1.2  JS如何与C++通信](#)

```javascript
// test.js
const addon = require('./build/Release/addon');

console.log('This should be eight:', addon.add(3, 5));
```
上面是js调用，再来看看C++代码（已被编译）

```javascript
// addon.cc
#include <node.h>

namespace demo {

using v8::Exception;
using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::NewStringType;
using v8::Number;
using v8::Object;
using v8::String;
using v8::Value;

// 这是 "add" 方法的实现。
// 输入参数使用 const FunctionCallbackInfo<Value>& args 结构传入。
void Add(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  // 检查传入的参数的个数。
  if (args.Length() < 2) {
    // 抛出一个错误并传回到 JavaScript。
    isolate->ThrowException(Exception::TypeError(
        String::NewFromUtf8(isolate, 
                        "参数的数量错误",
                            NewStringType::kNormal).ToLocalChecked()));
    return;
  }

  // 检查参数的类型。
  if (!args[0]->IsNumber() || !args[1]->IsNumber()) {
    isolate->ThrowException(Exception::TypeError(
        String::NewFromUtf8(isolate, 
                        "参数错误",
                            NewStringType::kNormal).ToLocalChecked()));
    return;
  }

  // 执行操作
  double value =
      args[0].As<Number>()->Value() + args[1].As<Number>()->Value();
  Local<Number> num = Number::New(isolate, value);

  // 设置返回值 (使用传入的 FunctionCallbackInfo<Value>&)。
  args.GetReturnValue().Set(num);
}

void Init(Local<Object> exports) {
  NODE_SET_METHOD(exports, "add", Add);
}

NODE_MODULE(NODE
_GYP_MODULE_NAME, Init)

}  // 命名空间示例
```
Nodejs 封装的插件开放一些对象和函数，供运行在 Node.js 中的 js 访问，当 js 调用函数 addon 时，输入参数和返回值与 C/C++ 代码相互映射，统一封装处理。这样就可以直接在 Node.js 中引入并使用。

#### [1.3 C++调用JS回调](#)

```javascript
// test.js
const addon = require('./build/Release/addon');
// 传入一个函数
addon((msg) => {
  console.log(msg);
// 打印: 'hello world'
});
```
传入C++并执行
```javascript
// addon.cc
#include <node.h>

namespace demo {

using v8::Context;
using v8::Function;
using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::NewStringType;
using v8::Null;
using v8::Object;
using v8::String;
using v8::Value;

void RunCallback(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();
  Local<Context> context = isolate->GetCurrentContext();
  Local<Function> cb = Local<Function>::Cast(args[0]);
  const unsigned argc = 1;
  // 这里有一个c++方法，将args[0]也就是我们传入的函数，转化成c++看得懂的，用cb接收
  Local<Value> argv[argc] = {
      String::NewFromUtf8(isolate,
                          "hello world",
                          NewStringType::kNormal).ToLocalChecked() };
  // 调用一下，传入的函数就被调用了，打印出hello world
  cb->Call(context, Null(isolate), argc, argv).ToLocalChecked();
}

void Init(Local<Object> exports, Local<Object> module) {
  NODE_SET_METHOD(module, "exports", RunCallback);
}

NODE_MODULE(NODE_GYP_MODULE_NAME, Init)

}
```
回调函数被同步地调用，要知道 C++ 是看不懂 js 的，所以如何做中间层的封装就交给这些 Node 插件去做,有了这些 Node.js 提供的插件（node binding），JS和C++就可以进行交互了，也使JS的能力被大大的扩展了。

### [2. 基本组件](#)

#### [2.1 什么是V8](#)
V8引擎是一个 JavaScript 引擎实现，最初由一些语言方面专家设计，后被谷歌收购，随后谷歌对其进行了开源
V8使用 C++ 开发，在运行 JavaScript 之前，相比其它的 JavaScript 的引擎转换成字节码或解释执行，V8 将其编译成原生机器码。
JavaScript 程序在 V8 引擎下的运行速度媲美二进制程序，它是现阶段执行 JavaScript 最快的一个引擎


- 将 JS 源代码变成本地代码并执行
- 维护调用栈，确保 JS 函数的执行顺序
- 内存管理，为所有对象分配内存
- 垃圾回收，重复利用无用的内存
- 实现 JS 的标准库

js 是单线程的，而 V8 本身是多线程的，开一个线程执行 js，开一个线程清理内存，然后再处理一些其他别的活儿，线程和线程之间毫无瓜葛。

#### [2.2 什么是libuv](#)
因为各个系统的 I/O 库都不一样，windows 系统有 IOCP，Linux 系统有 epoll。Node.js 的作者Ryan 为了将其整合在一起实现一个跨平台的异步 I/O 库，开始写 libuv。

- 从操作系统写文件到硬盘
- 访问网络，从操作系统发出数据到别的服务器
- 打印连接打印机，从操作系统发指令给打印机

以上这些行为都是 I/O，可以理解为系统和外界进行交互的过程都叫 I/O

而 libuv 会根据你是什么系统，自动的选择当前系统已经实现好了的异步操作（I/O）库，用于TCP/UDP/DNS文件等的异步操作

- 比如操作 TCP，我们都知道 HTTP 是基于 TCP/IP 的，如果可以操作 TCP 那么，就可以做 HTTP 的服务
- UDP，用于实时通信，常见的 QQ 聊天解析 DNS

包括读文件、写文件什么的，libuv 都可以帮你管理。这样 I/O 的部分就全部交给 C 语言去做，js 完全不用管，甩手掌柜，负责调用就行了

v8 和 libuv 在整个 Node.js 架构的底层是最为重要的

### [3. Node.js工作流程](#)
所有现代系统都是主程序启动完毕之后，对每个收到的请求开启一个进程，接下来根据不同技术有不同的处理方式，有时差异会大相径庭。典型的实现是：针对一个请求开启一个线程，一步接一步执行任务操作，如果某个操作执行缓慢，这个线程上的后续操作都会随之挂起，直到所有操作完成，返回结果。而在 Node.js 中，所有的操作都注册为一个事件，等待主程序或者外部请求来触发（**事件驱动**）。


<img src="./static/ASDAS244QWEQW1image.png" width="600" />


**Application** 就是咱们写的代码，把它放在 `V8` 上面去运行。发现需要去读一个文件，这时候 `libuv` 开一个线程去读文件。读完文件，操作系统会返回一个事件给 `event loop`，`event loop` 就把文件传回给 `V8`，再给到代码。

#### [3.1 Node.js中的Event Loop](#)
"Event Loop" 是一个程序结构，用于等待和发送消息和事件。

JavaScript 是单线程的，有了 Event Loop 的加持，Node.js 才可以非阻塞地执行 I/O 操作，把这些操作尽量转移给操作系统来执行

**Event**: 计时器到期了、文件可以读取了、读取出错了
**Loop **: Loop 就是循环，由于事件分优先级，所以处理起来也是分先后顺序，所以 Node.js 需要按顺序轮询每种事件，轮询是循环的。

**Event Loop 各阶段详解**

```
   ┌───────────────────────┐
┌─>│        timers         │检查计时器
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     I/O callbacks     │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     idle, prepare     │打扫战场，可以忽略
│  └──────────┬────────────┘      ┌─────────┐
│  ┌──────────┴────────────┐      │   incoming:   │
│  │         poll          │检查系统事件  <─────┤  connections, │
│  └──────────┬────────────┘      │   data, etc.  │
│  ┌──────────┴────────────┐      └─────────┘
│  │        check          │检查setImmediate回调
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
└──┤    close callbacks    │执行关闭事件的回调函数
   └───────────────────────┘
```
上图其中每个方框都是 Event Loop 中的一个阶段，每个阶段都有一个「先入先出队列」，这个队列存有要执行的回调函数

**timer 阶段**

先看看有没有计时器，有了执行回调函数

需要注意的是：当计时器指定的时间达到后，回调函数会尽早被执行。如果操作系统很忙，或者 Node.js 正在执行一个耗时的函数，那么计时器的回调函数就会被推迟执行

举个例子，置了一个计时器在 100 毫秒后执行，然后你的脚本用了 95 毫秒来异步读取了一个文件

**I/O 阶段**

这个阶段会执行一些系统操作的回调函数，还有没有其他没有归类的回调（没有归类的回调：不在 timers 阶段、close callbacks 阶段和 check 阶段这三个阶段执行的回调）

**Idle 阶段**

空闲一会儿，清理战场

**Poll 阶段**

轮询阶段，处理大部分的事件（文件可读了？读！http请求来了，处理！）

当 event loop 进入 poll 阶段，如果发现没有计时器，就会：

- 如果 poll 队列不是空的，event loop 就会依次执行队列里的回调函数，直到队列被清空或者到达 poll 阶段的时间上限。
- 如果 poll 队列是空的，就会：
    - 如果有 setImmediate() 任务，event loop 就结束 poll 阶段去往 check 阶段。
    - 如果没有 setImmediate() 任务，event loop 就会等待新的回调函数进入 poll 队列，并立即执行它。
- 一旦 poll 队列为空，event loop 就会检查计时器有没有到期，如果有计时器到期了，event loop 就会回到 timers 阶段执行计时器的调

**Check 阶段**

如果 poll 阶段空闲了，同时存在 setImmediate() 任务，event loop 就会进入 check 阶段

setImmediate() 是通过 libuv 里一个能将回调安排在 poll 阶段之后执行的 API 实现的

**Close callback 阶段**
比如 socket.destroy()，就会有一个 close 事件进入这个阶段

`------------循环-----------------`

但是 Node.js 不傻，不会一直循环循环，如果发现没什么事儿做，就会停留在 poll（轮询）阶段

轮询的阶段呢，会看看有没有文件可以读，有没有请求可以处理，就等着，时不时的看看有没有新的代码，或者检查一下最近的计时器，看看有没有需要过会儿去执行的 callback

如果计时器事件要处理了，我再从下出发，绕回 timers

Node.js 大部分时间都会停留在 poll 阶段，大部分事件都在 poll 阶段被处理，如文件、网络请求

程序结束时，Node.js 会检查 Event Loop 是否在等待异步 I/O 操作结束，是否在等待计时器触发，如果没有，就会关掉 Event Loop

#### [3.2 流程总结](#)
Application就是咱们写的代码，把它放在v8上面去运行

- 运行的过程中，发现我们写了个setTimeout，v8就会调用Node.js的bindings，把这个settimeout放进Even loop里面。
- Event loop就会等待适合的时机去发送一个事件去执行这些 js 代码，接着循环等待，一般停留在 poll阶段久一些。
- 发现需要去读一个文件，这时候 Event loop 就会通过 libuv 开一个线程去专门做读文件这事儿。
- 读完文件，操作系统会返回一个事件给 Event loop，Event loop 就把文件传回给v8，最后给到代码。

**js 从头至尾都不参与读文件这个事情，libuv去读**

一句话就是，代码到 V8，通过Node API 使用libuv 和其他一些 C/C++ 提供的功能去完成用户所需要的功能

Node.js 将这些模块进行整合，所以说 Node.js 不是一门语言，就是一个结合技术的平台。

### [4. JavaScript 运行机制](#)

#### [4.1 为什么JavaScript是单线程](#)
JavaScript语言的一大特点就是单线程，这与它的用途有关。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？

所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变。

**注意**：为了利用多核CPU的计算能力，HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程，但是子线程完全受主线程控制，且不得操作DOM。所以，这个新标准并没有改变JavaScript单线程的本质。

#### [5.2 任务队列](#)
单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。

如果排队是因为计算量大，CPU忙不过来，倒也算了，但是很多时候CPU是闲着的，因为IO设备（输入输出设备）很慢（比如Ajax操作从网络读取数据），不得不等着结果出来，再往下执行。

JavaScript语言的设计者意识到，这时主线程完全可以不管IO设备，挂起处于等待中的任务，先运行排在后面的任务。等到IO设备返回了结果，再回过头，把挂起的任务继续执行下去。

于是，所有任务可以分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous）

具体来说，异步执行的运行机制如下。（同步执行也是如此，因为它可以被视为没有异步任务的异步执行。）

1. 所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
2. 主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
3. 一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
4. 主线程不断重复上面的第三步。

只要主线程空了，就会去读取"任务队列"，这就是JavaScript的运行机制。这个过程会不断重复。

#### [5.3 事件和回调函数](#)
"任务队列"是一个事件的队列（也可以理解成消息的队列），IO设备完成一项任务，就在"任务队列"中添加一个事件，表示相关的异步任务可以进入"执行栈"了。主线程读取"任务队列"，就是读取里面有哪些事件。

"任务队列"中的事件，除了IO设备的事件以外，还包括一些用户产生的事件（比如鼠标点击、页面滚动等等）。只要指定过回调函数，这些事件发生时就会进入"任务队列"，等待主线程读取。

所谓"回调函数"（callback），就是那些会被主线程挂起来的代码。异步任务必须指定回调函数，当主线程开始执行异步任务，就是执行对应的回调函数。

**"任务队列"是一个先进先出的数据结构**，排在前面的事件，优先被主线程读取。主线程的读取过程基本上是自动的，只要执行栈一清空，"任务队列"上第一位的事件就自动进入主线程。但是，由于存在后文提到的"定时器"功能，主线程首先要检查一下执行时间，某些事件只有到了规定的时间，才能返回主线程。


#### [5.4 Node.js 运行](#)
Node.js 应用在单个进程中运行，不会为每个请求创建新线程。Node.js 在其标准库中提供了一组异步 I/O 原语，可防止 JavaScript 代码阻塞，并且通常，Node.js 中的库是使用非阻塞范例编写的，这使得阻塞行为成为例外而不是常态。

当 Node.js 执行 I/O 操作（如从网络读取、访问数据库或文件系统）时，Node.js 不会阻塞线程并浪费 CPU 周期等待，而是会在响应返回时恢复操作。

这使 Node.js 能够使用单个服务器处理数千个并发连接，而​​不会带来管理线程并发的负担，这可能是错误的重要来源。

Node.js 具有独特的优势，因为数百万为浏览器编写 JavaScript 的前端开发者现在能够编写服务器端代码以及客户端代码，而无需学习完全不同的语言。

在 Node.js 中，可以毫无问题地使用新的 ECMAScript 标准，因为你不必等待所有用户更新其浏览器 - 你可以通过更改 Node.js 版本来决定使用哪个 ECMAScript 版本，并且还可以通过使用标志运行 Node.js 来启用特定的实验性功能。

Node.js 最常见的示例 Hello World 是一个 Web 服务器：
```javascript
const { createServer } = require('node:http');

const hostname = '127.0.0.1';
const port = 3000;

const server = createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

### [6. npm 包管理器简介](#)
npm 是 Node.js 的标准包管理器,地球上最大的单一语言代码存储库，你可以肯定，几乎所有内容都有一个软件包！

它最初是一种下载和管理 Node.js 包依赖的方法，但后来也成为前端 JavaScript 中使用的工具。Yarn 和 pnpm 是 npm cli 的替代品。你也可以查看它们。

npm 安装、更新和管理项目依赖的下载。依赖是预先构建的代码片段，例如库和包，你的 Node.js 应用需要它们才能运行。


**安装所有依赖** 如果项目有一个 package.json 文件，通过运行如下命令，它将在 node_modules 文件夹中安装项目所需的所有内容，如果尚不存在，则创建它。
```shell
npm install
```
**安装单个包** ,你也可以通过运行如下命令来安装特定的包，此外，从 npm 5 开始，此命令将 `<package-name>` 添加到 `package.json` 文件依赖, 在版本 5 之前，你需要添加标志 `--save`。
```
npm install <package-name>
```
- `--no-save` 安装但不将条目添加到 package.json 文件依赖
- `--save-dev` 安装并将条目添加到 package.json 文件 devDependencies
- `--save-optional` 安装并将条目添加到 package.json 文件 optionalDependencies
- `--no-optional` 将阻止安装可选依赖

也可以使用标志的简写：
- `-S`:`--save`
- `-D`:`--save-dev`
- `-O`:`--save-optional`

devDependencies 和 dependency 之间的区别在于前者包含开发工具，如测试库，而后者在生产中与应用打包在一起。

### [参考文章](#)
- [https://nodejs.cn/en/learn](https://nodejs.cn/en/learn)