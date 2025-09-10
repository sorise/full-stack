## [JavaScript 函数](#)
> **介绍**：JavaScript函数是ECMAScript中最有意思的部分之一，这主要是因为函数实际上是对象。

-----
- [1. 基本概念](#1-基本概念)
- [2. 箭头函数](#2-箭头函数)
- [3. 函数参数](#3-函数参数)
- [4. 函数作为值](#4-函数作为值)
- [5. 函数内部](#5-函数内部)
- [6. 函数属性与方法](#6-函数属性与方法)
- [7. 递归与尾递归](#7-递归与尾递归)
- [8. 闭包](#8-闭包)
- [9. 立即调用的函数表达式](#9-立即调用的函数表达式)

-----
### [1. 基本概念](#)
**函数是这样的一段JavaScript代码，只定义一次，但可能被执行或调用任意次**。
每个函数都是Function 类型的实例，而 Function 也有属性和方法，跟其他引用类型一样。
因为函数是对象，所以函数名就是指向函数对象的指针，而且不一定与函数本身紧密绑定。

#### [1.1 函数的声明定义](#)
在 JavaScript 中定义一个函数使用 function 关键字，后面跟着函数名和一对圆括号 ()，括号中可以包含零个或多个参数，这些参数是在调用函数时传递给它的值。接着是一对花括号 {}，其中包含了函数体——即当函数被调用时要执行的语句。
**函数返回值**, 函数中的return语句用来返回函数调用后的返回值，return语句只能出现在函数体内。

JavaScript有四种声明函数的方法

第一种方式，**函数的声明语句** 函数声明的关键特点是**函数声明提升**，即函数声明会在代码执行之前获得定义。这意味着函数声明
可以出现在调用它的代码之后：

```javascript
function sum (num1, num2) {
    return num1 + num2;
}

sayHi();
function sayHi() {
    console.log("Hi!");
} 
```
另一种定义函数的语法是函数表达式,**函数表达式**与函数声明几乎是等价的：
```javascript
let sum = function(num1, num2) {
    return num1 + num2;
}; 
```
还有一种定义函数的方式与函数表达式很像，叫作“**箭头函数**”（arrow function），如下所示：
```javascript
let sum = (num1, num2) => {
    return num1 + num2;
}; 
```
最后一种定义函数的方式是使用 **Function 构造函数**。这个构造函数接收任意多个字符串参数，最后一个参数始终会被当成函数体，而之前的参数都是新函数的参数。来看下面的例子：
```javascript
let sum = new Function("num1", "num2", "return num1 + num2"); // 不推荐
```
**不推荐使用这种语法来定义函数，因为这段代码会被解释两次：第一次是将它当作常规
ECMAScript 代码，第二次是解释传给构造函数的字符串。这显然会影响性能**。

#### [1.2 函数的返回值](#)
函数可以通过 return 语句返回一个值。如果函数体中没有 return 语句，或者 return 后面没有值，则该函数返回 undefined。

```javascript
function add(a, b) {
return a + b;
}

let result = add(3, 4); // 结果为 7
```

#### [1.3 函数名](#)
因为**函数名就是指向函数的指针**，所以它们跟其他包含对象指针的变量具有相同的行为。这意味着一个函数可以有多个名称，如下所示：

```javascript
function sum(num1, num2) {
 return num1 + num2;
}
console.log(sum(10, 10)); // 20
let anotherSum = sum;

console.log(anotherSum(10, 10)); // 20
sum = null;

console.log(anotherSum(10, 10)); // 20 
```
ECMAScript 6 的所有函数对象都会暴露一个**只读的 name 属性**，其中包含关于函数的信息。多数情
况下，这个属性中保存的就是一个函数标识符，或者说是一个字符串化的变量名。**如果它是使用 Function 构造函数创建的，则会标识成"anonymous"**。
```javascript
function foo() {}
let bar = function() {};
let baz = () => {};
console.log(foo.name); // foo
console.log(bar.name); // bar
console.log(baz.name); // baz
console.log((() => {}).name); //（空字符串）
console.log((new Function()).name); // anonymous 

class pair{
    constructor(_key, _value){
        this.key = _key;
        this.value = _value;
    }

    [Symbol.toStringTag](){
        return `{${this.key}:${this.value}}`;
    }

    getKey = () => {
        console.log(this.key); // 10
    }
}

let kp = new pair(10,20);
console.log(kp.getKey.name);//getKey

function sum(num1, num2) {
    return num1 + num2;
}

console.log(sum.name); //name
let linkSum = sum;
console.log(linkSum.name); //name
```
如果函数是一个获取函数、设置函数，或者使用 bind()实例化，那么标识符前面会加上一个前缀：
```javascript
function foo() {}
console.log(foo.bind(null).name); // bound foo
let dog = {
    years: 1,
    get age() {
        return this.years;
    },
    set age(newAge) {
        this.years = newAge;
    }
}

let propertyDescriptor = Object.getOwnPropertyDescriptor(dog, 'age');
console.log(propertyDescriptor.get.name); // get age
console.log(propertyDescriptor.set.name); // set age 
```
#### [1.4 没有重载](#)
ECMAScript 函数不能像传统编程那样重载。

#### [1.5 函数声明提升](#)
JavaScript 引擎在加载数据时对它们是区别对待的。JavaScript 引擎在任何代码执行之前，会先读取函数声明，并在执行上下文中 生成函数定义。

以上代码可以正常运行，因为函数声明会在任何代码执行之前先被读取并添加到执行上下文。这个
过程叫作**函数声明提升**（function declaration hoisting）。
```javascript
// 没问题
console.log(sum(10, 10));
function sum(num1, num2) {
    return num1 + num2;
} 
```
在执行代码时，JavaScript 引擎会先执行一遍扫描，
把发现的函数声明提升到源代码树的顶部。因此即使函数定义出现在调用它们的代码之后，引擎也会把
函数声明提升到顶部。
```javascript
// 会出错
console.log(sum(10, 10));
let sum = function(num1, num2) {
    return num1 + num2;
}; 
```

### [2. 箭头函数](#)
ECMAScript 6 新增了使用胖箭头（`=>`）语法定义函数表达式的能力。很大程度上，箭头函数实例化的函数对象与正式的函数表达式创建的函数对象行为是相同的。
任何可以使用函数表达式的地方，都可以使用箭头函数：

```javascript
let arrowSum = (a, b) => {
 return a + b;
};

let functionExpressionSum = function(a, b) {
 return a + b;
};

console.log(arrowSum(5, 8)); // 13
console.log(functionExpressionSum(5, 8)); // 13 
```
箭头函数简洁的语法非常适合嵌入函数的场景：
```javascript
let ints = [1, 2, 3];
console.log(ints.map(function(i) { return i + 1; })); // [2, 3, 4]
console.log(ints.map((i) => { return i + 1 })); // [2, 3, 4] 
```
如果只有一个参数，那也可以不用括号。只有没有参数，或者多个参数的情况下，才需要使用括号：
```javascript
// 以下两种写法都有效
let double = (x) => { return 2 * x; };
let triple = x => { return 3 * x; };
// 没有参数需要括号
let getRandom = () => { return Math.random(); };
// 多个参数需要括号
let sum = (a, b) => { return a + b; };
// 无效的写法：
let multiply = a, b => { return a * b; }; 
```
**箭头函数也可以不用大括号**，但这样会改变函数的行为。使用大括号就说明包含“函数体”，可以
在一个函数中包含多条语句，跟常规的函数一样。如果不使用大括号，那么箭头后面就只能有一行代码，
比如一个赋值操作，或者一个表达式。而且，**省略大括号会隐式返回这行代码的值**：
```javascript
// 以下两种写法都有效，而且返回相应的值
let double = (x) => { return 2 * x; };
let triple = (x) => 3 * x;
// 可以赋值
let value = {};
let setName = (x) => x.name = "Matt";
setName(value);
console.log(value.name); // "Matt"
// 无效的写法：
let multiply = (a, b) => return a * b; 
```
箭头函数虽然语法简洁，但也有很多场合不适用。箭头函数不能使用 **arguments**、**super** 和
**new.target**，也不能用作构造函数。此外，箭头函数也没有 **prototype** 属性。

#### [2.1 this](#)
在 JavaScript 中，箭头函数相比于传统的函数声明（使用 function 关键字）有一个重要的不同点，那就是箭头
函数不会创建自己的 this 上下文。这意味着，**在箭头函数内部使用的 this 值实际上是从定义它的上下文中继承来的**。


**传统函数中的 this** ，在非箭头函数中，this 的值取决于函数是如何被调用的。
```javascript
var obj = {
    name: "example",
    getName: function() {
        console.log(this.name); // 输出 "example"
    }
};

obj.getName();
```
* 在一个对象的方法中调用函数时，this 通常指的是该对象；
* 而在全局作用域或者单独调用时，this 指的是全局对象（在浏览器中是 window，在 Node.js 中是 global），**在严格模式下则为 undefined**。

**箭头函数中的 this** 箭头函数并没有自己的 this 绑定，它会从外围的词法环境捕获 this 值，也就是所谓的“词法 this”。
```javascript
var obj = {
    name: "example",
    getName: () => {
        console.log(this.name); // 输出 "undefined" 在浏览器中或 "global" 在 Node.js 中
    }
};

obj.getName(); // 因为箭头函数没有自己的 this，这里的 this 是全局对象


class pair{
    constructor(_key, _value){
        this.key = _key;
        this.value = _value;
    }

    [Symbol.toStringTag](){
        return `{${this.key}:${this.value}}`;
    }

    getkey = () => {
        console.log(this.key); // 10
    }
}

let kp = new pair(10,20);
kp.getkey();//10
```

### [3. 函数参数](#)
ECMAScript 函数的参数跟大多数其他语言不同。ECMAScript 函数既不关心传入的参数个数，也不
关心这些参数的数据类型。定义函数时要接收两个参数，并不意味着调用时就传两个参数。你可以传一
个、三个，甚至一个也不传，解释器都不会报错。

**为ECMAScript 函数的参数在内部表现为一个数组**。 在使用 function 关键字定义（非箭头）函数时，可以在函数内
部访问 **arguments** 对象，从中取得传进来的每个参数值。

**arguments 对象是一个类数组对象（但不是 Array 的实例）**，因此可以使用中括号语法访问其中的
元素（第一个参数是 arguments[0]，第二个参数是 arguments[1]）。而要确定传进来多少个参数，
可以访问 **arguments.length 属性**。


在下面的例子中，sayHi()函数的第一个参数叫 name：
```javascript
function sayHi(name, message) {
 console.log("Hello " + name + ", " + message);
}
```
可以通过 `arguments[0]` 取得相同的参数值。因此，把函数重写成不声明参数也可以：
```javascript
function sayHi() {
 console.log("Hello " + arguments[0] + ", " + arguments[1]);
}

function doAdd(num1, num2) {
    if (arguments.length === 1) {
        console.log(num1 + 10);
    } else if (arguments.length === 2) {
        console.log(arguments[0] + num2);
    }
}
```

#### [3.1 箭头函数中的参数](#)
如果函数是使用箭头语法定义的，那么传给函数的参数将不能使用 arguments 关键字访问，而只能通过定义的命名参数访问。
```javascript
function foo() {
 console.log(arguments[0]);
}
foo(5); // 5
let bar = () => {
 console.log(arguments[0]);
};
bar(5); // ReferenceError: arguments is not defined
```
虽然箭头函数中没有 arguments 对象，但可以在包装函数中把它提供给箭头函数：
```javascript
function foo() {
    let bar = () => {
        console.log(arguments[0]); // 5
    };
    bar();
}
foo(5);
```
> ECMAScript 中的所有参数都按值传递的。不可能按引用传递参数。如果把对象作
为参数传递，那么传递的值就是这个对象的引用。

#### [3.2 默认参数值](#)
在 ECMAScript5.1 及以前，实现默认参数的一种常用方式就是检测某个参数是否等于 undefined，
如果是则意味着没有传这个参数，那就给它赋一个值：
```javascript
function makeKing(name) {
 name = (typeof name !== 'undefined') ? name : 'Henry';
 return `King ${name} VIII`;
}

console.log(makeKing()); // 'King Henry VIII'
console.log(makeKing('Louis')); // 'King Louis VIII' 
```
ECMAScript 6 之后就不用这么麻烦了，因为它支持显式定义默认参数了。下面就是与前面代码等价
的 ES6 写法，只要在函数定义中的参数后面用=就可以为参数赋一个默认值：
```javascript
function makeKing(name = 'Henry') {
    return `King ${name} VIII`;
}
console.log(makeKing('Louis')); // 'King Louis VIII'
console.log(makeKing()); // 'King Henry VIII' 
```
给参数传 undefined 相当于没有传值，不过这样可以利用多个独立的默认值：
```javascript
function makeKing(name = 'Henry', numerals = 'VIII') {
    return `King ${name} ${numerals}`;
}

console.log(makeKing()); // 'King Henry VIII'
console.log(makeKing('Louis')); // 'King Louis VIII'
console.log(makeKing(undefined, 'VI')); // 'King Henry VI' 
```
箭头函数同样也可以这样使用默认参数，只不过在只有一个参数时，就必须使用括号而不能省略了：
```javascript
let makeKing = (name = 'Henry') => `King ${name}`;
console.log(makeKing()); // King Henry 
```

#### [3.3 扩展参数、可变参数](#)
ES6 引入 rest 参数（形式为...变量名），用于获取函数的多余参数，这样就不需要使用arguments对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

```javascript
function add(...values) {
  let sum = 0;

  for (var val of values) {
    sum += val;
  }

  return sum;
}

add(2, 5, 3) // 10

function getProduct(a, b, c = 1) {
    return a * b * c;
}
let getSum = (a, b, c = 0) => {
    return a + b + c;
}

console.log(getProduct(...[1,2])); // 2
console.log(getProduct(...[1,2,3])); // 6
console.log(getProduct(...[1,2,3,4])); // 6
console.log(getSum(...[0,1])); // 1
console.log(getSum(...[0,1,2])); // 3
console.log(getSum(...[0,1,2,3])); // 3 
```
箭头函数虽然不支持 arguments 对象，但支持收集参数的定义方式，因此也可以实现与使用arguments 一样的逻辑：
```javascript
let getSum = (...values) => {
 return values.reduce((x, y) => x + y, 0);
}
console.log(getSum(1,2,3)); // 6 
```

### [4. 函数作为值](#)
因为函数名在 ECMAScript 中就是变量，所以函数可以用在任何可以使用变量的地方。这意味着不仅
可以把函数作为参数传给另一个函数，而且还可以在一个函数中返回另一个函数。

```javascript
function callSomeFunction(someFunction, someArgument) {
    return someFunction(someArgument);
}

//返回一个函数
function createComparisonFunction(propertyName) {
    return function(object1, object2) {
        let value1 = object1[propertyName];
        let value2 = object2[propertyName];
        if (value1 < value2) {
            return -1;
        } else if (value1 > value2) {
            return 1;
        } else {
            return 0;
        }
    };
}

let data = [
    {name: "Zachary", age: 28},
    {name: "Nicholas", age: 29}
];
data.sort(createComparisonFunction("name"));
console.log(data[0].name); // Nicholas
data.sort(createComparisonFunction("age"));
console.log(data[0].name); // Zachary 
```

### [5. 函数内部](#)
在 ECMAScript 5 中，函数内部存在几个个特殊的对象：arguments 和 this。ECMAScript 6 又新增
了 new.target 属性。

#### [5.1 arguments](#)
它是一个类数组对象，包含调用函数时传入的所有参数, 具有属性 length、callee。
callee 属性，是一个指向 arguments 对象所在函数的指针。

```javascript
function factorial(n) {
    if (n === 0) {
        return 1;
    } else {
        return n * arguments.callee(n - 1);
    }
}

console.log(factorial(5)); // 输出 120
```
在JavaScript中，arguments.callee和caller属性曾经在ECMAScript3规范中被定义，但在ECMAScript 5（ES5）
中被标记为遗弃，**并且在ECMAScript 6（ES6）及之后的版本中被彻底移除**。这两个属性在现代JavaScript开发中
不应该被使用，因为它们存在一定的安全风险，并且在严格模式（strict mode）下是不可用的。
```javascript
"use strict";

function factorial(n) {
    if (n === 0) {
        return 1;
    } else {
        return n * arguments.callee(n - 1); //报错
    }
}

console.log(factorial(5)); // 输出 120
```

#### [5.2 this](#)
在标准函数中，this 引用的是把函数当成方法调用的上下文对象，这时候通常称其为 this 值（在网
页的全局上下文中调用函数时，this 指向 windows）。

```javascript
window.color = 'red';
let o = {
 color: 'blue'
};
function sayColor() {
    console.log(this.color);
}
sayColor(); // 'red'
o.sayColor = sayColor;

o.sayColor(); // 'blue'
```
在箭头函数中，**this引用的是定义箭头函数的上下文**。
```javascript
window.color = 'red';
let o = {
 color: 'blue'
};
let sayColor = () => console.log(this.color);
sayColor(); // 'red'
o.sayColor = sayColor;
o.sayColor(); // 'red' 
```
在事件回调或定时回调中调用某个函数时，this 值指向的并非想要的对象。此时将
回调函数写成箭头函数就可以解决问题。这是因为箭头函数中的 this 会保留定义该函数时的上下文：
```javascript
function King() {
 this.royaltyName = 'Henry';
 // this 引用 King 的实例
 setTimeout(() => console.log(this.royaltyName), 1000);
} 
```

#### [5.3 caller](#)
ECMAScript 5 也会给函数对象上添加一个属性：caller。虽然 ECMAScript 3 中并没有定义，但所
有浏览器除了早期版本的 Opera 都支持这个属性。这个属性引用的是调用当前函数的函数，或者如果是
在全局作用域中调用的则为 null。比如：
```javascript
function outer() {
    inner();
}
function inner() {
    console.log(inner.caller);
}

outer();
```
以上代码会显示 outer()函数的源代码。这是因为 ourter()调用了 inner()，inner.caller
指向 outer()。如果要降低耦合度，则可以通过 arguments.callee.caller 来引用同样的值：
```javascript
function outer() {
    inner();
}
function inner() {
    console.log(arguments.callee.caller);
}
outer();
//[Function: outer]

function factorial(num) {
    if(num <= 1){
        return 1
    }else {
        return num * arguments.callee(num - 1)
    }
}
```
在严格模式下访问 arguments.callee 会报错。ECMAScript 5 也定义了 arguments.caller，但
在严格模式下访问它会报错，在非严格模式下则始终是 undefined。

这是为了分清 arguments.caller和函数的 caller 而故意为之的。而作为对这门语言的安全防护，这些改动也让第三方代码无法检测同
一上下文中运行的其他代码。 严格模式下还有一个限制，就是不能给函数的 caller 属性赋值，否则会导致错误。

**在es5.0 的标准模式下不能用callee 和 caller**

#### [5.4 new.target](#)
ECMAScript 中的函数始终可以作为构造函数实例化一个新对象，也可以作为普通函数被调用。
ECMAScript 6 新增了检测函数是否使用 new 关键字调用的 new.target 属性。如

```javascript
function King() {
 if (!new.target) {
    throw 'King must be instantiated using "new"'
 }
 console.log('King instantiated using "new"');
}
new King(); // King instantiated using "new"
King(); // Error: King must be instantiated using "new" 
```

### [6. 函数属性与方法](#)
前面提到过，ECMAScript 中的函数是对象，因此有属性和方法。每个函数都有两个属性：length
和 prototype。

* length 属性保存函数定义的命名参数的个数。
* prototype **指向原型**，是保存引用类型所有实例方法的地方，这意味着toString()、valueOf()等方法实际上都保存在 prototype 上，进而由所有实
例共享。**prototype 属性是不可枚举的，因此使用 for-in 循环不会返回这个属性**。

bind、call和apply都是JavaScript中用于处理函数调用的方法。它们的作用都是相同的而主要区别在于它们如何设置和传递函数的上下文以及参数。

#### [6.1 bind](#)
bind会创建一个新的函数，并将原始函数绑定到指定的上下文,以后可以进行复用。这意味着无论在什么时候调用这个新函数，它都会使用绑定的上下文。

```javascript
const info = { name: '张三' };

function Info(age) {
  console.log(`姓名: ${this.name}`);
  console.log(`年龄: ${age}`);
}

const bindInfo = Info.bind(info,'19');
bindInfo();  // 输出结果为:姓名: 张三
             //          年龄: 19 
```

#### [6.2 call()](#)
call可以在指定的上下文中调用函数，并传递一个或多个参数。与bind不同的是，call会立即调用函数，而不是返回一个新函数，所以并不能像bind一样进行复用。

call方法的第一个参数是要绑定的上下文对象，后面的参数是要传递给函数的参数。
```javascript
const info = { name: '张三' };

function Info(age,sex) {
  console.log(`姓名: ${this.name}`);
  console.log(`年龄: ${age}`);
  console.log(`性别: ${sex}`);
}

Info.call(info,'19','男'); // 输出结果为:姓名: 张三
                           //            年龄: 19
                           //            性别: 男

```
#### [6.3 apply()](#)
apply在指定的上下文中调用函数，并传递一个数组作为参数。与call类似，apply也是立即调用函数。第一个参数是要绑定的上下文对象，但与call不同的是，apply的第二个参数是一个数组这个数组里面包含要传递给函数的参数。
```javascript
func.apply(thisArg, [argsArray]);
```

```javascript
function sum(num1, num2) {
    return num1 + num2;
}

function callSum1(num1, num2) {
    return sum.apply(this, arguments); // 传入 arguments 对象
}


const info = { name: '张三' };

function Info(age,sex) {
  console.log(`姓名: ${this.name}`);
  console.log(`年龄: ${age}`);
  console.log(`性别: ${sex}`);
}

Info.apply(info,['19','男']);// 输出结果为:姓名: 张三
//            年龄: 19
//            性别: 男
```

### [7. 递归与尾递归](#)
递归函数通常的形式是一个函数通过名称调用自己，如下面的例子所示：
```javascript
function factorial(num) {
    if (num <= 1) {
        return 1;
    } else {
        return num * arguments.callee(num - 1);
    }
}
```

#### [7.1 尾调用优化](#)
ECMAScript 6 规范新增了一项内存管理优化机制，让 JavaScript 引擎在满足条件时可以重用栈帧。
具体来说，这项优化非常适合“尾调用”，**即外部函数的返回值是一个内部函数的返回值**。

```javascript
function outerFunction() {
    return innerFunction(); // 尾调用
} 
```
**尾调用优化的条件**
* 代码在严格模式下执行；
* 外部函数的返回值是对尾调用函数的调用；
* 尾调用函数返回后不需要执行额外的逻辑；
* 尾调用函数不是引用外部函数作用域中自由变量的闭包。

可以通过把简单的递归函数转换为待优化的代码来加深对尾调用优化的理解。下面是一个通过递归计算斐波纳契数列的函数：
```javascript
"use strict";
// 基础框架
function fib(n) {
    return fibImpl(0, 1, n);
}
// 执行递归
function fibImpl(a, b, n) {
    if (n === 0) {
        return a;
    }
    return fibImpl(b, a + b, n - 1);
} 
```

### [8. 闭包](#)
匿名函数经常被人误认为是闭包（closure）。闭包指的是那些引用了另一个函数作用域中变量的函数，通常是在嵌套函数中实现的。

> 闭包（closure）是一个函数以及其捆绑的周边环境状态（lexical environment，词法环境）的引用的组合。
> 换而言之，闭包让开发者可以从内部函数访问外部函数的作用域。在 JavaScript 中，闭包会随着函数的创建而被同时创建。

**在一个作用域中可以访问另一个函数内部的局部变量的函数。**

```javascript
function makeFunc() {
    var name = "Mozilla";
    function displayName() {
        console.log(name);
    }
    return displayName;
}

var myFunc = makeFunc();
myFunc();//Mozilla

function createComparisonFunction(propertyName) {
    return function(object1, object2) {
        let value1 = object1[propertyName];
        let value2 = object2[propertyName];
        if (value1 < value2) {
            return -1;
        } else if (value1 > value2) {
            return 1;
        } else {
            return 0;
        }
    };
} 
```

#### [8.1 闭包的特性](#)
闭包的实现，实际上是利用了JavaScript中作用域链的概念，简单理解就是：在JavaScript中，如果在某个作用域下访问某个变量的时候，如果不存在，就一直向外层寻找，直到在全局作用域下找到对应的变量为止，这里就形成了所谓的作用域链。
* 闭包可以访问到父级函数的变量
* 访问到父级函数的变量不会销毁

```javascript
var age = 18;

function person(){
    age++;
    console.log(age);
}
    
person(); // 19
person(); // 20
person(); // 21
```
可以看到这里调用了3次函数，age的值也从18增长到了21，但是这么写会导致全局变量被污染，所以将age的定义移
动到person函数内部，代码如下：
```javascript
function person() {
  var age = 18;
  age++;
  console.log(age);
}

person(); // 19
person(); // 19
person(); // 19
```
但是这又导致了另一个问题，变为局部变量的age不会自增了，所以那么就可以利用闭包的这个特性将每次调用时的age保存起来这样就可以实现变量的自增了，代码如下：
```javascript
function person() {
  var age = 18;
  return function(){
    age++;
    console.log(age);
  }
}

let getPersonAge = person();
getPersonAge(); // 19
getPersonAge(); // 20
getPersonAge(); // 21
```

#### [8.2 this 对象](#)
在闭包中使用 this 会让代码变复杂。如果内部函数没有使用箭头函数定义，则 this 对象会在运
行时绑定到执行函数的上下文。

```javascript
window.identity = 'The Window';
let object = {
    identity: 'My Object',
    getIdentityFunc() {
        return function() {
            return this.identity;
        };
    }
};
console.log(object.getIdentityFunc()()); // 'The Window' 
```

### [9. 立即调用的函数表达式](#)
立即调用的匿名函数又被称作 **立即调用的函数表达式**（IIFE，Immediately Invoked Function Expression）。

```javascript
(function() {
 // 块级作用域
})(); 
```