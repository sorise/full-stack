## [JavaScript 变量 作用域 内存](#)
> **介绍**：JS 变量和其他语言的变量有很大区别,js变量是松散类型,他只是在特定时间用于保存特定值的一个
> 名字而已，由于不存在定义某个变量必须保存何种数据类型值的规则,变量的值和数据可以在脚本的生命周期内改
> 变,尽管开起来很强大,但是容易出问题。

---

- [1. 原始值与引用值](#1-原始值与引用值)
- [2. 执行上下文与作用域](#2-执行上下文与作用域)
- [3. 控制语句](#3-控制语句)
- [4. for await of异步迭代器](#4-for-await-of异步迭代器)

----

### [1. 原始值与引用值](#)
ECMAScript 变量可以包含两种不同类型的数据：原始值和引用值。原始值（primitive value）就是最简
单的数据，引用值（reference value）则是由多个值构成的对象。

在把一个值赋给变量时，JavaScript 引擎必须确定这个值是原始值还是引用值。

**原始值**：Undefined、Null、Boolean、Number、String 和 Symbol。保存原始值的变量是按值（by value）访
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
JS 对象的属性是动态的,可以动态的为一个对象添加、修改、删除属性或方法 未定义属性返回undefined 但是不会报错

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
**数组中使用delete**

在数组中，与普通的旧对象不同，使用delete在表单中留下垃圾，null在数组中创建一个“洞”, 而且length不变。
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
变量或函数的上下文决定了它们可以访问哪些数据，以及它们的行为。每个上下文都有一个关联的变量对象（variable object），
而这个上下文中定义的所有变量和函数都存在于这个对象上。虽然无法通过代码访问变量对象，但后台处理数据会用到它。

全局上下文是最外层的上下文。根据 ECMAScript 实现的宿主环境，表示全局上下文的对象可能不一样。
* 在Web浏览器中，全局环境被认为是window对象。 因此所有通过 var 定义的全局变量和函数都会成为 window 对象的属性和方法。使
* 对 Node.js 而言，它的名字是 **global**，其它环境可能用的是别的名字。



* 全局环境是最外围的一个执行环境,在web浏览器中,全局环境被认为是window对象,因为此所有全局编和函数都是作为window对象的属性和方法创建。
* 注意: 某个执行环境中的所有代码执行完之后,这个环境就会被销毁,保存在其中的所有变量和函数定义也会被销毁 `[全局变量会在应用程序退出后 关闭网页/浏览器 之后被销毁]`。
* 每个函数都有自己的作用域,当执行流进入一个函数的时,函数的环境就会被推入一个环境栈中,执行完成后 环境栈再弹出环境,吧控制权返回给之前的函数环境 ECS 程序中的执行流正式有这个方便的机制控制着。