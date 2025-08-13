## [JavaScript 变量、作用域、内存](#)
> **介绍**：JS 变量和其他语言的变量有很大区别,js变量是松散类型,他只是在特定时间用于保存特定值的一个
> 名字而已，由于不存在定义某个变量必须保存何种数据类型值的规则,变量的值和数据可以在脚本的生命周期内改
> 变,尽管开起来很强大,但是容易出问题。

---

- [1. 原始值与引用值](#1-原始值与引用值)
- [2. 执行上下文与作用域](#2-执行上下文与作用域)
- [3. 垃圾回收](#3-垃圾回收)
- [4. 内存管理](#4-内存管理)

----

### [1. 原始值与引用值](#)
ECMAScript 变量可以包含两种不同类型的数据：原始值和引用值。原始值（primitive value）就是最简
单的数据，引用值（reference value）则是由多个值构成的对象。

在把一个值赋给变量时，JavaScript 引擎必须确定这个值是原始值还是引用值。

**原始值**：Undefined、Null、Boolean、Number、String、BigInt 和 Symbol。保存原始值的变量是按值（by value）访
问的，因为我们操作的就是存储在变量中的实际值。

引用值是保存在内存中的对象。与C/C++语言不同，JavaScript 不允许直接访问内存位置。在操作对象时，实际上操作的是对该对象的引用（reference）而非
实际的对象本身。为此，保存引用值的变量是按引用（by reference）访问的。
#### [1.2 动态属性](#)
原始值和引用值的定义方式很类似，都是创建一个变量，然后给它赋一个值。不过，在变量保存了这个值之后，可以对这个
值做什么，则大有不同。对于引用值而言，可以随时添加、修改和删除其属性和方法。

```javascript
let person = new Object(); 
person.name = "Nicholas"; 
console.log(person.name); // "Nicholas"
```

原始值不能有属性，尽管尝试给原始值添加属性不会报错。比如：
```javascript
let name = "Nicholas"; 
name.age = 27; 
console.log(name.age); // undefined
```

#### [1.3 变量复制的问题](#)
除了存储方式不同，原始值和引用值在通过变量复制时也有所不同。在通过变量把一个原始值赋值到另一个变量时，原始值会被复制到新变量的位置。

* 基本类型值: **按值** 复制 复制值在一个新的数据段里面
* 引用类型值:复制引用
* Js 函数参数都是按照值传递,传递的值类型会被赋值给一个函数内部的局部变量
```javascript
function setName(obj){
    obj.Name = "Nike";
    obj = new Object(); //申请了一个新的内存
    obj.Name = "maike";
}

var o = new Object();
setName(o);
console.log(o.Name); //Nike
```
#### [1.4 delete](#)
JS 对象的属性是动态的,可以动态的为一个对象添加、修改、删除属性或方法 未定义属性返回 `undefined` 但是不会报错

使用 delete 来删除属性 delete 操作符会从某个对象上移除指定属性。成功删除的时候回返回 true，否则返回 false。但是，以下情况需要重点考虑！

```javascript
let apple = new Object();
apple.color = "red";
apple.place = "XiaoMing"
apple.isExisit = true;

let isDeletePlace = delete apple.place;

console.log(isDeletePlace);//true

console.log(apple.place); //undefined
console.log(apple.owner); //undefined
```
**数组中使用delete**, 在数组中，与普通的旧对象不同，使用delete在表单中留下垃圾，null在数组中创建一个“洞”, 而且length不变。
```javascript
var array = [1, 2, 3, 4];
delete array[2];
/*
Expected result --> [1, 2, 4]
Actual result   --> [1, 2, null, 4]
*/
```
#### [1.5 确定类型](#)
前一章提到的 typeof 操作符最适合用来判断一个变量是否为原始类型。

更确切地说，它是判断一个变量是否为字符串、数值、布尔值或 undefined 的最好方式。如果值是对象或 null，那么 typeof返回"object"，如下面的例子所示：
```javascript
let s = "Nicholas";
let b = true;
let i = 22;
let u;
let n = null;
let o = new Object();
console.log(typeof s); // string
console.log(typeof i); // number
console.log(typeof b); // boolean
console.log(typeof u); // undefined
console.log(typeof n); // object
console.log(typeof o); // object
```

> ECMA-262 规定，任何实现内部 `[[Call]]` 方法的对象都应该在typeof 检测时返回"function"。因为上述浏览器中的正则表达式实现了这个方法，所以 typeof 对正则表达式也返回"function"。

我们通常不关心一个值是不是对象，而是想知道它是什么类型的对象。为了解决这个问题，ECMAScript 提供了 instanceof 操作符，语法如下：

```javascript
console.log(person instanceof Object); // 变量 person 是 Object 吗？
console.log(colors instanceof Array); // 变量 colors 是 Array 吗？
console.log(pattern instanceof RegExp); // 变量 pattern 是 RegExp 吗？
```

### [2. 执行上下文和作用域](#)
**执行上下文（以下简称“上下文”）的概念在 JavaScript 中是颇为重要的。变量或函数的上下文决定了它们可以访问哪些数据，以及它们的行为**。

每个上下文都有一个关联的变量对象（**variable object**），而这个上下文中定义的所有变量和函数都存在于这个对象上。虽然无法通过代码访问变量对象，但后台处理数据会用到它。

全局上下文是最外层的上下文。根据 ECMAScript 实现的宿主环境，表示全局上下文的对象可能不一样。
* 在Web浏览器中，全局环境被认为是window对象。 因此所有通过 var 定义的全局变量和函数都会成为 window 对象的属性和方法。使
* 对 Node.js 而言，它的名字是 **global**，其它环境可能用的是别的名字。

> 每个函数调用都有自己的上下文。当代码执行流进入函数时，函数的上下文被推到一个上下文栈上。
> 在函数执行完之后，上下文栈会弹出该函数上下文，将控制权返还给之前的执行上下文。ECMAScript
> 程序的执行流就是通过这个上下文栈进行控制的。

上下文中的代码在执行的时候，会创建变量对象的一个**作用域链**（**scope chain**）。这个作用域链决定了各级上下文中的代码在访问变量和函数时的顺序。

代码正在执行的上下文的变量对象始终位于作用域链的最前端。如果上下文是函数，则其活动对象（activation object）用作变量对象。
活动对象最初只有一个定义变量：**arguments**。（全局上下文中没有这个变量。）作用域链中的下一个变量对象来自包含上下
文，再下一个对象来自再下一个包含上下文。以此类推直至全局上下文；全局上下文的变量对象始终是作用域链的最后一个变量对象。

**JavaScript 中有三种执行上下文**
- **全局执行上下文** — 这是默认或者说基础的上下文，任何不在函数内部的代码都在全局上下文中。它会执行两件事：创建一个全局的 window对象（浏览器的情况下），并且设置 this 的值等于这个全局对象。一个程序中只会有一个全局执行上下文。
- **函数执行上下文** — 每当一个函数被调用时, 都会为该函数创建一个新的执行上下文。每个函数都有它自己的执行上下文，不过是在函数被调用时创建的。函数上下文可以有任意多个。每当一个新的执行上下文被创建，它会按定义的顺序（将在后文讨论）执行一系列步骤。
- **Eval 函数执行上下文** — 执行在 eval 函数内部的代码也会有它属于自己的执行上下文，但由于 并不经常使用 eval，所以在这里不作讨论。

执行上下文的生命周期包括三个阶段：创建阶段→执行阶段→回收阶段，本文重点介绍创建阶段。


#### [2.1  作用域链增强](#)
虽然执行上下文主要有全局上下文和函数上下文两种（eval()调用内部存在第三种上下文），但有其他方式来增强作用域链。

* try/catch语句
* with 语句
这两种情况下，都会在作用域链前端添加一个变量对象。对 with 语句来说，会向作用域链前端添加指定的对象；
**对catch语句而言，则会创建一个新的变量对象，这个变量对象会包含要抛出的错误对象的声明**。

```javascript
function buildName(){
    var qs = "?Name=jx";
    with(location){
        var url = href + qs;
    }
    return url; // xxx?Name=jx
}

console.log(buildName());
```

### [3. 垃圾回收](#)
JavaScript 是使用垃圾回收的语言，也就是说执行环境负责在代码执行时管理内存。在浏览器的发展史上，用到过两种主要的
标记策略：标记清理和引用计数。

> 垃圾回收过程是一个近似且不完美的方案，因为某块内存是否还有用，属于“不可判定的”问题，意味着靠算法是解决不了的。

#### [3.1 标记清理](#)
标记清理是JavaScript常用的垃圾回收策略。 变量离开上下文时，被标记离开上下文。就会清理掉加上标记的变量。

当变量进入上下文，比如在函数内部声明一个变量时，这个变量会被加上存在于上下文中的标记。而在上下文中的变量，逻辑上讲，永
远不应该释放它们的内存，因为只要上下文中的代码在运行，就有可能用到它们。当变量离开上下文时，也会被加上离开上下文的标记。

#### [3.2 引用计数](#)
每个值都记录它被引用的次数。声明变量并给他赋值一个引用值时，引用次数加1。若同一个值又被赋给另一个变量时，那么
引用数加一。若保存其值的变量被其他覆盖，那么引用数减一，当引用次数为0时，就没办法再访问这个值了，它就可以被回收了。

**问题**：循环引用导致无法为0，无法回收。 在<=IE8的IE中，并非所有的对象都是JavaScript对象。BOM和DOM中的对象时
C++实现的组件对象模型（COM）对象，而这个COM采用的就是引用计数。 解决办法就是把不用的引用值重新赋值为null。

```javascript
let element = document.getElementById("some_element");
let myObject = new Object();
myObject.element = element;
element.someObject = myObject; 

//为避免类似的循环引用问题
myObject.element = null;
element.someObject = null; 
```
把变量设置为 null 实际上会切断变量与其之前引用值之间的关系。当下次垃圾回收程序运行时，这些值就会被删除，内存也会被回收。

#### [3.3 性能](#)
垃圾回收程序会周期性运行，如果内存中分配了很多变量，则可能造成性能损失，因此垃圾回收的时间调度很重要。

现代垃圾回收程序会基于对 JavaScript 运行时环境的探测来决定何时运行。探测机制因引擎而异，
但基本上都是根据已分配对象的大小和数量来判断的。

>  环境的不同分配给JavaScript程序的内存也不同。也是为了避免大量的JavaScript堆内存过度消耗导致系统卡顿。

> 在某些浏览器中是有可能（但不推荐）主动触发垃圾回收的。在 IE 中，window.
> CollectGarbage()方法会立即触发垃圾回收。在 Opera 7 及更高版本中，调用 window.
> opera.collect()也会启动垃圾回收程序。

### [4. 内存管理](#)
JavaScript开发者无需考虑关系内存管理。 环境的不同分配给JavaScript程序的内存也不同。也是为了避免大量的JavaScript堆内存过度消耗导致系统卡顿。

> JavaScript 运行在一个内存管理与垃圾回收都很特殊的环境。分配给浏览器的内存通常比分配给桌面软件的要少很多，分配给移动
> 浏览器的就更少了。这更多出于安全考虑而不是别的，就是为了避免运行大量 JavaScript 的网页耗尽系
> 统内存而导致操作系统崩溃。这个内存限制不仅影响变量分配，也影响调用栈以及能够同时在一个线程中执行的语句数量。

#### [4.1 通过 const 和 let 声明提升性能](#)
ES6 增加这两个关键字不仅有助于改善代码风格，而且同样有助于改进垃圾回收的过程。因为 const和 let 都以块（而非函数）为作用域，所以相比于使用 var，使用这两个新关键字可能会更早地让垃圾回
收程序介入，尽早回收应该回收的内存。

#### [4.2 隐藏类和删除操作](#)
运行期间，V8 会将创建的对象与隐藏类关联起来，以跟踪它们的属性特征。能够共享相同隐藏类
的对象性能会更好，V8 会针对这种情况进行优化，但不一定总能够做到。比如下面的代码：
```javascript
function Article() {
this.title = 'Inauguration Ceremony Features Kazoo Band';
}
let a1 = new Article();
let a2 = new Article();
```
V8 会在后台配置，让这两个类实例共享相同的隐藏类，因为这两个实例共享同一个构造函数和原型。假设之后又添加了下面这行代码：
```javascript
a2.author = 'Jake';
```
此时两个 Article 实例就会对应两个不同的隐藏类。根据这种操作的频率和隐藏类的大小，这有可能对性能产生明显影响。

为此需要**尽量在类中添加属性和方法，不要在实例化后增加属性**。

不用的值设置为null来代替 delete 操作。

#### [4.3 内存泄露](#)
写得不好的 JavaScript 可能出现难以察觉且有害的内存泄漏问题。

* 多是函数调用多次导致，不合理的引用导致的。
* 意外声明的全局变量
* 定时器 还在定时器中使用闭包。
* JavaScript闭包

定时器也可能会悄悄地导致内存泄漏。下面的代码中，定时器的回调通过闭包引用了外部变量：
```javascript
let name = 'Jake';
setInterval(() => {
 console.log(name);
}, 100); 
```
只要定时器一直运行，回调函数中引用的 name 就会一直占用内存。垃圾回收程序当然知道这一点，因而就不会清理外部变量。

**使用 JavaScript 闭包很容易在不知不觉间造成内存泄漏**。
```javascript
let outer = function() {
 let name = 'Jake';
 return function() {
     return name;
 };
}; 
```

#### [4.4 静态分配与对象池](#) 
间接控制垃圾回收触发的条件。避免多余的垃圾回收，就可以保住因为释放内存而导致的损失的性能。

* 静态分配 不在函数中new 一个对象。函数外部new一个对象然后通过参数传入。
* **对象池**：极端优化。 在初始化的某一时刻，创建一个对象池，来管理一组可回收的对象，应用程序向这个对象池请求一个对象，设置其属性使用它操作完后再把它还给对象池，由于没有发生对象初始化，垃圾回收探测器就不会发现由对象更替，就不用频繁的运行。
* 创建数组时大小固定。 