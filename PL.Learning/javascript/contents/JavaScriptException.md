## [JavaScript 异常、错误处理 ](#)
> **介绍** : 。
---

### [1. 错误与异常](#1-)
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