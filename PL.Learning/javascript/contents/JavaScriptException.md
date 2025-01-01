## [JavaScript 异常、错误处理 ](#)
> **介绍** : 所有主流桌面浏览器，包括 IE/Edge、Firefox、Safari、Chrome 和 Opera，都提供了向用户报告错误
的机制。
---

- [1. 错误与异常](#1-错误与异常)
- [2. try catch finally](#2-try-catch-finally)
- [3. 错误类型](#3-错误类型)
- [4. 错误事件](#4-错误事件)

---

### [1. 错误与异常](#)
ECMAScript 第 3 版致力于改进这个方面，引入了 `try/catch` 和 `throw` 语句，以及一些错误类型，以帮助开发者在出错时正确地处理它们。

#### [1.1 异常](#)
**定义**：从根本上来说，异常就是一个普通的对象，其保存了异常发生的相关信息，比如错误码、错误信息等。
JS 中的标准内置对象 Error 为例，其标准属性有 message。许多宿主环境额外增加了 filename 和 stack (node.js) 等属性。

#### [1.2 错误](#)
**错误只有被 throw，才会产生异常**，不被抛出的错误不会产生异常。比如直接new Error()甚至打印 Error 但是不 throw，也是不会产生异常

#### [1.3 编译时异常](#)
源代码在编译成可执行代码之前产生的异常，无需执行即有异常。编译、语法解析发生错误。编译型语言对于这种很常见的，但是解析型的 js 也是会有编译型异常。

```javascript
console.log(1)

let 1 // Uncaught SyntaxError: Unexpected number

function test() {
    console.log(1)
    await 1
}
```

#### [1.4 运行时异常](#)
代码被执行之后产生的异常。这些通常是很难提前发现的，因为代码实际运行中会遇到。
比较常见的如TypeError: Cannot read properties of undefined这样的读取了undefined的属性。

运行时异常对比编译时异常的特点是代码执行到异常代码前都是会正常执行的

#### [1.5 异常传播](#)
异常抛出就像事件冒泡一样具有传递性。如果一个异常没有被 catch，它会沿着函数调用栈一层层传播直到栈空。

异常会不断传播直到遇到第一个 catch。 如果都没有捕获，会抛出类似 unCaughtError，表示发生了一个异常，未被捕获的异常通常会被打印在控制台上

### [2. try catch finally](#)
ECMA-262 第 3 版新增了 try/catch 语句，作为在 JavaScript 中处理异常的一种方式。基本的语法
如下所示，跟 Java 中的 try/catch 语句一样：
```javascript
try {
    //代码 1
}catch (err) {
    //代码 2：当代码1出现异常后，会执行这里的代码，异常对象会传递给err
}finally {
    //代码 3: 可以省略。无论是否有异常，都会执行。
}

// 无异常执行顺序:代码1-->代码3
// 有异常执行顺序:代码1-->出现异常，中断代码1的执行 -->代码2 -->代码3
```
try/catch 语句中可选的 finally 子句始终运行。如果 try 块中的代码运行完，则接着执行finally 块中的代码。

如果出错并执行 catch 块中的代码，则 finally 块中的代码仍执行。try 或catch 块无法阻止 finally 块执行，包括 return 语句。

```javascript
function testFinally(){
    try {
        return 2;
    } catch (error){
        return 1;
    } finally {
        return 0;
    }
}

let v =testFinally();

console.log(`value: ${v}`); //最终返回 value: 0
```

#### [2.1 throw](#)
try/catch 语句对应的一个机制是 throw 操作符，用于在任何时候抛出自定义错误。throw 操
作符必须有一个值，但值的类型不限。下面这些代码都是有效的：

```javascript
throw 12345; 
throw "Hello world!"; 
throw true; 
throw { name: "JavaScript" };
```
可以通过内置的错误类型来模拟浏览器错误。每种错误类型的构造函数都只接收一个参数，就是错误消息。
```javascript
throw new Error("Something bad happened.");
throw new SyntaxError("I don't like your syntax.");
throw new InternalError("I can't do that, Dave.");
throw new TypeError("What type of variable do you take me for?");
throw new RangeError("Sorry, you just don't have the range.");
throw new EvalError("That doesn't evaluate.");
throw new URIError("Uri, is that you?");
throw new ReferenceError("You didn't cite your references properly.");
```

### [3. 错误类型](#)
代码执行过程中会发生各种类型的错误。每种类型都会对应一个错误发生时抛出的错误对象。 ECMA-262 定义了以下 8 种错误类型：

* **Error** -- 错误对象
* **SyntaxError** --解析过程语法错误（上面提到的编译时异常）
* **TypeError** -- 不属于有效类型（上面举例的运行时异常）
* **ReferenceError** -- 无效引用（严格模式下直接访问一个未定义的变量）
* **RangeError** -- 数值超出有效范围
* **URIError** -- 解析 URI 编码出错
* **EvalError** -- 调用 eval 函数错误
* **InternalError** -- Javascript 引擎内部错误的异常抛出， "递归太多"

#### [3.1 Error](#)
[Error](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Error/Error) 是基类型，其他错误类型继承该类型。因此，所有错误类型都共享相同的属性（所有错误对象上的方法都是这个默认类型定义的方法）。浏览器很少会抛出 Error 类型的错误，该类型主要用于开
发者抛出自定义错误。

Error本身作为函数直接调用和被 new 调用的效果是一样的

```javascript
const a = Error('a')
const b = new Error('b')

new Error()
new Error(message)
new Error(message, options)
new Error(message, fileName)
new Error(message, fileName, lineNumber)

Error()
Error(message)
Error(message, options)
Error(message, fileName)
Error(message, fileName, lineNumber)
```
**参数**：
* options 一个包含以下属性的对象：
  * cause 可选 指示错误的具体原因，反映在 cause 属性中。当捕获并重新抛出带有更具体或有用的错误消息的错误时，可以使用此属性传递原始错误。
* fileName 可选 
  * 非标准引发此错误的文件路径，反映在 fileName 属性中。默认为调用 Error() 构造函数的代码所在文件的名称。

 
默认的 error 对象只有一个 message 信息，很多时候对于错误的细分是很不好使，一般可以通过扩展这个错误对象，抛异
常时抛出自定义的错误对象，在异常处理或时实现更精细化的处理。
```javascript
class ApiError extends Error {

  constructor(message, code) {
    super(message);
    this.code = code
  }
}

const err = new ApiError('xxx', 404)

err instanceof  ApiError
```
**使用 cause 重新抛出错误**： 在捕获错误时，我们可能会使用新的错误信息对错误进行包装，再将其重新抛出。这种场景下，你应当将原始错误也传入新的 Error 的构造函数，如下所示：
```javascript
try {
  frameworkThatCanThrow();
} catch (err) {
  throw new Error("New error message", { cause: err });
}
```
**实例属性**
* Error: cause 指示导致该错误的具体原始原因。
* Error: message 错误的人类可读描述。
* Error.prototype.stack 默认情况下，V8 引发的几乎所有错误都具有一个 stack 属性，该属性保存最顶层的 10 个堆栈帧，格式为字符串 at xxx

#### [3.2 InternalError](#)
InternalError 类型的错误会在底层 JavaScript 引擎抛出异常时由浏览器抛出。例如，递归过多导
致了栈溢出。这个类型并不是代码中通常要处理的错误，如果真发生了这种错误，很可能代码哪里弄错
了或者有危险了。

### [4. 错误事件](#)
浏览器中有一些特殊的错误事件，需要关注。
#### [4.1 window.error 事件](#)
任何没有被 try/catch 语句处理的错误都会在 window 对象上触发 error 事件。该事件是浏览器早期支持的
事件，为保持向后兼容，很多浏览器保持了其格式不变。在 onerror 事件处理程序中，任何浏览器都不会传入 event 对象。

```javascript
window.onerror = (message, url, line) => { 
 console.log(message); 
};
```
可以返回 false 来阻止浏览器默认报告错误的行为，如下所示：
```javascript
window.onerror = (message, url, line) => { 
 console.log(message); 
 return false; 
};
```
#### [4.2 图片 error](#)
图片也支持 error 事件。任何时候，如果图片 src 属性中的 URL 没有返回可识别的图片格式，就
会触发error事件。这个事件遵循 DOM 格式，返回一个以图片为目标的 event 对象。下面是个例子：

```javascript
const image = new Image(); 

image.addEventListener("load", (event) => { 
 console.log("Image loaded!"); 
}); 

image.addEventListener("error", (event) => { 
 console.log("Image not loaded!"); 
}); 

image.src = "doesnotexist.gif"; // 不存在，资源会加载失败
```