## [ECMAScript 标准化流程](#)
> **介绍** JavaScript（JS）是一种具有函数优先特性的轻量级、解释型或者说即时编译型的编程语言,是一种
基于原型、多范式、单线程的动态语言，并且支持面向对象、命令式和声明式（如函数式编程）风格。

### [1. 总览](#)
ECMAScript 版本由每年的 ECMA 大会批准并作为标准发布。所有的进展都在 [Ecma TC39 GitHub]()https://github.com/tc39 组织上公
开，该组织托管提案、官方规范文本和会议记录。

ECMAScript 定义了：
* 语法（解析规则、关键词、流程控制、对象初始化，等等）
* 错误处理机制（throw、try...catch，以及创建用户自定义错误类型的能力）
* 类型（布尔值、数字、字符串、函数、对象，等等）
* 基于原型的继承机制
* 内置对象和函数（JSON、Math、Array 方法、parseInt、decodeURI，等等）
* 严格模式
* 模块系统
* 基本内存模型


近年来，JavaScript 的发展主要通过 ECMAScript (ES) 标准的迭代来推动。每年都会有一个新的版本发布，带来新的功能和改进。以下是 JavaScript 近年来的一些重要版本和关键功能.

### [2. ES13 - ECMAScript 2022](#)
ECMAScript 2022，也称为 ES13 或 ES2022，是 JavaScript 标准的一个年度版本。
该版本于2022年6月正式发布。ES2022 引入了一些新的特性和改进，以下是一些主要的变化：

* **类字段的私有性**： 在 ES2022 中，类字段的私有性得到了增强。现在可以在类中声明私有方法和属性，使用 # 符号前
缀来表示它们是私有的。这使得在类内部定义私有状态变得更加直接和安全。
* **顶层 await**：允许在模块的顶层使用 await，无需包裹在 async 函数中。
* **类的静态块**：可以在类中定义静态代码块，用于初始化静态属性。

#### [2.1 Field declarations](#)
在ES2022之前，给class定义一个字段，我们要在constructor里定义：

```javascript
class muse {
  constructor() {
    this.a = 123;
  }
}
```
ES2022允许我们直接这么写：
```javascript
class muse {
  a = 123;
}
```
公共字段和私有字段如果没有被初始化赋值，则会默认设置为 **undefined** 。
```javascript
class X {
  a;
  #b;
  getB() {
      console.log(this.#b);
  }
}

x = new X();
x.a // undefined
x.getB() // undefined
```
对于多个class继承，如果一个字段在多个class上都有定义，那么会以最近的class定义为准
```javascript
class A {
  a = 1;
}

class B extends A {
  a;
}

const b = new B();
b.a // undefined


class C {
  a;
}

class D extends C {
  a = 1;
}

const d = new D();
d.a // 1
```

#### [2.2 Public fields created with Object.defineProperty](#)
公共字段都是通过Object.defineProperty创造的。当某一个字段，get、set也同时存在时，TC39委员会经过
漫长的讨论，最终决定用Object.defineProperty的get、set默认行为，而不是class里定义的get和set。

```javascript
class A {
  set x(value) { console.log(++value); }
  get x() { return 'x' }
}

class B extends A {
  x = 1;
}

const b = new B();
b.x; // 1 (并不会返回'x')
b.x = 2; // 控制台不会打印3
```

#### [2.3 Private fields](#)
在 **ES2022** 之前，并没有实际意义上的私有字段。大家形成一种默契，通常用下划线_开头的字段名来表示私有字段，但是这
些字段还是可以手动更改的。

**ES2022** 给我们提供了更加安全便捷的私有字段定义方法，就是以 `#` 开头命名的字段，都会被当成私有字段，在class外部是
没办法直接读取、修改这些私有字段的。

```javascript
class X {
  #a = 123;
}

const x = new X();
x.#a // Uncaught SyntaxError: Private field '#a' must be declared in an enclosing class
```

#### [2.4 RegExp Match Indices](#)
正则表达式增加了一个`/d`修饰符，当使用正则表达式的exec()方法时，如果有`/d`修饰符，那么结果会多返回一个indices属性，用来表示匹配的结果的在原字符串中的起始index值。

```javascript
const re1 = /a+/d;

const s1 = "aaabbb";
const m1 = re1.exec(s1);
m1.indices[0] //[0, 3];
```
如果正则表达式中有具名捕获组，那么indices[1]则表示捕获组的起始index值，indices.groups同样记录了捕获组信息。

