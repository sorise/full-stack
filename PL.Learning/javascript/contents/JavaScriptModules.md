## [Javasript 模块化](#)
> **介绍** 将一个复杂的程序依据一定的规则(规范)封装成几个块(文件), 并进行组合在一起，块的内部数据与实现是私有的, 只是向外部暴露一些接口(方法)与外部其它模块通信。

---

### [1. JavaScript 模块化的历史](#1-javascript-模块化的历史)
JavaScript 模块化的历史可以追溯到早期的开发实践，但直到近年来才在语言层面得到了更好的支持和规范化。

- **早期开发实践**（2000年前）：在 JavaScript 的早期，开发者通常使用**全局变量**和**命名空间**来组织代码，这种方式容易导致命名冲突和代码混乱，不利于大型应用的开发和维护。
- **CommonJS**（2009年）：**CommonJS** 是 **Node.js** 开始使用的模块系统规范，由于越来越复杂的后端编写代码的需求。再继续像以前那样将JavaScript文件写在一起已经远远不能满足开发的需求了，于是它通过 `require` 和 `module.exports` 实现模块的导入和导出，使得 JavaScript 在服务器端的模块化变得更加简单和可靠。
- **AMD**（2011年）：AMD（Asynchronous Module Definition）是为浏览器环境设计的模块化规范，最初由 `RequireJS` 提出。AMD 支持异步加载模块，在 Web 开发中得到了一定的应用。
- **ES6 Modules** （2015年）：随着 ECMAScript 2015（ES6）的发布，JavaScript 正式引入了官方的模块化系统，即 ES Modules。
ES Modules 支持静态导入和导出，通过 `import` 和 `export` 关键字实现模块的声明和引用。
**ES Modules** 提供了更加简洁、清晰的模块化语法，并且得到了现代浏览器和 `Node.js` 的广泛支持。

传统的脚本方式逐渐暴露出许多问题：
* **命名冲突**：不同脚本文件中的变量容易出现命名冲突，导致难以调试。
* **依赖管理复杂**：需要手动维护脚本之间的依赖关系，这种方式非常脆弱且容易出错。
* **代码复用性差**：代码没有统一的模块规范，无法实现有效的代码复用。

### [2. CommonJS](#)
CommonJS 是 Node.js 采用的模块化规范，主要用于服务端的 JavaScript 环境。
根据 CommonJS 规范，一个单独的文件就是一个模块，每一个模块都是一个单独的作用域，在一个文件定义的变量（还包括函数和类），都是私有的，对其他文件是不可见的。

CommonJS 通过 `require()` 函数同步加载依赖模块，并使用 `module.exports` 导出模块成员。
```javascript
// CommonJS2也可以通过这种方式导出
module.exports = {
    a: 1
}
// CommonJS1只能通过这种方式
exports.a = 1

// b.js
var module = require('./a.js')
module.a // -> log 1
```
**CommonJS 的特性**:
* **同步加载**：模块在代码运行时同步加载，适用于服务端，但不适用于浏览器环境，因为浏览器环境中同步加载会阻塞渲染进程。
* **缓存机制**：同一个模块在多次加载时会被缓存，除非明确清除缓存。
* **简单易用**：通过 require 和 module.exports 实现模块的导入和导出，简单直观。

