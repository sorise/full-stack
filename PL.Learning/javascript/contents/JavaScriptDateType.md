### [Javasript 基础](#)
> **介绍**： 语法基础，看看就行！

-----
- [1. Javasript语法基础](#1-javasript语法基础)
- [2. 变量声明](#2-变量声明)
- [3. javasript原生数据类型](#3-javasript原生数据类型)
- [4. 模板字符串](#4-模板字符串)
- [5. math](#5-math)
- [5. bigInt 对象](#5-bigint-对象)

-----

### [1. Javasript语法基础](#)
**标识符**: 英文大小写字母、数字、下划线（ _ ）和美元符号（ $ ） (可以使用汉字或其他合法字符命名，但是不推荐)。
1. 区分大小写。
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
* 在非严格模式下可以不声明直接使用对象等非严格操作。
* 在严格模式下JavasScript3中不确定的行为将为得到处理，不安全的操作将会抛出错误。

#### [1.1 语句](#)
ECMAScript 中的语句以分号结尾。省略分号意味着由解析器确定语句在哪里结尾：
```javascript
let sum = list.length + limit;
let gap = liner - cicer  //有解析器判断语句结尾
```
推荐一定要写分号结尾！有助于提高性能！

#### [1.2 保留关键字](#)

```javascript
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

#### [1.3 内置对象](#)
您也应该避免使用 JavaScript 内置的对象、属性和方法的名称作为 Javascript 的变量或函数名：

```javascript
Array       Date        eval        function        hasOwnProperty
Infinity    isFinite    isNaN       isPrototypeOf   length
Math        NaN	        name	    Number          Object
prototype   String      toString    undefined       valueOf
```


### [2. 变量声明](#)
ECMAScript 变量是松散类型的，意思是变量可以用于保存任何类型的数据。每个变量只不过是一个用于保存任意值的命名占位符。 可以使用三个关键字声明变量  var const let

ECS 包含两种不同的数据类型：
* 基本类型值:简单的数据段  Undefind Null Boolean Number String Symbol 这几种类型都是按值访问
* 引用类型值:可以由多个值构成的对象 保存在内存中 按引用访问,实际操作是引用

#### [2.1 var 关键字](#)
使用var 来声明非常简单啊，但是var也有一些缺点，已经不再推荐使用！
- 变量提升
- 非块级作用域，函数级作用域，使用var操作符定义的变量会成为包含它的函数的局部变量。
- 可重复声明
- 不同类型赋值

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

**for-var 的问题**
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
* 块级作用域，不可重复声明，不会再作用域被提升[暂时性死区]！
* 在全局声明 使用 let 中声明的变量不会成为window对象的属性。
* 不支持条件声明！

```javascript
try{
    console.log(age);
    let age = 25;
}catch (e) {
    console.error(Error Name: ${e.name} );//ReferenceError
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

const声明的变量是引用类型，那么可以改变引用类型里面的值，但是不能改变引用：
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
1. Javasript 有如下几种简单数据类型 Undefined Null  Boolean String  Number  Object Function  Symbol 可以使用 typeof 判断类型
2. 没必要将一个变量初始化为 Undefined 因为它是自动的
3. undefined 派生自null  所以两个相等 未经初始化的变量自动是 undefined null表示一个空对象引用 typeof 返回类型为 object。 function本质上也是 object 但是 typeof返回function
4. 可以使用 typeof 运算符来粗略的确定数据类型

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

#### [3.1 Undefined](#)
对于尚未初始化的变量,会被JS引擎会自动初始化为 Undefined
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
转换为 Boolean 值返回为 false

#### [3.2 Null 类型](#)
Null 类型同样只有一个值即  null。 空对象指针(引用)

```javascript
let name = null;

if ( name ){
    console.log('这里不会执行');
}

if (name === null){
    console.log('name 是 null');
}
```

转换为 Boolean 值返回为 false

#### [3.3 Boolean 类型](#)
只有两个字面值 true false, true 不是 1, false 不是 0。

* Boolean 对象是 Boolean 原始类型的引用类型。要创建 Boolean 对象，只需要传递 Boolean 值作为参数：
* Boolean 对象将覆盖 Object 对象的 ValueOf() 方法，返回原始值，即 true 和 false。ToString() 方法也会被覆盖，返回字符串 "true" 或 "false"。
* 遗憾的是，在 ECMAScript 中很少使用 Boolean 对象，即使使用，也不易理解。
* 平时值类型的布尔类型的实际

```javascript
let found = true;
let lost = false;
```

|数据类型|转换为true|转换为false
|--|:--:|--:
|Boolean|true|false
|String|任何非空字符|空字符
|Number|任何非零数字值|0和NaN
|Object|任何对象|null
|Undefined|undefined 只能转换为 false|undefined

可以使用Boolean(obj) 将一个值转换为Boolean值

平时值类型的布尔类型的实际
```javascript
let val = true;
console.log(val.toString());
```
val 明明是值类型 为何还能够调用方法呢？ 类似于引用类型
```javascript
// 实际做法 做了包装 在不调用方法的时候就是值类型 将要调用方法的时候将其转换为引用类型 
// 然后调用结束后 再转换会值类型  有一个包装的过程
let val = new Boolean(true);
console.log(val.toString());
```
#### [3.4 Number 对象](#)
Number类型采用IEEE 754格式表示整数和浮点值 正如你可能想到的，Number 对象是 Number 原始类型的引用类型。要创建 Number 对象，采用下列代码：

```javascript
//引用类型
let oNumberObject = new Number(68);
//要得到数字对象的 Number 原始值，只需要使用 valueOf() 方法
let iNumber = oNumberObject.valueOf();
//值类型
let age = 78;
```

Number的属性： MAX_VALUE  MIN_VALUE  NEGATIVE_INFINITY  POSITIVE_INFINITY

```javascript
let max = Number.MAX_VALUE //获得最大值
let min = Number.MIN_VALUE //获得最小值
let negative_infinity = Number.NEGATIVE_INFINITY; // 正无穷
let positive_infinity = Number.POSITIVE_INFINITY; // 负无穷

console.log(max) //1.7976931348623157e+308
console.log(min) //5e-324
console.log(isFinite(max)) //true
```

#### [3.5 Number类型的方法](#)
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

#### [3.6 进制表示](#)
最基本的数值字面量格式是十进制整数 八进制 使用前缀 0 十六进制 0x
```javascript
let kicker = 54;
let umix = 070 ; //56 八进制
let remix = 0xlf ; //31 十六进制
```

#### [3.7 Number 数值转换](#)
Number 数值转换 将其他值转换为数值类型:只有不为空的且非全数字的字符串 和 undefined 会转换成NaN

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
var num1 = parseInt("6546sdf"， 10);// 按照十进制解析
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

#### [3.8 浮点值](#)
浮点数的小问题， 浮点数采用 IEEE 754数值标准。

```javascript
let float_number_one = 1. ; //. 后面有问题 当作整数 1 处理
let float_number_two = 10.0 ; //小数点后面是 0 当作整数处理

let score =  39.0;
console.log(score);//39
```

#### [3.9 String 类型](#)
String 字符串数据类型表示 0-N 个 16 Unicode 字符串序列！ ES6 加强了对 Unicode 的支持，允许采用\uxxxx形式表示一个字符，其中xxxx表示字符的 Unicode 码点。
String 官方文档解释方法属性 String 是一个基本包装类型,它是唯一的,一旦创建不能改变,每次改变实际上都是创建一个新的字符串 可以使用单引号 也可以使用双引号表示

```javascript
let userName = 'remix';
let message = 'this is a message';

let sentence = ${userName} said: "${message}"!;

console.log(sentence); //remix said: "this is a message"!
```

#### [3.10 String 转换](#)
有两种方法将一个其他类型转换为字符串

1. toString(n) 方法 n 表示 蒋数字转换为几进制 默认值 10
2. String(带转换变量) undefined, null 没有 toString方法！

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
#### [3.11 String ES6 改造](#)
JS5的字符串是基于16位的UTF-16编码进行构建的,每十六位表示一个编程单元,代表一个字符,许多字符串方法和属性都是基于此编程单元的的,但是过去16足够了但是 Unicode引入扩展字符集,
编码规则不得不进行变更,所有不再限制在16位,扩展到了32位,一个字符对应一个码位例如 55362 表示 吉这个字符.UTF-16中,前面216码位 均以16位的编码单位表示,这个范围被称为基于多文
中平面(BMP)超过这个范围的码位则要归属于某个辅助平面,其中码位仅用16位就无法表示了,为此UTF-16引入 代理对 其规定用两个16位编码单位表示一个码位,也就是说,字符串里的字符有两种,
一种是16位编码单位的BMP字符,一种是两个编码单位的32位字符串,为此产生了很多的问题

1.采用了代理对的一个字符串长度为2,例如 let val="𠮷"; console.log(val.length); 结果长度为2

2.不利于对字符串进行排序和比较,要使用normalize() 方法进行等效关系标准化.

#### [3.12 针对代理对编码提供了码点方法](#)
codePointAt() 方法, 能够正确处理 4(32位) 个字节储存的字符，返回一个字符的码点。 charCodeAt() 方法只能返回16位的码点,无法兼容到代理对  
ES5 提供String.fromCharCode方法，用于从码点返回对应字符，但是这个方法不能识别 32 位的 UTF-16 字符（Unicode 编号大于0xFFFF）。  
ES6 提供了String.fromCodePoint方法，可以识别大于0xFFFF的字符，弥补了String.fromCharCode方法的不足。在作用上，正好与codePointAt方法相反。

```javascript
let g="𠮷";
console.log("[𠮷] 长度为:"+g.length);    
console.log(g.charCodeAt(0));
console.log(g.codePointAt(0));//Unicode 代理对
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

#### [3.13 标准化 normalize](#)
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

#### [3.14 字符串遍历器](#)
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

#### [3.15 直接输入](#)
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
### [4. 模板字符串](#)
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
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
);
```
模板字符串（template string）是增强版的字符串，用反引号（&#180;）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。
```javascript
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
let greeting = \Yo\ World!;
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

${x} + ${y} = ${x + y}
// "1 + 2 = 3"

${x} + ${y * 2} = ${x + y * 2}
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

foo ${fn()} bar
// foo Hello World bar
```
如果大括号中的值不是字符串，将按照一般的规则转为字符串。比如，大括号中是一个对象，将默认调用对象的toString方法。

如果模板字符串中的变量没有声明，将报错。
```javascript
// 变量place没有声明
let msg = Hello, ${place};
// 报错
```
由于模板字符串的大括号内部，就是执行 JavaScript 代码，因此如果大括号内部是一个字符串，将会原样输出。
```javascript
Hello ${'World'}
// "Hello World"
```
模板字符串甚至还能嵌套。

```javascript
const tmpl = addrs => 
  <table>
  ${addrs.map(addr => 
    <tr><td>${addr.first}</td></tr>
    <tr><td>${addr.last}</td></tr>
  ).join('')}
  </table>
;
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

#### [4.1 标签模板](#)
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

#### [4.2 ES6 扩展的String 方法](#)
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

#### [4.3 String.raw()](#)
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
### [5. Math](#)
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

#### [5. 1 Math 对象的扩展](#)
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

### [6. BigInt 对象](#)
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


-----
时间: [2022/8/14 第三次 再次修订] 


