## [Javasript 基础](#)
> **介绍**：ECMAScript 的语法很大程度上借鉴了 C 语言和其他类 C 语言，如 Java 和 Perl。熟悉这些语言的开
发者，应该很容易理解 ECMAScript 宽松的语法。

-----
- [1. Javasript语法基础](#1-javasript语法基础)
- [2. 变量声明](#2-变量声明)
- [3. javasript原生数据类型](#3-javasript原生数据类型)
- [4. undefined](#4-undefined)
- [5. Null 类型](#5-null-类型)
- [6. Boolean 类型](#6-boolean-类型)
- [7. Number 对象](#7-number-对象)
- [8. String 类型](#8-string-类型)
- [9. 模板字符串精讲](#9-模板字符串精讲)
- [10. math](#10-math)
- [11. bigInt 对象](#11-bigint-对象)
- [12. Symbol 类型](#12-symbol-类型)
- [13. Object 类型](#13-object-类型)
- [14. 函数](#14-函数)
-----

### [1. Javasript语法基础](#)
首先要知道的是，ECMAScript 中一切都区分大小写。无论是变量、函数名还是操作符，都区分大小写。换句话说，变量 test 和变量 Test 是两个不同的变量。类似地，typeof 不能作为函数名，因
为它是一个关键字（后面会介绍）。但 Typeof 是一个完全有效的函数名。

#### 1.1 标识符
**标识符**: 英文大小写字母、数字、下划线（ **_** ）和美元符号（ **$** ） (可以使用汉字或其他合法字符命名，但是不推荐)。
1. 区分大小写，第一个字符必须是一个字母、下划线（_）或美元符号（$），剩下的其他字符可以是字母、下划线、美元符号或数字；
2. 可以以 字母 $ _ 开头。  
3. 注释: 单行注释 // 多行注释 /**/ 每个语句结束最好加; 避免不必要的错误。

```javascript
"use strict";//开启严格模式

let name  = 'jxKicker';
var $name = "is you father";
const _name = ";";

// gg = "what you said?"; //在严格模式下不能运行
console.log(name,$name,_name);
console.log(gg);

/* 多行注释 在此 */
```

#### 1.2 严格模式
ECMAScript 5 增加了严格模式（strict mode）的概念。严格模式是一种不同的 JavaScript 解析和执
行模型，ECMAScript 3 的一些不规范写法在这种模式下会被处理，对于不安全的活动将抛出错误。要对整个脚本启用严格模式，在脚本开头加上这一行：
```javascript
"use strict"; 
```
虽然看起来像个没有赋值给任何变量的字符串，但它其实是一个预处理指令。任何支持的 JavaScript 引擎看到它都会切换到严格模式。选择这种语法形式的目的是不破坏 ECMAScript 3 语法。

```javascript
function doSomething() {
 "use strict";
 // 函数体
}
```
严格模式会影响 JavaScript 执行的很多方面，因此本书在用到它时会明确指出来。所有现代浏览器都支持严格模式。
* 在非严格模式下可以不声明直接使用对象等非严格操作。
* 在严格模式下JavasScript3中不确定的行为将为得到处理，不安全的操作将会抛出错误。

#### [1.3 语句](#)
ECMAScript 中的语句以分号结尾。省略分号意味着由解析器确定语句在哪里结尾：
```javascript
let sum = list.length + limit;
let gap = liner - cicer  //有解析器判断语句结尾
```
推荐一定要写分号结尾！有助于提高性能！

#### [1.4 关键字与保留字](#)
规范中也描述了一组未来的保留字，同样不能用作标识符或属性名。虽然保留字在语言中没有特定 用途，但它们是保留给将来做关键字用的。
```
abstract    arguments   boolean     break       byte
case        catch       char        class*      const
continue    debugger	default     delete      do
double	    else        enum*       eval        export*
extends*    false       final       finally     float
for         function    goto        if          implements
import*     in          instanceof  int         interface
let         long        native      new         null
package	    private     protected   public      return
short	    static      super*      switch      synchronized
this	    throw       throws      transient   true
try         typeof      var	void   volatile
while	    with        yield		
```

#### [1.5 内置对象](#)
您也应该避免使用 JavaScript 内置的对象、属性和方法的名称作为 Javascript 的变量或函数名：

```
Array       Date        eval        function        hasOwnProperty
Infinity    isFinite    isNaN       isPrototypeOf   length
Math        NaN	        name	    Number          Object
prototype   String      toString    undefined       valueOf
```

### [2. 变量声明](#)
ECMAScript 变量是松散类型的，意思是变量可以用于保存任何类型的数据。**每个变量只不过是一个用于保存任意值的命名占位符**，
可以使用三个关键字声明变量  var const let。

ECS 包含两种不同的数据类型：
* 基本类型值:简单的数据段  Undefind Null Boolean Number String Symbol 这几种类型都是按值访问
* 引用类型值:可以由多个值构成的对象 保存在内存中 按引用访问,实际操作是引用

#### [2.1 var 关键字](#)
使用var 来声明非常简单啊，但是var也有一些缺点，已经不再推荐使用！
- 变量提升。
- 非块级作用域，函数级作用域，使用var操作符定义的变量会成为包含它的函数的局部变量。
- 可重复声明。
- 不同类型之前可以相互赋值。

```javascript
//变量提升 以下代码并不会报错
let test = function(){
    console.log(age);
    var age = 25;
}
test(); //undefined

//运行时会被解析为
let test = function(){
    var age;
    console.log(age);
    age = 25;
}
```
由于var声明变量会发生变量提升，JS引擎会自动将多余的声明再作用域顶部合并为一个声明！
```javascript
//可重复声明
var age = 45;
var age = 55;
var age = -45;

//不等类型赋值
var userName = 'JxKicker';
var userName = 45;
```
可以省略var操作符定义全局变量，但不推荐这么做 在全局声明 使用 var 中声明的变量会成为window对象的属性。
```javascript
message = '这是一条全局消息';
```
> 虽然可以通过省略 var 操作符定义全局变量，但不推荐这么做。在局部作用域中定义的全局变量很难维护，也会造成困惑。这是因为不能一下子断定省略 var 是不是有意而为之。在严格模式下，如果像这样给未声明的变量赋值，则会导致抛出 ReferenceError。

##### for-var 的问题
```javascript
let funStack = [];
for (var i = 0; i < 5; i++) {
    funStack.push( () => { console.log(i); } )
}

console.log(i);//5

for (var j = 0; j < 5; j++) {
    funStack[j]();
}//5 5 5 5 5
```

#### [2.2 let 声明](#)
let 跟 var 的作用差不多，但有着非常重要的区别，最明显的区别是，let 声明的范围是块作用域， 而 var 声明的范围是函数作用域。

* 块级作用域，不可重复声明，不会再作用域被提升 **暂时性死区** ！
* 在全局声明 使用 let 中声明的变量不会成为window对象的属性。
* 不支持条件声明！

```javascript
try{
  console.log(age);
  let age = 25;
}catch (e) {
  console.error(`Error Name: ${e.name}` );//ReferenceError
  console.error(e.message); //Cannot access 'age' before initialization
}
```
使用尚未声明的let变量会抛出 ReferenceError 错误！ 表示无效引用。比如引用一个不存在的变量。
**for-let**
```javascript
let funStack = [];
for (let i = 0; i < 5; i++) {
    funStack.push( () => { console.log(i); } )
}

for (let i = 0; i < 5; i++) {
    funStack[i]();
}//0 1 2 3 4

try{
    console.log(i);//5
}catch (e) {
    console.error(Error Name: ${e.name} );//ReferenceError
}
```


#### [2.3 const 声明](#)
const 的行为和let基本一样，唯一的区别就是 const 一旦声明必须初始化，一旦初始化就不能更改其引用其值！

const 声明的限制只适用于它指向的变量的引用。换句话说，如果 const 变量引用的是一个对象，那么修改这个对象内部的属性并不违反 const 的限制。
```javascript
const userName = 'remix';
const user = {
    _Name: 'gala',
    _Age: 25
}

try {
    user._Name = 'xiaoHu';
    console.log(user); //{ _Name: 'xiaoHu', _Age: 25 }
    userName = 'umix';//不能改变值
    user = {} ;//报错 不能改变引用
}catch (e) {
    console.error(Error Name: ${e.name} );//TypeError
    console.error(e.message); //Assignment to constant variable.
}
```

### [3. Javasript原生数据类型](#)
**ECMAScript** 有如下几种简单数据类型 **Undefined**、 **Null**、**Boolean**、**String**、**Number**、**Object**、**Function**、**Symbol** 可以使用 typeof 判断类型。

1. 没必要将一个变量初始化为 Undefined 因为它是自动的
2. **undefined 派生自null**  所以两个相等,**未经初始化的变量自动是 undefined**, **null表示一个空对象引用 typeof 返回类型为 object**。
3. function本质上也是 object 但是 typeof返回function
4. 可以使用 typeof 运算符来粗略的确定数据类型

> 严格来讲，函数在 ECMAScript 中被认为是对象，并不代表一种数据类型。可是，函数也有自己特殊的属性。为此，就有必要通过 typeof 操作符来区分函数和其他对象。

**例子**：
```javascript
let gala;  //undefined
let xiaohu = null;
let theshy = true;
let langx = 20;
let wei = function () {}
let bom = Symbol('remix');
let user = new Object({name: 'kicker', age: 20});
let str = 'umix';

console.log(typeof gala); //undefined
console.log(typeof xiaohu);//object
console.log(typeof theshy); //boolean
console.log(typeof langx); //number

console.log(typeof wei);//function
console.log(typeof bom);//symbol
console.log(typeof user);//object
console.log(typeof str); //string
```
因为 ECMAScript 的类型系统是松散的，所以需要一种手段来确定任意变量的数据类型。typeof 操作符就是为此而生的。对一个值使用 typeof 操作符会返回下列字符串之一：
* "undefined"表示值未定义；
* "boolean"表示值为布尔值；
* "string"表示值为字符串；
* "number"表示值为数值；
* "object"表示值为对象（而不是函数）或 null；
* "function"表示值为函数；

### [4. Undefined](#)
对于尚未初始化的变量,会被JS引擎会自动初始化为 Undefined。
typeof 变量的问题， 如果变量声明了没有初始化那么返回 undefined，如果这个变量都没有声明过那么也返回 ndefined

```javascript
let name;

if (typeof name == "undefined"){
    console.log('name 尚未初始化');
}

if (typeof age == "undefined"){
    console.log('age 尚未初始化或声明');
}
```
注意，包含 undefined 值的变量跟未定义变量是有区别的。
```javascript
let message; // 这个变量被声明了，只是值为 undefined
// 确保没有声明过这个变量
// let age
console.log(message); // "undefined"
console.log(age); // 报错
```
**对未初始化的变量调用 typeof 时，返回的结果是"undefined"，但对未声明的变量调用它时，返回的结果还是"undefined"**，这就有点让人看不懂了。
```javascript
let message; // 这个变量被声明了，只是值为 undefined
// 确保没有声明过这个变量
// let age
console.log(typeof message); // "undefined"
console.log(typeof age); // "undefined"
```
如何区分,如果需要，可以用更简洁的方式检测它。
```javascript
let message; // 这个变量被声明了，只是值为 undefined
// age 没有声明
if (message) {
 // 这个块不会执行
}
if (!message) {
 // 这个块会执行
}
if (age) {
 // 这里会报错
} 
```

转换为 Boolean 值返回为 false

### [5. Null 类型](#)
**Null 类型** 同样只有一个值，即特殊值 null。逻辑上讲，null 值表示一个空对象指针，这也是给typeof 传一个 null 会返回"object"的原因：

* 在定义将来要保存对象值的变量时，建议使用 null 来初始化，不要使用其他值。
```javascript
let car = null;
console.log(typeof car); // "object" 

let name = null;

if ( name ){
    console.log('这里不会执行');
}

//判断类型是否是null
if (name === null){ 
    console.log('name 是 null');
}
```
**转换为 Boolean 值返回为 false**, undefined 值是由 null 值派生而来的，因此 ECMA-262 将它们定义为表面上相等，如下面的例子所示：
```javascript
console.log(null == undefined); // true
```

### [6. Boolean 类型](#)
Boolean（布尔值）类型是 ECMAScript 中使用最频繁的类型之一，有两个字面值：true 和 false。

* Boolean 对象是 Boolean 原始类型的引用类型。要创建 Boolean 对象，只需要传递 Boolean 值作为参数：
* Boolean 对象将覆盖 Object 对象的 ValueOf() 方法，返回原始值，即 true 和 false。ToString() 方法也会被覆盖，返回字符串 "true" 或 "false"。
* 遗憾的是，在 ECMAScript 中很少使用 Boolean 对象，即使使用，也不易理解。
* 平时值类型的布尔类型的实际

```javascript
let found = true;
let lost = false;
```

|数据类型|转换为true|转换为false| 
|--|:--:|----------:|
|Boolean|true|    false  |
|String|任何非空字符|       空字符 |
|Number|任何非零数字值|     0和NaN |
|Object|任何对象|      null |
|Undefined|undefined 只能转换为 false| undefined |

可以使用Boolean(obj) 将一个值转换为Boolean值

平时值类型的布尔类型的实际
```javascript
let val = true;
console.log(val.toString());
```
**val** 明明是值类型 为何还能够调用方法呢？ 类似于引用类型
```javascript
// 实际做法 做了包装 在不调用方法的时候就是值类型 将要调用方法的时候将其转换为引用类型 
// 然后调用结束后 再转换会值类型  有一个包装的过程
let val = new Boolean(true);
console.log(val.toString());
```

### [7. Number 对象](#)
ECMAScript 中最有意思的数据类型或许就是 Number 了。Number 类型使用 IEEE 754 格式表示整
数和浮点值（在某些语言中也叫双精度值）。不同的数值类型相应地也有不同的数值字面量格式。
```javascript
//引用类型
let oNumberObject = new Number(68);
//要得到数字对象的 Number 原始值，只需要使用 valueOf() 方法
let iNumber = oNumberObject.valueOf();
//值类型
let age = 78;
```

Number的属性： **MAX_VALUE  MIN_VALUE  NEGATIVE_INFINITY  POSITIVE_INFINITY**。

```javascript
let max = Number.MAX_VALUE //获得最大值
let min = Number.MIN_VALUE //获得最小值
let negative_infinity = Number.NEGATIVE_INFINITY; // 正无穷
let positive_infinity = Number.POSITIVE_INFINITY; // 负无穷

console.log(max) //1.7976931348623157e+308
console.log(min) //5e-324
console.log(isFinite(max)) //true
```

#### [7.1 Number类型的方法](#)
* toFixed() :方法返回的是具有指定位数小数的数字的字符串表示。
* toExponential() 方法:与格式化数字相关的另一个方法是 toExponential()，它返回的是用科学计数法表示的数字的字符串形式。
* toPrecision() 方法:toPrecision() 方法根据最有意义的形式来返回数字的预定形式或指数形式。它有一个参数，即用于表示数的数字总数（不包括指数）。如下所示
* Number.isNaN():判断是否是非数字，只要能被转换为Number 就都返回true
* Number.isInteger():判断是否是整数
* Number.isSafeInteger():判断是否是安全整数
* Number.parseFloat(v):转换为float
* Number.parseInt(v)：v 转换为int
* Number.parseInt(v,p)：v 转换为int p表示v是几进制数 例如 Number.parseInt(10,16) //结果为:16

```javascript
let oNumberObject = new Number(68);
console.log(oNumberObject.toFixed(2));  //输出 "68.00"
console.log(oNumberObject.toExponential(1));  //输出 "6.8e+1"

console.log(oNumberObject.toPrecision(1));  //输出 "7e+1"
console.log(oNumberObject.toPrecision(2));  //输出 "68"
console.log(oNumberObject.toPrecision(3));  //输出 "68.0"
```

存储浮点值使用的内存空间是存储整数值的两倍，所以 ECMAScript 总是想方设法把值转换为整数。在小数点后面没有数字的情况下，数值就会变成整数。

#### [7.2 进制表示](#)
最基本的数值字面量格式是十进制整数 八进制 使用前缀 0 十六进制 0x。
```javascript
let kicker = 54;
let umix = 070 ; //56 八进制
let remix = 0x1f ; //31 十六进制
```

#### [7.3 Number 数值转换](#)
有 3 个函数可以将非数值转换为数值：Number()、parseInt()和 parseFloat()。Number()是转型函数，可用于任何数据类型。后两个函数主要用于将字符串转换为数值。对于同样的参数，这 3 个函数执行的操作也不同。

**Number() 函数基于如下规则执行转换**。
* 布尔值，true 转换为 1，false 转换为 0。
* 数值，直接返回。
* null，返回 0。
* undefined，返回 NaN。
* 字符串，应用以下规则。
  * 如果字符串包含数值字符，包括数值字符前面带加、减号的情况，则转换为一个十进制数值。 因此，`Number("1")` 返回 1，`Number("123")` 返回 123，`Number("011")` 返回 11（忽略前面 的零）。
  * 如果字符串包含有效的浮点值格式如"1.1"，则会转换为相应的浮点值（同样，忽略前面的零）。
  * 如果字符串包含有效的十六进制格式如"0xf"，则会转换为与该十六进制值对应的十进制整数值。
  * 如果是空字符串（不包含字符），则返回 0。
  * 如果字符串包含除上述情况之外的其他字符，则返回 NaN。
* 对象，调用 valueOf()方法，并按照上述规则转换返回的值。如果转换结果是 NaN，则调用toString()方法，再按照转换字符串的规则转换。

```javascript
var num1 = Number("6546sdf")
var num2 = Number("sdf")
var num3 = Number("")
var num4 = Number("00056")
var num5 = Number(true)
var num6 = Number(null)
var num7 = Number(undefined)
console.log(num1,num2,num3,num4,num5,num6,num7) 
         // NaN   NaN  0    56   1    0    NaN
```
parseInt只认数字或者以数字开头 近能够进行进制转换
```javascript
var num1 = parseInt("6546sdf",10);// 按照十进制解析
var num2 = parseInt("sdf")
var num3 = parseInt("")
var num4 = parseInt("00056", 8);//按照八进制解析
var num5 = parseInt(true)
var num6 = parseInt(null)
var num7 = parseInt(undefined)
var num8 = parseInt("523",8) // 转换8进制
var num9 = parseInt("AAA",16) //  16进制
console.log(num1,num2,num3,num4,num5,num6,num7,num8,num9) 
         // 6546  NaN  NaN  56   NaN  NaN NaN  339  2730
```
parseFloat只认数字或者以数字开头 不能够进行进制转换
```javascript
var num1 = parseFloat("4554.2jj")
var num2 = parseFloat("0085.2")
var num3 = parseFloat("15.2e5")
var num4 = parseFloat("asdsa")
var num5 = parseFloat(true)
var num6 = parseFloat(null)
var num7 = parseFloat(undefined)
var num8 = parseFloat("0523",8) // 转换8进制
var num9 = parseFloat("0xAAA",16) //  16进制
console.log(num1,num2,num3,num4,num5,num6,num7,num8,num9) 
         // 4554.2 85.2 1520000 NaN NaN NaN NaN 523 0
```

#### [7.4 浮点值](#)
要定义浮点值，数值中必须包含小数点，而且小数点后面必须至少有一个数字，浮点数采用 IEEE 754数值标准。

```javascript
let float_number_one = 1. ; //. 后面有问题 当作整数 1 处理
let float_number_two = 10.0 ; //小数点后面是 0 当作整数处理

let score =  39.0;
console.log(score);//39
```
因为存储浮点值使用的内存空间是存储整数值的两倍，所以 ECMAScript 总是想方设法把值转换为整数。

#### [7.5 NaN](#)
有一个特殊的数值叫 NaN，意思是“不是数值”（Not a Number），用于表示本来要返回数值的操作
失败了（而不是抛出错误）。比如，用 0 除任意数值在其他语言中通常都会导致错误，从而中止代码执
行。但在 ECMAScript 中，0、+0 或 0 相除会返回 NaN：
```javascript
console.log(0/0); // NaN
console.log(-0/+0); // NaN
```
如果分子是非 0 值，分母是有符号 0 或无符号 0，则会返回 Infinity 或-Infinity：
```javascript
console.log(5/0); // Infinity
console.log(5/-0); // -Infinity
```
NaN 有几个独特的属性。首先，任何涉及 NaN 的操作始终返回 NaN（如 NaN/10），在连续多步计算
时这可能是个问题。其次，NaN 不等于包括 NaN 在内的任何值。例如，下面的比较操作会返回 false：

```javascript
console.log(NaN == NaN); // false
```
为此，ECMAScript 提供了 isNaN()函数。该函数接收一个参数，可以是任意数据类型，然后判断
这个参数是否“不是数值”。把一个值传给 isNaN()后，该函数会尝试把它转换为数值。某些非数值的
值可以直接转换成数值，如字符串"10"或布尔值。任何不能转换为数值的值都会导致这个函数返回
true。举例如下：

```javascript
console.log(isNaN(NaN)); // true 
console.log(isNaN(10)); // false，10 是数值
console.log(isNaN("10")); // false，可以转换为数值 10 
console.log(isNaN("blue")); // true，不可以转换为数值
console.log(isNaN(true)); // false，可以转换为数值 1
```

### [8. String 类型](#)
String（字符串）数据类型表示零或多个 16 位 Unicode 字符序列。字符串可以使用双引号（"）、 单引号（'）或反引号（`）标示，因此下面的代码都是合法的：

String 官方文档解释方法属性 String 是一个基本包装类型,它是唯一的,一旦创建不能改变,每次改变实际上都是创建一个新的字符串 可以使用单引号 也可以使用双引号表示

```javascript
let userName = 'remix';
let message = 'this is a message';

let sentence = ${userName} said: "${message}"!;

console.log(sentence); //remix said: "this is a message"!
```
JavaScript字符串字面量还支持一些特殊字符的转义，如下表所示：

| 转义字符    | 	描述      |
|:--------|:---------|
| `\\`    | 反斜杠字符 `\`| 
| `\"`	   |双引号|
| `\'`	   |单引号|
| `\n`	   |换行符|
| `\r`	   |回车符|
| `\t`	   |制表符|
| `\b`	   |退格符|
| `\f`	   |换页符|
| `\uXXXX`	 |以16进制编码的Unicode字符（其中XXXX为4位数）|

> **特点** : ECMAScript 中的字符串是不可变的（immutable），意思是一旦创建，它们的值就不能变了。要修改
某个变量中的字符串值，必须先销毁原始的字符串，然后将包含新值的另一个字符串保存到该变量。

#### [8.1 String 转换 toString](#)
有两种方法将一个其他类型转换为字符串

1. **toString(n)** 方法 n 表示 蒋数字转换为几进制 默认值 10
2. **String(带转换变量)** undefined, null 没有 toString方法！

如果值是undefined 则返回字符串 undefined 。null 同理一样！如果数字是非数字 那么返回NaN 
```javascript
let value = 1052;

console.log(value.toString(2)); //二进制 10000011100
console.log(value.toString(8));//八进制 2034
console.log(value.toString(16));//八进制 41c

let und;
let nl = null;
console.log(String(und)); //undefined
console.log(String(nl)); //null
```

#### [8.3 模板字面量与字符串插值](#)
ECMAScript 6 新增了使用[模板字面量](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Template_literals)定义字符串的能力。与使用单引号或双引号不同，模板字面量保留换行字符，可以跨行定义字符串：

模板字面量有时被非正式地叫作模板字符串，因为它们最常被用作字符串插值（通过替换占位符来创建字符串）。

```javascript
const name = "Alice";
const age = 25;

// 不带标签的模板字面量
const greeting = `Hello, ${name}! You are ${age} years old.`;
console.log(greeting); // 输出: Hello, Alice! You are 25 years old.
```

#### [8.4 模板字面量标签函数](#)
模板字面量也支持定义标签函数（tag function），而通过标签函数可以自定义插值行为。标签函数会接收被插值记号分隔后的模板和对每个表达式求值的结果。

```javascript
let a = 6; 
let b = 9; 
function simpleTag(strings, aValExpression, bValExpression, sumExpression) { 
 console.log(strings); 
 console.log(aValExpression); 
 console.log(bValExpression); 
 console.log(sumExpression); 
 return 'foobar'; 
} 
let untaggedResult = `${ a } + ${ b } = ${ a + b }`; 
let taggedResult = simpleTag`${ a } + ${ b } = ${ a + b }`; 
// ["", " + ", " = ", ""] 
// 6 
// 9 
// 15
```
因为表达式参数的数量是可变的，所以通常应该使用剩余操作符（rest operator）将它们收集到一个数组中：

```javascript
let a = 6; 
let b = 9; 
function simpleTag(strings, ...expressions) { 
 console.log(strings); 
 for(const expression of expressions) { 
 console.log(expression); 
 } 
 return 'foobar'; 
} 
let taggedResult = simpleTag`${ a } + ${ b } = ${ a + b }`; 
// ["", " + ", " = ", ""] 
// 6 
// 9 
// 15 
console.log(taggedResult); // "foobar"
```

#### [8.5 原始字符串](#)
使用模板字面量也可以直接获取原始的模板字面量内容（如换行符或 Unicode 字符），而不是被转换后的字符表示。为此，可以使用默认的 String.raw 标签函数：

```javascript
// Unicode 示例
// \u00A9 是版权符号
console.log(`\u00A9`); // © 
console.log(String.raw`\u00A9`); // \u00A9 
// 换行符示例
console.log(`first line\nsecond line`); 
// first line 
// second line 
console.log(String.raw`first line\nsecond line`); // "first line\nsecond line"
```
另外，也可以通过标签函数的第一个参数，即字符串数组的.raw 属性取得每个字符串的原始内容：

```javascript
function printRaw(strings) {
console.log('Actual characters:');
for (const string of strings) {
console.log(string);
}
console.log('Escaped characters;');
for (const rawString of strings.raw) {
console.log(rawString);
}
}
printRaw`\u00A9${ 'and' }\n`;
// Actual characters:
// ©
//（换行符）
// Escaped characters:
// \u00A9
// \n
```


#### [8.6 String ES6 改造](#)
JS5的字符串是基于16位的UTF-16编码进行构建的,每十六位表示一个编程单元,代表一个字符,许多字符串方法和属性都是基于此编程单元的的,但是过去16足够了但是 Unicode引入扩展字符集,
编码规则不得不进行变更,所有不再限制在16位,扩展到了32位,一个字符对应一个码位例如 55362 表示 吉这个字符.UTF-16中,前面216码位 均以16位的编码单位表示,这个范围被称为基于多文
中平面(BMP)超过这个范围的码位则要归属于某个辅助平面,其中码位仅用16位就无法表示了,为此UTF-16引入 代理对 其规定用两个16位编码单位表示一个码位,也就是说,字符串里的字符有两种,
一种是16位编码单位的BMP字符,一种是两个编码单位的32位字符串,为此产生了很多的问题

1.采用了代理对的一个字符串长度为2,例如 let val="𠮷"; console.log(val.length); 结果长度为2
2.不利于对字符串进行排序和比较,要使用normalize() 方法进行等效关系标准化.

#### [8.7 针对代理对编码提供了码点方法](#)
codePointAt() 方法, 能够正确处理 4(32位) 个字节储存的字符，返回一个字符的码点。 charCodeAt() 方法只能返回16位的码点,无法兼容到代理对  
* 
* ES5 提供String.fromCharCode方法，用于从码点返回对应字符，但是这个方法不能识别 32 位的 UTF-16 字符（Unicode 编号大于0xFFFF）。  
* ES6 提供了 **String.fromCodePoint** 方法，可以识别大于0xFFFF的字符，弥补了String.fromCharCode方法的不足。在作用上，正好与codePointAt方法相反。

```javascript
let g="𠮷";
console.log("[𠮷] 长度为:"+g.length);    
console.log(g.charCodeAt(0));
console.log(g.codePointAt(0));
//Unicode 代理对
console.log(String.fromCodePoint(134071)); 
console.log(String.fromCharCode(134071)); 
//输出
/*
    [𠮷] 长度为:2
    55362
    134071
    𠮷
    ஷ    
*/
//判断是否是代理对
function is32Bit(code){
    return code.codePointAt(0)>0XFFFF;
}
```
注意:fromCodePoint方法定义在String对象上，而codePointAt方法定义在字符串的实例对象上。

#### [8.8 标准化 normalize](#)
许多欧洲语言有语调符号和重音符号。为了表示它们，Unicode 提供了两种方法。一种是直接提供带重音符号的字符，比如Ǒ（\u01D1）。另一种是提供合成符号（combining character），即原字
符与重音符号的合成，两个字符合成一个字符，比如O（\u004F）和ˇ（\u030C）合成Ǒ（\u004F\u030C）。这两种表示方法，在视觉和语义上都等价，但是 JavaScript 不能识别。JavaScript 将合
成字符视为两个字符，导致两种表示方法不相等。

* ES6 提供字符串实例的normalize()方法，用来将字符的不同表示方法统一为同样的形式，这称为 Unicode 正规化。

* normalize方法可以接受一个参数来指定normalize的方式，参数的四个可选值如下。
    * NFC，默认参数，表示“标准等价合成”（Normalization Form Canonical Composition），返回多个简单字符的合成字符。所谓“标准等价”指的是视觉和语义上的等价。
    * NFD，表示“标准等价分解”（Normalization Form Canonical Decomposition），即在标准等价的前提下，返回合成字符分解的多个简单字符。
    * NFKC，表示“兼容等价合成”（Normalization Form Compatibility Composition），返回合成字符。所谓“兼容等价”指的是语义上存在等价，但视觉上不等价，比如“囍”和“喜喜”。（这只是用来举例，normalize方法不能识别中文。）
    * NFKD，表示“兼容等价分解”（Normalization Form Compatibility Decomposition），即在兼容等价的前提下，返回合成字符分解的多个简单字符。
* normalize方法目前不能识别三个或三个以上字符的合成，默认 NFD 形式
```javascript
'\u01D1'==='\u004F\u030C' //false

'\u01D1'.length // 1
'\u004F\u030C'.length // 2

 console.log('\u01D1'.normalize() === '\u004F\u030C'.normalize());
// true
'\u004F\u030C'.normalize('NFC').length // 1
'\u004F\u030C'.normalize('NFD').length // 2
// 上面代码表示，NFC参数返回字符的合成形式，NFD参数返回字符的分解形式。
```
```javascript
let values=["𠮷","天","下","王","者","J","i","1","5","q"];
//默认排序
console.log(values.sort());

//标准化后 第一种排序加标准化
let normalized=values.map(function(text){
    return text.normalize();
})
console.log(normalized.sort(function(one,two){
           if(one <two){
               return 1;
           }else if(one == two){
               return 0;
           }else{
               return -1;
           }
    })
);
// 第二种排序加标准化
console.log(values.sort(function(first,second){
    let firstNormalized=first.normalize();
    let secondNormalized=second.normalize();
    if(firstNormalized <secondNormalized){
        return 1;
    }else if(firstNormalized == secondNormalized){
        return 0;
    }else{
        return -1;
    }
}));
```

#### [8.9 字符串遍历器](#)
for...of循环遍历，除了遍历字符串，这个遍历器最大的优点是可以识别大于0xFFFF的码点，传统的for循环无法识别这样的码点。
```javascript
for (let codePoint of 'foo') {
  console.log(codePoint)
}
// "f"
// "o"
// "o"
```
除了遍历字符串，这个遍历器最大的优点是可以识别大于0xFFFF的码点，传统的for循环无法识别这样的码点。

```javascript
let text = String.fromCodePoint(0x20BB7);

for (let i = 0; i < text.length; i++) {
  console.log(text[i]);
}
// " "
// " "

for (let i of text) {
  console.log(i);
}
// "𠮷
```

#### [8.10 直接输入](#)
JavaScript 字符串允许直接输入字符，以及输入字符的转义形式。举例来说，“中”的 Unicode 码点是 U+4e2d，你可以直接在字符串里面输入这个汉字，也可以输入它的转义形式\u4e2d，两者是等价的。

```javascript
'中' === '\u4e2d' // true
```
JavaScript 规定有5个字符，不能在字符串里面直接使用，只能使用转义形式。

* U+005C：反斜杠（reverse solidus)
* U+000D：回车（carriage return）
* U+2028：行分隔符（line separator）
* U+2029：段分隔符（paragraph separator）
* U+000A：换行符（line feed）

举例来说，字符串里面不能直接包含反斜杠，一定要转义写成\\或者\u005c。

这个规定本身没有问题，麻烦在于 JSON 格式允许字符串里面直接使用 U+2028（行分隔符）和 U+2029（段分隔符）。这样一来，服务器输出的 JSON 被JSON.parse解析，就有可能直接报错。

```javascript
const json = '"\u2028"';
JSON.parse(json); // 可能报错
```
JSON 格式已经冻结（RFC 7159），没法修改了。为了消除这个报错，ES2019 允许 JavaScript 字符串直接输入 U+2028（行分隔符）和 U+2029（段分隔符）。

```node
const PS = eval("'\u2029'");
```
### [9. 模板字符串精讲](#)
传统的 JavaScript 语言，输出模板通常是这样写的（下面使用了 jQuery 的方法）。
```javascript
$('#result').append(
  'There are <b>' + basket.count + '</b> ' +
  'items in your basket, ' +
  '<em>' + basket.onSale +
  '</em> are on sale!'
);
```
上面这种写法相当繁琐不方便，ES6 引入了模板字符串解决这个问题。

```javascript
$('#result').append(
   `
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
  `
);
```
模板字符串（template string）是增强版的字符串，用反引号（&#180;）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。
```
// 普通字符串
In JavaScript '\n' is a line-feed.

// 多行字符串
In JavaScript this is
 not legal.

console.log(string text line 1
string text line 2);

// 字符串中嵌入变量
let name = "Bob", time = "today";
Hello ${name}, how are you ${time}?
```
上面代码中的模板字符串，都是用反引号表示。如果在模板字符串中需要使用反引号，则前面要用反斜杠转义。

```javascript
let greeting = `\Yo\ World!`;
```
如果使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中。

```javascript
$('#list').html(
<ul>
  <li>first</li>
  <li>second</li>
</ul>
);
```
上面代码中，所有模板字符串的空格和换行，都是被保留的，比如<ul>标签前面会有一个换行。如果你不想要这个换行，可以使用trim方法消除它。

```javascript
$('#list').html(
<ul>
  <li>first</li>
  <li>second</li>
</ul>
.trim());
```
模板字符串中嵌入变量，需要将变量名写在${}之中。
```node
function authorize(user, action) {
  if (!user.hasPrivilege(action)) {
    throw new Error(
      // 传统写法为
      // 'User '
      // + user.name
      // + ' is not authorized to do '
      // + action
      // + '.'
      User ${user.name} is not authorized to do ${action}.);
  }
}
```
大括号内部可以放入任意的 JavaScript 表达式，可以进行运算，以及引用对象属性。

```javascript
let x = 1;
let y = 2;

`${x} + ${y} = ${x + y}`
// "1 + 2 = 3"

`${x} + ${y * 2} = ${x + y * 2}`
// "1 + 4 = 5"

let obj = {x: 1, y: 2};
${obj.x + obj.y}
// "3"
```
模板字符串之中还能调用函数。

```javascript
function fn() {
  return "Hello World";
}

`foo ${fn()} bar``
// foo Hello World bar
```
如果大括号中的值不是字符串，将按照一般的规则转为字符串。比如，大括号中是一个对象，将默认调用对象的toString方法。

如果模板字符串中的变量没有声明，将报错。
```javascript
// 变量place没有声明
let msg = `Hello, ${place};`
// 报错
```
由于模板字符串的大括号内部，就是执行 JavaScript 代码，因此如果大括号内部是一个字符串，将会原样输出。
```javascript
`Hello ${'World'}`
// "Hello World"
```
模板字符串甚至还能嵌套。

```javascript
const tmpl = addres => `<table>
    ${
        addres.map(addr => `
            <tr><td>${addr.first}</td></tr>
            <tr><td>${addr.last}</td></tr>
        `).join('')
}
</table>`;
```
上面代码中，模板字符串的变量之中，又嵌入了另一个模板字符串，使用方法如下。

```javascript
const data = [
    { first: '<Jane>', last: 'Bond' },
    { first: 'Lars', last: '<Croft>' },
];

console.log(tmpl(data));
// <table>
//
//   <tr><td><Jane></td></tr>
//   <tr><td>Bond</td></tr>
//
//   <tr><td>Lars</td></tr>
//   <tr><td><Croft></td></tr>
//
// </table>
```
如果需要引用模板字符串本身，在需要时执行，可以写成函数。

```javascript
let func = (name) => Hello ${name}!;
func('Jack') // "Hello Jack!"
```
上面代码中，模板字符串写成了一个函数的返回值。执行这个函数，就相当于执行这个模板字符串了。

#### [9.1 标签模板](#)
模板字符串的功能，不仅仅是上面这些。它可以紧跟在一个函数名后面，该函数将被调用来处理这个模板字符串。
这被称为“标签模板”功能（tagged template）。
```node
fmode123
// 等同于
fmode(123)

let a = 5;
let b = 10;

tagHello ${ a + b } world ${ a * b };
// 等同于
tag(['Hello ', ' world ', ''], 15, 50);
```
* 函数tag依次会接收到多个参数。
```node
function tag(stringArr, ...values){
  // ...
}
```
```node
let a = 5;
let b = 10;

function tag(s, v1, v2) {
  console.log(s[0]);
  console.log(s[1]);
  console.log(s[2]);
  console.log(v1);
  console.log(v2);

  return "OK";
}

tagHello ${ a + b } world ${ a * b}; //=tag(['Hello ', ' world ', ''], 15, 50)
// "Hello "
// " world "
// ""
// 15
// 50
// "OK"
```
末班实例
```node
let total = 30;
let msg = passthruThe total is ${total} (${total*1.05} with tax);

function passthru(literals) {
  let result = '';
  let i = 0;

  while (i < literals.length) {
    result += literals[i++];
    if (i < arguments.length) {
      result += arguments[i];
    }
  }

  return result;
}
```

#### [9.2 ES6 扩展的String 方法](#)
* str.includes(string [,indexStart]) ：返回布尔值，表示是否找到了参数字符串。
* str.startsWith(string [,indexStart]) ：返回布尔值，表示参数字符串是否在原字符串的头部。
* str.endsWith(string [,indexStart]) ：返回布尔值，表示参数字符串是否在原字符串的尾部。
* str.repeat(number) :方法返回一个新字符串，表示将原字符串重复n次。
* str.padStart(time,string) : 用于头部补全
* str.padEnd(time,string) : 用于尾部补全
* str.matchAll(pattern):方法返回一个正则表达式在当前字符串的所有匹配
```javascript
let s = 'Hello world!';

s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true
```
* 这三个方法都支持第二个参数，表示开始搜索的位置。

```javascript
let s = 'Hello world!';

s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
```
* repeat
```javascript
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""
```
* padStart
```javascript
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
```

#### [9.3 String.raw()](#)
ES6 还为原生的 String 对象，提供了一个raw方法。往往用来充当模板字符串的处理函数，返回一个斜杠都被转义（即斜杠前面再加一个斜杠）的字符串，对应于替换变量后的模板字符串。
```javascript
String.rawHi\n${2+3}!;
// 返回 "Hi\\n5!"

String.rawHi\u000A!;
// 返回 "Hi\\u000A!"

String.rawHi\\n
// 返回 "Hi\\\\n"
```
作为函数，String.raw的代码实现基本如下。

```javascript
String.raw({ raw: 'test' }, 0, 1, 2); // 't0e1s2t'
// 注意这个测试，传入一个 string，和一个类似数组的对象
// 下面这个函数和 foo${2 + 3}bar${'Java' + 'Script'}baz 是相等的。

String.raw({
  raw: ['foo', 'bar', 'baz']
}, 2 + 3, 'Java' + 'Script'); // 'foo5barJavaScriptbaz'


String.raw = function (strings, ...values) {
  let output = '';
  let index;
  for (index = 0; index < values.length; index++) {
    output += strings.raw[index] + values[index];
  }

  output += strings.raw[index]
  return output;
}
```
### [10. Math](#)
Math 对象用于执行数学任务。Math 对象并不像 Date 和 String 那样是对象的类，因此没有构造函数 Math()。

```javascript
let x = Math.PI; // 返回 PI
let y = Math.sqrt(16); // 返回 16 的平方根
```
|Math|对象属性|
|----|----|
|属性|描述|
|E|返回算术常量 e，即自然对数的底数（约等于2.718）。|
|LN2|返回 2 的自然对数（约等于0.693）。|
|LN10|返回 10 的自然对数（约等于2.302）。|
|LOG2E|返回以 2 为底的 e 的对数（约等于 1.4426950408889634）。|
|LOG10E|返回以 10 为底的 e 的对数（约等于0.434）。|
|PI|返回圆周率（约等于3.14159）。|
|SQRT1_2|返回 2 的平方根的倒数（约等于 0.707）。|
|SQRT2|返回 2 的平方根（约等于 1.414）。|

* abs(x)	返回 x 的绝对值。
* pow(x,y)	返回 x 的 y 次幂。
* random()	返回 0 ~ 1 之间的随机数。
* exp(x)	返回 Ex 的指数。
* round(x)	四舍五入。
* trunc(x)	将数字的小数部分去掉，只保留整数部分。
* max(x,y,z,...,n)	返回 x,y,z,...,n 中的最高值。
* min(x,y,z,...,n)	返回 x,y,z,...,n中的最低值。
* floor(x)	对 x 进行下舍入。
* log(x)	返回数的自然对数（底为e）。
* ceil(x)	对数进行上舍入。
* sqrt(x)	返回数的平方根。

#### [10.1 Math 对象的扩展](#)
Math.trunc() 方法用于去除一个数的小数部分，返回整数部分。
```javascript
Math.trunc(4.9) // 4
Math.trunc(-4.1) // -4
```
Math.sign() 方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。
* 它会返回五种值。
    * 参数为正数，返回+1;
    * 参数为负数，返回-1;
    * 参数为 0，返回0;
    * 参数为-0，返回-0;
    * 其他值，返回NaN。
```javascript
Math.sign(-5) // -1
Math.sign(5) // +1
Math.sign(0) // +0
Math.sign(-0) // -0
Math.sign(NaN) // NaN
```
Math.cbrt() 方法用于计算一个数的立方根。
```javascript
Math.cbrt(-1) // -1
Math.cbrt(0)  // 0
Math.cbrt(1)  // 1
Math.cbrt(2)  // 1.2599210498948734

Math.cbrt = Math.cbrt || function(x) {
  var y = Math.pow(Math.abs(x), 1/3);
  return x < 0 ? -y : y;
};
```

Math.clz32() Math.clz32()方法将参数转为 32 位无符号整数的形式，然后返回这个 32 位值里面有多少个前导 0

```javascript
Math.clz32(0) // 32
Math.clz32(1) // 31
Math.clz32(1000) // 22
Math.clz32(0b01000000000000000000000000000000) // 1
Math.clz32(0b00100000000000000000000000000000) // 2
```

### [11. BigInt 对象](#)
JavaScript 原生提供BigInt对象，可以用作构造函数生成 BigInt 类型的数值。转换规则基本与Number()一致，将其他类型的值转为 BigInt。

```javascript
BigInt(123) // 123n
BigInt('123') // 123n
BigInt(false) // 0n
BigInt(true) // 1n
```

BigInt()构造函数必须有参数，而且参数必须可以正常转为数值，下面的用法都会报错。
```javascript
new BigInt() // TypeError
BigInt(undefined) //TypeError
BigInt(null) // TypeError
BigInt('123n') // SyntaxError
BigInt('abc') // SyntaxError

/* 参数如果是小数，也会报错。*/

BigInt(1.5) // RangeError
BigInt('1.5') // SyntaxError
```

上面代码中，尤其值得注意字符串123n无法解析成 Number 类型，所以会报错。

* BigInt.asUintN(width, BigInt)： 给定的 BigInt 转为 0 到 2width - 1 之间对应的值。
* BigInt.asIntN(width, BigInt)：给定的 BigInt 转为 -2width - 1 到 2width - 1 - 1 之间对应的值。
* BigInt.parseInt(string[, radix])：近似于Number.parseInt()，将一个字符串转换成指定进制的 BigInt。

```javascript
const max = 2n ** (64n - 1n) - 1n;

BigInt.asIntN(64, max)
// 9223372036854775807n
BigInt.asIntN(64, max + 1n)
// -9223372036854775808n
BigInt.asUintN(64, max + 1n)
// 9223372036854775808n
```

### [12. Symbol 类型](#)
ES6 新增的类型，Symbol是原始值，且符号实例是唯一、不可变的。符号的用途是确保对象属性使用唯一标识符！不会发生属性冲突的危险！可以用来实现接口操作！

尽管听起来跟私有属性有点类似，但符号并不是为了提供私有属性的行为才增加的（尤其是因为Object API 提供了方法，可以更方便地发现符号属性）。相反，符号就是用来创建唯一记号，进而用作非 字符串形式的对象属性。

> Symbol 类型主要用于创建唯一的标识符，解决了对象属性名称冲突的问题

#### 12.1 符号的创建
符号要是有Symbol 函数初始化,本身是原始类型，typeof 返回 symbol。 Symbol(str) 不能和 new一起使用。即不能当构造函数，这样做是为了比卖你创建符号包装对象！

symbol Symbol(str):返回一个符号原始值，这个唯一值不能更变，可以传入一个字符串参数作为对符号的描述！这个字符串参数与符号定义或标识无关，仅仅是一个描述！ 如果两个符号的描述一样，其值也不一样！
```javascript
let nameSymbol = Symbol('name symbol');
let nameSymbol_ = Symbol('name symbol');
let ageSymbol = Symbol();

console.log(nameSymbol, ageSymbol);//Symbol(name symbol) Symbol()
console.log(nameSymbol == nameSymbol_);//false

//不建议 包装 symbol
let wrapper =  Object(ageSymbol);
console.log(wrapper);//[Symbol: Symbol()]
```

#### [12.2 使用全局符号注册表](#)
如果运行时的不同部分需要共享和重用符号实例，那么可以用一个字符串作为键，在全局符号注册表中创建并重用符号！使用 `Symbol.for(key)` 方法！

如果未创建则创建新符号然后返回，已创建则找到后返回！
```javascript
let nameSymbolGlobal = Symbol.for('name');
let names = Symbol.for('name');
let iterator = Symbol.iterator; //内置对象
let iterator_ = Symbol.iterator;

console.log(iterator == iterator_);//true
console.log(nameSymbolGlobal == names);//true
```
全局注册的符号不等于使用函数常见的符号，即使描述等于键！

```javascript
let nameSymbolGlobal = Symbol.for('name');
let names = Symbol('name');


console.log(nameSymbolGlobal == names);//false
```
还可以反过来用符号查询键值 使用方法:  `Symbol.keyFor(symbol)`

```javascript
let nameSymbolGlobal = Symbol.for('name');
console.log(Symbol.keyFor(nameSymbolGlobal));//name
```

#### [12.3 使用符号作为对象属性](#)
凡是使用字符串或数值作为属性的方法的地方，都可以使用符号！同理支持使用 defineProperty、 defineProperties定义属性。

* Object.getOwnPropertyNames()：返回对象实例的常规属性数组。
* Object.getOwnPropertySymbols()：返回对象实例的符号属性数组。这两个方法的返回值互斥。

```javascript
let nameSymbol = Symbol('name');
let ageSymbol  = Symbol('age');
let sexSymbol = Symbol('sex');

let user =  {
    uid: '2016110418',
    [nameSymbol]: 'remix',
    [ageSymbol]: 25,
    toString: function () {
        return `Name: ${this[nameSymbol]} age: ${this[ageSymbol]} sex: ${this[sexSymbol]?'boy':'girl'}`
    }
}

Object.defineProperty(user, sexSymbol,  {
    value: true,
    configurable: false,
    enumerable: true,
    writable: false
})


let uidSymbol = Symbol('uid');
let gradeSymbol = Symbol('grade');
Object.defineProperties(user, {
    [uidSymbol]: {
        value:'2016110418',
        configurable: false,
        enumerable: true,
        writable: false
    },
    [gradeSymbol]: {
        value:'高三',
        configurable: false,
        enumerable: true,
        writable: false
    }
});


console.log(user.toString());//Name: remix  age: 25
console.log(user[nameSymbol]);//remix
console.log(user[sexSymbol]);//true

for (const property of  Object.getOwnPropertyNames(user)) {
    console.log(property);
}//uid toString


for (const symbol of  Object.getOwnPropertySymbols(user)) {
    console.log(symbol);
}//Symbol(name) Symbol(age) Symbol(sex) Symbol(uid) Symbol(grade)
```

> 注意：对象字面量只能在计算属性语法中使用符号为属性！` `注意使用 [] 来使用属性和定义符号属性！


**常用内置符号** ECMAScript6也引入了一批常用内置符号(well-known symbol) 用于暴露语言内部行为，开发者可以直接访问、重写或模拟这些行为。这些内置符号都已Symbol工厂函数字符串 属性的形式存在！

for-of 循环为在相关对象上使用Symbol.iterator属性。那么就可以通过在自定义对象上重新定义Symbol.iterator的值，来改变 for-of 在迭代该对象时的行为。

**内置符号属性都是不可写、不可枚举、不可配置！**

在提到ECMAScript规范时，经常会引用符号在规范中的名称，前缀为`@@`。例如： `@@iterator` 指的是 Symbol.iterator;

##### [2.1 Symbol.hasInstance](#)
根据ECMAScript规范，这个符号作为一个属性表示“一个方法，该方法决定一个构造器对象是否认可一个对象时它的实例。由instanceof操作符使用”。instanceof操作符可以用来确定一个对象实例的原型链上是否有原型。instanceof的典型使用场景如下：　
```javascript
function Foo(){}
let f = new Foo()
console.log(f instanceof Foo)//true

class Bar{}
let b = new Bar();
console.log(b instanceof Bar)//true
```
在ES6中，instanceof操作符会使用Symbol.hasInstance函数来确定关系。以Symbol.hasInstance为键的函数会执行同样的操作，只是操作数对调了一下：

```javascript
function Foo(){}
let f = new Foo()
console.log(Foo[Symbol.hasInstance](f))//true

class Bar{}
let b = new Bar()
console.log(Bar[Symbol.hasInstance](b))//true
```
这个属性定义在Function的原型上，因此默认在所有函数和类上都可以调用。由于instanceof操作符会在原型链上寻找这个属性定义，就跟在原型链上寻找其他属性一样，因此可以在继承的类上通过静态方法重新定义这个函数：

```javascript
class Bar{}
class Baz extends Bar{
    static [Symbol.hasInstance](){
        return false;
    }
}

let b =  new Baz()
console.log(Bar[Symbol.hasInstance](b))//true
console.log(b instanceof Bar)          //true
console.log(Baz[Symbol.hasInstance](b))//false
console.log(b instanceof Baz)          //false
```

#### [12.3 Symbol.iterator](#)
根据ECMAScript规范，这个符号作为一个属性表示“一个方法，该方法返回对象默认的迭代器。由for-of语句使用”。换句话说，这个符号表示实现迭代器API的函数。

for-of循环这样的语言结构会利用这个函数执行迭代操作。循环时它们会调用以Symbol.iterator为键的函数，并默认这个函数会返回一个实现迭代器API的对象。很多时候，返回对象是实现该API的Generator:

```javascript
class Foo{
    *[Symbol.iterator](){}
}

let f = new Foo()
console.log(f[Symbol.iterator]())
//Genatator{<suspaneded>}
```
技术上，这个由Symbol.iterator函数生成的对象应该通过其next()方法陆续返回值。可以通过显式地调用next()方法返回，也可以隐式地通过生成器函数返回：

```javascript
class Emitter {
    constructor(max) {
        this.max = max;
        this.idx = 0;
    }
    *[Symbol.iterator]() {
        while (this.idx < this.max) {
            yield this.idx++;
        }
    }
}

function count() {
    let emitter = new Emitter(5);
    for(const x of emitter){
        console.log(x);
    }
}
count();
```

#### [12.4 Symbol.asyncIterator](#)
一个方法，该方法返回对象默认的AsyncIterator。由for-await-of语句使用 这个符号表示实现异步迭代器API的函数


#### [12.5 Symbol.split](#)
一个正则表达式方法，该方法在匹配正则表达式的索引位置拆分字符串。由String.prototype.split()方法使用

#### [12.6 Symbol.toPrimitive](#)
一个方法，该方法将对象转换为相应的原始值。由ToPrimitive抽象操作使用

#### [12.7 Symbol.species](#)

#### [12.8 ...其他内置符号请需要百度看](#)
...

### [13. Object 类型](#)
ECMAScript 中的对象其实就是一组数据和功能的集合。对象通过 new 操作符后跟对象类型的名称来创建。开发者可以通过创建 Object 类型的实例来创建自己的对象，然后再给对象添加属性和方法：

```javascript
let o = new Object();
```
这个语法类似 Java，但 ECMAScript 只要求在给构造函数提供参数时使用括号。如果没有参数，如上面的例子所示，那么完全可以省略括号（不推荐）：
```javascript
let o = new Object; // 合法，但不推荐
```
Object 的实例本身并不是很有用，但理解与它相关的概念非常重要。类似 Java 中的 java.lang.Object，ECMAScript 中的 Object 也是派生其他对象的基类。Object 类型的所有属性和方法在派生的对象上同样存在。

**每个 Object 实例都有如下属性和方法。**
* constructor：用于创建当前对象的函数。在前面的例子中，这个属性的值就是 Object()
函数。
* hasOwnProperty(propertyName)：用于判断当前对象实例（不是原型）上是否存在给定的属
性。要检查的属性名必须是字符串（如 o.hasOwnProperty("name")）或符号。
* isPrototypeOf(object)：用于判断当前对象是否为另一个对象的原型。
* propertyIsEnumerable(propertyName)：用于判断给定的属性是否可以使用for-in 语句枚举。与 hasOwnProperty()一样，属性名必须是字符串。
* toLocaleString()：返回对象的字符串表示，该字符串反映对象所在的本地化执行环境。
* toString()：返回对象的字符串表示。
* valueOf()：返回对象对应的字符串、数值或布尔值表示。通常与 toString()的返回值相同。
因为在 ECMAScript 中 Object 是所有对象的基类，所以任何对象都有这些属性和方法。


### [14. 函数](#)
函数对任何语言来说都是核心组件，因为它们可以封装语句，然后在任何地方、任何时间执行。 
ECMAScript 中的函数使用 function 关键字声明，后跟一组参数，然后是函数体。

```javascript
function functionName(arg0, arg1,...,argN) { 
 statements 
} 
function sayHi(name, message) { 
 console.log("Hello " + name + ", " + message); 
} 
```
ECMAScript 中的函数不需要指定是否返回值。任何函数在任何时间都可以使用 return 语句来返回函数的值，用法是后跟要返回的值。比如：
```javascript
function sum(num1, num2) { 
 return num1 + num2; 
}
```
严格模式对函数也有一些限制,如果违反上述规则，则会导致语法错误，代码也不会执行。
* 函数不能以 eval 或 arguments 作为名称；
* 函数的参数不能叫 eval 或 arguments；
* 两个命名参数不能拥有同一个名称。


-----
时间: [2024/9/28 第四次 再次修订](#) 