[CommonJS 本质 视频讲解 【渡一教育】](https://www.bilibili.com/video/BV15P1PYdEiB/?spm_id_from=333.337.search-card.all.click&vd_source=a03ca1a86c1e90990c4facd27ae17815)

#### [2.1 module 是个什么东西](#)
在 Node.js 源码中可以找到其对 CommonJS 模块机制的具体实现，源码中的具体实现相对较复杂，为了方便大家更容易理解 CommonJS 的核心实现原理，我对源码做了一些精简
```javascript
function Module(id = '', parent) {
	// 模块文件的绝对路径
	this.id = id;
	// 模块所在的目录
	this.path = path.dirname(id);
	// 导出内容
	this.exports = {};
	// 将该模块添加到父模块的children数组中
	updateChildren(parent, this, false);
	this.filename = null;
	// 是否加载完成
	this.loaded = false;
	// 引用的子模块，即在当前模块中通过require加载的模块
	this.children = [];
}

// 用于包裹模块代码
Module.wrap = function (script) {
	return (
		'(function (exports, require, module, __filename, __dirname) {' +
		script +
		'\n})'
	);
};

// 模块缓存
Module._cache = Object.create(null);

Module._load = function (request, parent) {
	// Module._resolveFilename 用于将 require 的模块地址解析成能定位到模块的绝对路径
	const filename = Module._resolveFilename(request, parent);

	const cachedModule = Module._cache[filename];
	// 命中缓存直接返回
	if (cachedModule) {
		return cachedModule.exports;
	}

	// 未命中缓存，则创建一个新的模块实例
	const module = new Module(filename, parent);

	// 将该模块记录到模块缓存中
	Module._cache[filename] = module;

	// 获取到模块代码内容
	const content = fs.readFileSync(filename, 'utf8');

	// 将参数传入并执行模块代码，可以理解为eval，但源码中的实现并不是eval，而是更安全的执行方式
	runInThisContext(Module.wrap(content))(
		module.exports, // 对应包裹函数中接收的 exports 形参
		module.require, // 对应包裹函数中接收的 require 形参
		module, // 对应包裹函数中接收的 module 形参
		filename, // 对应包裹函数中接收的 __filename 形参
		module.path // 对应包裹函数中接收的 __dirname 形参
	);

	// 将该模块标识为加载完成
	module.loaded = true;

	// 最后返回执行完该模块内代码后的模块导出内容
	return module.exports;
};

// 模块原型上的require方法
Module.prototype.require = function (id) {
	// ...
	return Module._load(id, this);
};
```
通过上面的源码我们应该知道了，module是通过new Module()实例化得到的一个CommonJS模块实例，它包含了该模块的一些信息，下面我打印了一下module：
```json lines
{
  id: '.',
  path: 'F:\\GitRepo\\front-end\\ponder\\scripts',
  exports: {},
  filename: 'F:\\GitRepo\\front-end\\ponder\\scripts\\op.js',
  loaded: false,
  children: [
    {
      id: 'F:\\GitRepo\\front-end\\ponder\\scripts\\me.js',
      path: 'F:\\GitRepo\\front-end\\ponder\\scripts',
      exports: [Object],
      filename: 'F:\\GitRepo\\front-end\\ponder\\scripts\\me.js',
      loaded: true,
      children: [],
      paths: [Array]
    }
  ],
  paths: [
    'F:\\GitRepo\\front-end\\ponder\\scripts\\node_modules',
    'F:\\GitRepo\\front-end\\ponder\\node_modules',
    'F:\\GitRepo\\front-end\\node_modules',
    'F:\\GitRepo\\node_modules',
    'F:\\node_modules'
  ]
}
```
**module.id**是每个模块的唯一标识ID，通常是模块文件的在文件系统中的绝对路径，但是也存在module.id是'.'的模块，这表示该模块是执行的入口模块process.mainModule，比如你在命令行中执行:
```shell
$ node main.js
```
那么main.js这个模块就是Node进程的入口模块。在后面讲解require的时候会再次介绍到它。
* **module.path** 模块的目录名，通常与path.dirname(module.id)相同。
* **module.exports** 模块导出的内容，默认是一个对象{}，你也可以将任何你想导出的内容赋值给module.exports，以将其作为模块的导出内容。
* **module.filename** 模块的完整解析文件名，一个绝对路径，通过它能够准确定位到模块文件。
* **module.loaded** 一个用于标识模块是否加载完成的状态标识，为true时表示该模块已经加载完成。
* **module.children** 该模块的子模块数组，比如foo模块中加载了bar模块和baz模块，则bar模块和baz模块都是foo模块的子模块。
* **module.paths** 模块查找的路径数组，更准确地说，是第三方模块的查找路径数组。
* **module.parent** 模块的父模块，比如你在模块a.js中require了模块b.js，那么模块b.js的父模块就是模块a.js。注意：该属性从Node.js v14.6.0、v12.19.0版本开始已弃用。

#### [2.2 module、module.exports、exports、require](#)
之所以能直接在模块代码中使用module、module.exports、exports、require，是因为在模块加载过程中CommonJS模块系统帮我们包裹了一层匿名函数，并在执行该匿名函数时将其接收的5个参数传入，这5个参数分别是：

* `exports`，等同于module.exports，作为module.exports的快捷访问；
* `require`，通常为模块原型上的require方法，用于加载其他模块；
* `module`，当前模块对象，通过new Module()实例化创建的；
* `__filename`，当前模块的绝对路径文件名；
* `__dirname`，当前模块所在目录的绝对路径；

所以我们编写的代码实际上是在该函数内部执行的，我们使用的module、exports、require等其实都是该函数的形参，并不是什么全局变量。通过上面的源码我们还可以知道，除了module、module.exports、exports和require，还有__filename和__dirname这两个变量我们也是可以直接在模块代码中使用的。

```javascript
// 用于包裹模块代码
Module.wrap = function(script) {
        return '(function (exports, require, module, __filename, __dirname) {' +
        script +
    '\n})'
}
```

**为什么这种方式可以实现模块化的效果**?

这是因为我们的模块代码最终是被放在一个匿名函数内部执行的，每个模块都处于单独的函数作用域，所以不会造成
污染全局变量的问题。并且通过module.exports实现了模块导出，可以将模块内部的某些代码共享给其他模块复
用，通过require可以方便地加载其他模块的导出内容供当前模块使用。

#### [2.3 exports和module.exports有什么联系和区别](#)
module.exports是整个模块的导出内容，默认是个 `{}`， exports其实就是module.exports，是对同一个对象的引用，它可以作为
对module.exports的快捷访问。但是需要注意的是，如果你对module.export或者exports进行了一些破坏性的赋值操作，这种操作
将导致exports和module.exports引用的不再是同一个对象，也就意味着exports和module.exports之间的联系被切断了，我们结合wrap函数来理解：
```javascript
function wrap(exports, require, module, __filename, __dirname) {
    // exports 只是 wrap 函数的一个形参，就相当于在函数内部存在的一个变量
    // 此时它被赋值为一个新对象，那么它跟 module.exports 就没有关系了
    exports = {
        name: '一知'
    }

    // 这个 age 只会出现在 module.exports 上，在 exports 中不存在
    module.exports.age = 18
}

// 之所以 exports 会等于 module.exports，是因为在执行 wrap 函数时传入的 exports 实参是 module.exports
wrap(module.exports, require, module, __filename, __dirname)
```
比如下面这样，module.exports被赋值为一个新的对象，而此时exports还是最开始module.exports引用的那个对象，所以之后我们再给exports对象上添加age属性，也并不会影响到module.exports，因此该模块的导出内容（即module.exports）实际上是{ name: '一知' }，而不是{ name: '一知', age: 18 }。

```javascript
// bar.js
exports = {
    name: '一知'
}

module.exports.age = 18
```

```javascript
// foo.js
const bar = require('./bar.js')

console.log(bar) // { age: 18 }
```
下面这样的用法也是同样的问题：
```javascript
// bar.js
module.exports = {
    name: '一知'
}

exports.age = 18
```
```javascript
// foo.js
const bar = require('./bar.js')

console.log(bar) // { name: '一知' }
```
**所以我们一般会这样用**：
```javascript
module.exports = {
    name: '一知',
    age: 18,
    sayHello() {
        // ...
    },
    ...
}
```
或者：
```javascript
exports.name = '一知'
exports.age = 18
exports.sayHello = function sayHello() {
    // ...
}
```
`module.exports` 通常会是一个对象，但也可以是其他任何值，比如函数、字符串等等。
```javascript
// 导出一个函数
module.exports = function() {
    // ...
}
```

#### [2.4 require的查找机制是怎样的](#)
在回答这个问题之前，我们先来了解一下关于require的几个辅助属性。

##### require.main

等同于process.mainModule，二者是对同一个对象的引用。表示 Node.js 进程启动时加载的入口脚本的模块对象，如果程序的入口点不是 CommonJS 模块，则为 undefined。 比如：

bar.js：
```javascript
module.exports = 1
```

foo.js：
```javascript
const bar = require('bar')
exports.foo = 2 + bar // 3
```
那么当我们在命令行中执行如下命令时
```shell
node foo.js
```
foo.js这个模块将作为入口模块，即process.mainModule，也即require.main

##### require.cache
等同于前面源码中的Module._cache，同一个对象引用，记录了模块加载的缓存。

##### require.resolve
一个辅助方法，用于将模块标识符解析成一个可以定位到模块文件的filename（比如原生模块'fs'、绝对路径 `/path/to/module/xxx.js`），
其内部实现也是调用了前面源码中的 `Module._resolveFilename` 方法。

**require过程**
- 首先，将模块标识解析（通过 `Module._resolveFilename()` ）成能定位到模块文件的一个filename字符串，通常为一个绝对路径或者原生模块标识符如'fs'；
- 通过filename读取到模块内容（针对非原生模块）；
- 将模块内容包裹在module wrapper函数中执行（针对非原生模块）；
- 返回 `module.exports` 作为导出内容，`require` 加载完毕。

那么当我们 **require(X)** 时，CommonJS模块系统是如何将这个模块标识符X解析成相应模块的地址，从而找到这个模块的呢？
我们通常require的场景有这几种：
- 加载Node.js内置模块，如 fs、path等；
- 加载相对路径或绝对路径文件模块，如 `./xxx、../xxx` 和 `/xxx` 等；
- 加载第三方模块，比如 express、koa 等。

##### 加载原生模块 
```javascript
const path = require('path')
const fs = require('node:fs')
```
如果模块加载标识符是原生模块标识符如fs，那么经过Module._resolveFilename方法解析后得到的filename还是fs，然后
判断Module_cache['fs']是否命中缓存，如果缓存命中，则返回该缓存模块的module.exports，require结束；

若未命中缓存则加载该原生模块，然后返回其module.exports，require结束。在Node.js中原生模块被编译成了二
进制代码，所以相较于非原生模块加载速度更快。

从`Node.js v16.0.0`和`v14.18.0`开始，加载原生内置模块可以加上node:前缀，这种方式可以跳过模块缓存检查，直接加载相应原生模块，例如：

```javascript
 const realFs = require('node:fs')

const fakeFs = {}
require.cache.fs = { exports: fakeFs }

console.log(require('fs') === fakeFs) // true
console.log(require('node:fs') === fakeFs) // false
console.log(require('node:fs') === realFs) // true
```
我们可以这样获取到Node.js中的原生模块，通过require('module')获取到Module，注意，这个module并不是
我们上面介绍的模块代码中module.exports中的那个module，而是用来创建模块的new Module()的这个Module构造函数。

其中Module.builtinModules数组就是Node.js提供的所有原生内置模块的标识符，我们最常用的fs和path等都在其中。
javascript 代码解读复制代码。

```javascript
const mod = require('module')

console.log(mod.builtinModules)
// 以下为所有内置元素模块：
[
    	  '_http_agent',         '_http_client',        '_http_common',
    	  '_http_incoming',      '_http_outgoing',      '_http_server',
    	  '_stream_duplex',      '_stream_passthrough', '_stream_readable',
    	  '_stream_transform',   '_stream_wrap',        '_stream_writable',
    	  '_tls_common',         '_tls_wrap',           'assert',
    	  'assert/strict',       'async_hooks',         'buffer',
    	  'child_process',       'cluster',             'console',
    	  'constants',           'crypto',              'dgram',
    	  'diagnostics_channel', 'dns',                 'dns/promises',
    	  'domain',              'events',              'fs',
    	  'fs/promises',         'http',                'http2',
    	  'https',               'inspector',           'module',
    	  'net',                 'os',                  'path',
    	  'path/posix',          'path/win32',          'perf_hooks',
    	  'process',             'punycode',            'querystring',
    	  'readline',            'repl',                'stream',
    	  'stream/consumers',    'stream/promises',     'stream/web',
    	  'string_decoder',      'sys',                 'timers',
    	  'timers/promises',     'tls',                 'trace_events',
    	  'tty',                 'url',                 'util',
    	  'util/types',          'v8',                  'vm',
    	  'worker_threads',      'zlib'
]
```

##### 加载相对路径或绝对路径的本地模块
```javascript
// foo.js
const koa = require('koa')
```
如果是第三方模块，则模块系统将从当前目录下的 `node_modules` 目录中开始，查找是否存在该第三方模块标识符对应的文件
或者目录，如果找不到，则继续往上一级的node_modules目录找，一直到根目录下的node_modules目录，如果根目录下的
node_modules中也没找到，则抛出MODULE_NOT_FOUND的错误。

比如你当前执行require('koa')的加载模块的文件是 `/root/path/to/module/foo.js`，那么模块系统将依次尝试在
以下node_modules目录中查找对应模块：

```javascript
[
    '/root/path/to/module/node_modules',
    '/root/path/to/node_modules',
    '/root/path/node_modules',
    '/root/node_modules',
    '/node_modules',
]
```
在 `node_modules` 目录下查找这个模块时的过程，与前面相对路径和绝对路径的查找过程一样，会进行文件推断、添加文件
扩展名、目录推断、package.json等尝试，直到找到该模块，然后后面的加载执行的过程和上面一样。

##### 如果出现循环引用会怎样 
即使出现循环引用，也不会出现无限循环的问题。

使用require()加载一个模块时，由于存在缓存（require.cache，即Module._cache）机制，加载模块时会先判断是否存在该模块的缓存，如果存在，就会直接返回缓存中该模块的module.exports，而不会再次执行模块代码。

CommonJS 中模块加载的策略是 **深度优先遍历**，类似递归的过程。

a.js：
```javascript
console.log('a.js 开始执行');
exports.done = false;
const b = require('./b.js');
console.log('a.js 中, b.done = %j', b.done);
exports.done = true;
console.log('a.js 执行完成');
```
b.js：
```javascript
console.log('b.js 开始执行');
exports.done = false;
const a = require('./a.js');
console.log('b.js 中, a.done = %j', a.done);
exports.done = true;
console.log('b.js 执行完成');
```
main.js
```javascript
console.log('main.js 开始执行');
const a = require('./a.js');
const b = require('./b.js');
console.log('main.js 中, a.done = %j, b.done = %j', a.done, b.done);
```
运行一下：
```shell
$ node main.js
main.js 开始执行
a.js 开始执行
b.js 开始执行
b.js 中, a.done = false
b.js 执行完成
a.js 中, b.done = true
a.js 执行完成
main.js 中, a.done = true, b.done = true
```

#### [2.5 CommonJS模块识别](#)
默认情况下，Node.js 会将以下内容视为 CommonJS 模块：

* 扩展名为`.cjs`的文件；
* 扩展名为`.js`的文件，并且离它最近的（与它同级或在它的父级目录）**package.json** 文件中包含了值为`"commonjs"`的顶级 `type` 字段；
* 扩展名为`.js`的文件，并且离它最近的（与它同级或在它的父级目录）**package.json** 文件中未指定顶级 `"type"` 字段；
* 扩展名为 `.mjs`、`.cjs`、`.json`、`.node` 或 `.js` 的文件，当离它最近的（与它同级或在它的父级目录） **package.json** 
文件包含值为 `"module"` 的顶级字段 `"type"` 时，这些文件只有在 `require` 它们时才会被识别为 CommonJS 模块，而不是在用作程序的命令行入口点时）。

### [3. AMD 模块](#)
AMD（Asynchronous Module Definition，异步模块定义）是一个在浏览器环境中使用的模块化规范。 
它解决了 CommonJS 在浏览器中同步加载的问题，使用异步加载方式来加载模块。

* **异步加载**：通过异步方式加载模块，适合在浏览器环境下使用，避免了浏览器渲染的阻塞问题。
* **依赖前置**：在定义模块时需要声明所有的依赖模块，这些模块会在代码运行前加载完成。
* **较复杂的定义方式**：需要使用 define() 函数来定义模块，并声明依赖。

```javascript
// math.js
define([], function() {
  const add = (a, b) => a + b;
  const subtract = (a, b) => a - b;
  return {
    add,
    subtract
  };
});

// main.js
require(['./math'], function(math) {
  console.log(math.add(1, 2)); // 输出: 3
  console.log(math.subtract(5, 3)); // 输出: 2
});
```

#### [3.1 AMD 可能存在的问题](#)
虽然 AMD 规范在解决浏览器环境中模块异步加载方面有显著的优势，但它也存在一些潜在的问题和局限性：

- **模块定义复杂性增加**：AMD 使用 define() 函数来定义模块，并且需要提前声明所有的依赖模块。这种显式声明的方式虽然在一定程度上清晰明了，但在大型项目中会显得繁琐复杂，特别是当依赖关系较多时，代码的可读性和维护性会下降。
- **加载速度较慢**：尽管 AMD 通过异步方式加载模块来避免阻塞浏览器渲染进程，但由于模块依赖的前置加载特性，所有依赖模块需要在主模块执行之前全部加载完毕。这在依赖关系复杂或者网络较差的情况下，可能导致模块加载速度变慢，影响页面性能。
- **过度依赖回调函数**：AMD 模块化规范依赖于回调函数，这会导致代码结构的嵌套层级增加，出现俗称的“回调地狱”现象，使得代码的调试和维护变得更加困难。
- **生态系统和工具支持限制**：相比于 ES6 Module 等更现代的模块化标准，AMD 的生态系统支持较为有限。虽然 RequireJS 等工具对 AMD 提供了良好的支持，但相比于现代工具链（如 Webpack、Rollup 等）对于 ES6 Module 的优化和支持，AMD 的兼容性和性能优化相对较弱。


### [4. ES6 Module](#)
ES6 Module（ESM）是由 ECMAScript 官方在 ES6（ECMAScript 2015）中引入的模块化规范。
它是 JavaScript 语言级别的模块系统，支持静态分析，能够在编译时确定模块的依赖关系。

> 相较于 CommonJS 和 AMD，ESM 具有更灵活和更高效的模块管理能力。

> 当浏览器社区考虑引入模块系统时，发现 commonjs 并不适合浏览器，这是因为 commonjs 是为同步导入设计的模块系统，在 Node.js 中引入一个模块是通过文件系统，同步非常合理，也非常简单。但在浏览器环境中，都是基于网络加载 js 文件的，需要设计一套异步加载规范。

**ES6 Module 的特性**
* **静态依赖分析**： ES6 Module 在编译时就可以确定模块的依赖关系，从而实现静态分析和树摇（Tree Shaking）优化。这意味着模块中没有被使用的代码可以在打包阶段被移除，从而减小最终的文件大小。
* **严格模式**（Strict Mode）： ES6 Module 自动采用 JavaScript 严格模式。这意味着模块中不能使用某些不安全的语法（如 with 语句），提高了代码的安全性和性能。
* **独立的模块作用域**： 每个模块都有独立的作用域，模块内部的变量、函数不会污染全局作用域，避免了变量命名冲突问题。
* **导入和导出语句**（Import 和 Export）： ES6 Module 使用 import 和 export 关键字来导入和导出模块成员。导出可以是命名导出（Named Export）或默认导出（Default Export）。
* **异步加载支持**： ES6 Module 可以异步加载模块，避免了阻塞浏览器的渲染进程，从而提升了页面加载性能。


浏览器原生支持：
现代浏览器原生支持 ES6 Module，无需额外的加载器（如 RequireJS）或打包工具（如 Webpack）即可直接使用。

ES6 Module 主要通过 **export** 和 **import** 语法来管理模块。

#### [4.1 导出模块（Export）](#)
ES6 Module 提供了两种导出方式：**命名导出** 和 **默认导出**。

**命名导出**（**Named Export**）：允许导出多个成员，导出时需要使用 `{}` 包裹。

```javascript
// module-a.js
export const data = "moduleA data";

export function methodA() {
  console.log("This is methodA");
}

export class MyClass {
  constructor() {
    console.log("This is MyClass");
  }
}
```
**默认导出**（Default Export）：每个模块只能有一个默认导出，使用 export default 关键字。
```javascript
// module-b.js
export default function () {
  console.log("This is the default exported function");
}
```

#### [4.2 导入模块（Import）](#)
**导入命名导出**：需要使用花括号 `{}` 指定导入的成员。
```javascript
// main.js
import { data, methodA, MyClass } from "./module-a.js";

console.log(data); // 输出：moduleA data
methodA(); // 输出：This is methodA
const instance = new MyClass(); // 输出：This is MyClass
```
**导入默认导出**：直接指定导入的变量名称。
```javascript
// main.js
import defaultFunction from "./module-b.js";

defaultFunction(); // 输出：This is the default exported function
```
**同时导入命名导出和默认导出**：
```javascript
// main.js
import defaultFunction, { data, methodA } from "./module-b.js";

defaultFunction();
console.log(data);
methodA();
```

#### [4.3 ES6 Module 与其他模块规范的比较](#)
ES6 Module 相较于 CommonJS 和 AMD 有显著的优势：

加载方式： CommonJS 使用同步加载，这在服务器端是可行的，但在浏览器中会导致阻塞。而 ES6 Module **支持异步加载**，不会阻塞浏览器的渲染进程。

模块依赖分析： CommonJS 模块的依赖关系在运行时解析，这可能导致加载时的性能开销。ES6 Module 在编译阶段就能确定依赖关系，优化了加载效率和性能。

代码优化： 由于 ES6 Module 支持静态分析工具，构建工具能够对代码进行更有效的优化（如 Tree Shaking），减少最终产物的大小。



**参考资料**

[搞懂前端模块化之 CommonJS 篇](https://juejin.cn/post/7108410887052427301)