```javascript
const re1 = /a+(?<B>b+)/d;

const s1 = "aaabbbccc";
const m1 = re1.exec(s1);
m1.indices[1] //[3, 6];
m1.indices.groups //{ B: [3, 6] };
```

#### [2.5 Top-level await](#)
之前我们使用await时，必须使用async包裹起来，新的提案允许我们直接使用await; 官方的例子：

```javascript
// awaiting.mjs
import { process } from "./some-module.mjs";

export default (async () => {
  const dynamic = await import(computedModuleSpecifier);
  const data = await fetch(url);
  const output = process(dynamic.default, data);
  return { output };
})();
```

```javascript
// usage.mjs
import promise from "./awaiting.mjs";

export default promise.then(({output}) => {
  function outputPlusValue(value) { return output + value }

  console.log(outputPlusValue(100));
  setTimeout(() => console.log(outputPlusValue(100), 1000));

  return { outputPlusValue };
});
```
不使用顶层await，awaiting.mjs export的是一个异步方法，其他文件引用后，代码逻辑要写到回调里。
```javascript
// awaiting.mjs
import { process } from "./some-module.mjs";
const dynamic = import(computedModuleSpecifier);
const data = fetch(url);
export const output = process((await dynamic).default, await data);
```

```javascript
// usage.mjs
import { output } from "./awaiting.mjs";
export function outputPlusValue(value) { return output + value }

console.log(outputPlusValue(100));
setTimeout(() => console.log(outputPlusValue(100), 1000));
```
使用顶层await，awaiting.mjs export的是最终结果，其他文件引用后就可以直接使用。

### [3. ES12 - ECMAScript 2021](#)
ECMAScript 2021（简称 ES12 或 ES2021）是 JavaScript 的一次年度更新，带来了若干新的特性和改进。以下是 ES12 中的一些关键更新内容：

* 逻辑赋值运算符 `(&&=, ||=, ??=)`：简化逻辑运算中的赋值操作。
* 字符串方法 `replaceAll`：可以替换字符串中所有的匹配项。
* `Promise.any()`： 是一个新的静态方法，用于等待多个 Promise 中的任意一个解析。如果所有 Promise 都被拒绝，则会抛出一个 AggregateError，其中包含了所有的拒绝原因。

### [4. ES11 - ECMAScript 2020](#)
可选链操作符 (?.)：安全地访问嵌套属性。
```javascript
let user = {};
console.log(user?.address?.street); // undefined，不报错
```
空值合并操作符 (??)：提供默认值，当左侧为 null 或 undefined 时返回右侧的值。
```javascript
复制代码
let name = null;
let userName = name ?? 'Anonymous'; // "Anonymous"
```

### [5. ES10 - ECMAScript 2019](#)

* Array.prototype.flat 和 flatMap：用于扁平化嵌套数组。
* Object.fromEntries：将键值对列表转换为对象。

### [6. ES9 - ECMAScript 2018](#)
* 异步迭代 (for await...of)：可以异步遍历可迭代对象。
* Rest/Spread 属性：支持对象的解构与扩展。

```javascript
let { a, ...rest } = { a: 1, b: 2, c: 3 }; // rest = { b: 2, c: 3 }
```

### [7. ES8 - ECMAScript 2017](#)
async/await：异步编程的更优雅方式，基于 Promise。

```javascript
async function fetchData() {
  let response = await fetch(url);
  let data = await response.json();
}
```
对象属性的值简写：在对象定义中可以直接使用变量名作为键。
```javascript
let name = 'Alice';
let obj = { name }; // 等同于 { name: name }
```

### [8. ES7 - ECMAScript 2016](#)
指数运算符 (`**`)：简化了指数运算。

```javascript
let result = 2 ** 3; // 8
```
Array.prototype.includes：检查数组中是否包含某个元素。

### [9. ES6 - ECMAScript 2015](#)
这是 JavaScript 发展的一个重大转折点，引入了许多现代编程功能：

* let 和 const：用于声明变量，let 具有块级作用域，const 用于声明常量。
* 箭头函数：简化了函数的定义。
```javascript
const add = (a, b) => a + b;
```
* 类（Classes）：提供了类似面向对象编程的语法。
* 模块（Modules）：支持模块化开发，可以导入和导出代码。
* Promise：用于异步编程，简化了回调函数的使用。
