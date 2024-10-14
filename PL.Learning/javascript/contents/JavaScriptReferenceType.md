### [Javascript 基本引用类型](#)
>**介绍**：在 ECMAScript 中，引用类型是把数据和功能组织到一起的结构，经常被人错误地称作“类”，此章节认识常见的**内置引用类型**。

---

- [1. Date 日期类型](#1-date-日期类型)
- [2. RegExp 正则表达式](#2-regexp-正则表达式)
- [3. 原始值包装类型](#3-原始值包装类型)
- [4. Boolean](#4-boolean)


----

### [1. Date 日期类型](#)
ECMAScript 的 Date 类型参考了 Java 早期版本中的 **java.util.Date**。为此，Date 类型将日期
保存为自协调世界时（UTC，Universal Time Coordinated）时间 1970 年 1 月 1 日午夜（零时）至今所经过的**毫秒数**。


> 使用这种存储格式，Date 类型可以精确表示 1970 年 1 月 1 日之前及之后 285 616 年的日期, [官方文档：MDN Web Docs - JavaScript Date](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date) 。

创建日期对象,在不给 Date 构造函数传参数的情况下，创建的对象将保存当前日期和时间。
```javascript
let now = new Date(); 
```
要基于其他日期和时间创建日期对象，必须传入其毫秒表示（UNIX 纪元 1970 年 1 月 1 日午夜之后的毫秒数）。
ECMAScript为此提供了三个辅助**静态方法**：
* Date.parse()。 方法接收一个表示日期的字符串参数，尝试将这个字符串转换为表示该日期的毫秒数。
* Date.UTC()。方法接受的参数同 Date 构造函数接受最多参数时一样，但该前者会视它们为 UTC 时间，其返回从 1970 年 1 月 1 日 00:00:00 UTC 到指定时间的毫秒数。
* Date.now()。返回自1970年1月1日00:00:00 (UTC) 到当前时间的毫秒数。

#### [1.1 Date.parse](#)
Date.parse()方法接收一个表示日期的字符串参数，尝试将这个字符串转换为表示该日期的毫秒数。
* “月/日/年”，如"5/23/2019"；
* “月名 日, 年”，如"May 23, 2019"；
* “周几 月名 日 年 时:分:秒 时区”，如"Tue May 23 2019 00:00:00 GMT-0700"；
*  ISO 8601 扩展格式“YYYY-MM-DDTHH:mm:ss.sssZ”，如 2019-05-23T00:00:00（只适用于兼容 ES5 的实现）。

比如，要创建一个表示“2019 年 5 月 23 日”的日期对象，可以使用以下代码：

```javascript
let someDate = new Date(Date.parse("May 23, 2019"));
```
如果传给 Date.parse()的字符串并不表示日期，则该方法会返回 NaN。

#### [1.2 Date.UTC](#)
Date.UTC() 方法接受的参数同 Date 构造函数接受最多参数时一样，但该前者会视它们为 UTC 时间，其
返回从 1970 年 1 月 1 日 00:00:00 UTC 到指定时间的毫秒数。

* Date.UTC(year)
* Date.UTC(year, month)
* Date.UTC(year, month, day)
* Date.UTC(year, month, day, hour)
* Date.UTC(year, month, day, hour, minute)
* Date.UTC(year, month, day, hour, minute, second)
* Date.UTC(year, month, day, hour, minute, second, millisecond)

参数备注：
* **year**：从 0 到 99 的值会被映射到 1900 至 1999 年。其他的值则代表实际的年份。
* **month**：0（一月）到 11（十二月）之间的一个整数，表示月份。
```javascript
const utcDate1 = new Date(Date.UTC(96, 1, 2, 3, 4, 5));
const utcDate2 = new Date(Date.UTC(0, 0, 0, 0, 0, 0));

console.log(utcDate1.toUTCString());
// Expected output: "Fri, 02 Feb 1996 03:04:05 GMT"

console.log(utcDate2.toUTCString());
// Expected output: "Sun, 31 Dec 1899 00:00:00 GMT"
```

#### [1.3 Date.now](#)
Date.now() 方法返回自 1970 年 1 月 1 日 00:00:00 (UTC) 到当前时间的毫秒数。

```javascript
// This example takes 2 seconds to run
const start = Date.now();

console.log('starting timer...');
// Expected output: "starting timer..."

let ticker = setTimeout(() => {
  const millis = Date.now() - start;

  console.log(`seconds elapsed = ${Math.floor(millis / 1000)}`);
  // Expected output: "seconds elapsed = 2"
}, 2000);
```

#### [1.4 继承自Object的方法](#)
Date 类型重写了 toLocaleString()、toString()和 valueOf()方法。

* **toLocaleString()** 返回与浏览器运行的本地环境一致的日期和时间。
* **toString()** 方法通常返回带时区信息的日期和时间，而时间也是以 24 小时制（0~23）表示的。
* **valueOf()** 方法根本就不返回字符串，这个方法被重写后返回的是日期的毫秒表示。

```javascript
let current = new Date(Date.now());

console.log(current.toLocaleString());
console.log(current.toString());
//2024/10/13 23:04:51
//Sun Oct 13 2024 23:04:51 GMT+0800 (中国标准时间)
```

#### [1.5 日期格式化方法](#)
Date 类型有几个专门用于格式化日期的方法，它们都会返回字符串：

* toDateString()显示日期中的周几、月、日、年（格式特定于实现）；
* toTimeString()显示日期中的时、分、秒和时区（格式特定于实现）；
* toLocaleDateString()显示日期中的周几、月、日、年（格式特定于实现和地区）；
* toLocaleTimeString()显示日期中的时、分、秒（格式特定于实现和地区）；
* toUTCString()显示完整的 UTC 日期（格式特定于实现）。

这些方法的输出与 toLocaleString()和 toString()一样，会因浏览器而异。因此不能用于在用户界面上一致地显示日期。

#### [1.6 日期/时间组件方法](#)
Date 类型剩下的方法（见下表）直接涉及取得或设置日期值的特定部分。注意表中“UTC 日期”，指的是没有时区偏移（将日期转换为 GMT）时的日期。

**返回时间**:

|方法|描述|
|:---|:---|
|getFullYear()| 返回 4 位数年（即 2019 而不是 19）|
|getMonth() |返回日期的月（0 表示 1 月，11 表示 12 月）|
|getDate() |返回日期中的日（1~31）|
|getDay() |返回日期中表示周几的数值（0 表示周日，6 表示周六）|
|getTime() |返回日期的毫秒表示；与 valueOf()相同|
|getHours() |返回日期中的时（0~23）|
|getMinutes() |返回日期中的分（0~59）|
|getSeconds() |返回日期中的秒（0~59）|
|getMilliseconds()| 返回日期中的毫秒|

**返回UTC时间**:

|方法|描述|
|:---|:---|
|getUTCDay() 返回 UTC 日期中表示周几的数值（0 表示周日，6 表示周六）|
|getUTCDate() 返回 UTC 日期中的日（1~31）|
|getUTCHours() 返回 UTC 日期中的时（0~23）|
|getUTCMonth() 返回 UTC 日期的月（0 表示 1 月，11 表示 12 月）|
|getUTCFullYear() 返回 UTC 日期的 4 位数年|
|getUTCMinutes() 返回 UTC 日期中的分（0~59）|
|getUTCSeconds() 返回 UTC 日期中的秒（0~59）|
|getUTCMilliseconds() 返回 UTC 日期中的毫秒|


**设置时间**:

|方法|描述|
|:---|:---|
|setDate(date) |设置日期中的日（如果 date 大于该月天数，则加月）|
|setMonth(month)| 设置日期的月（month 为大于 0 的数值，大于 11 加年）|
|setHours(hours) |设置日期中的时（如果 hours 大于 23，则加日）|
|setTime(milliseconds)| 设置日期的毫秒表示，从而修改整个日期|
|setMinutes(minutes) |设置日期中的分（如果 minutes 大于 59，则加时）|
|setSeconds(seconds) |设置日期中的秒（如果 seconds 大于 59，则加分）|
|setMilliseconds(milliseconds) |设置日期中的毫秒|
|setFullYear(year) |设置日期的年（year 必须是 4 位数）|

**设置UTC时间**:

|方法|描述|
|:---|:---|
|setUTCFullYear(year)| 设置 UTC 日期的年（year 必须是 4 位数）|
|setUTCMonth(month) |设置 UTC 日期的月（month 为大于 0 的数值，大于 11 加年）|
|setUTCDate(date) |设置 UTC 日期中的日（如果 date 大于该月天数，则加月）|
|setUTCMinutes(minutes)| 设置 UTC 日期中的分（如果 minutes 大于 59，则加时）|
|setUTCHours(hours) |设置 UTC 日期中的时（如果 hours 大于 23，则加日）|
|setUTCSeconds(seconds)| 设置 UTC 日期中的秒（如果 seconds 大于 59，则加分）|
|setUTCMilliseconds(milliseconds) |设置 UTC 日期中的毫秒|
|getTimezoneOffset()| 返回以分钟计的 UTC 与本地时区的偏移量（如美国 EST 即“东部标准时间” 返回 300，进入夏令时的地区可能有所差异）|

#### [1.7 其他方法](#)
**Object.prototype.constructor** 指向创建该实例对象的构造函数

#### [1.8 实例方法](#)

* **Function.prototype.apply()**  apply() 方法会以给定的 this 值和作为数组（或类数组对象）提供的 arguments 调用该函数。
* **Function.prototype.bind()**
* **Function.prototype.call()**
* **Function.prototype** `[Symbol.hasInstance]()`
* **Function.prototype.toString()**

### [2. RegExp 正则表达式](#)
ECMAScript 通过 RegExp 类型支持正则表达式，正则表达式使用类似 Perl 的简洁语法来创建, [JavaScript 菜鸟教程、正则表达式](https://www.runoob.com/regexp/regexp-tutorial.html)。

```
let expression = /pattern/flags; 
```
这个正则表达式的 pattern（模式）可以是任何简单或复杂的正则表达式，包括字符类、限定符、
分组、向前查找和反向引用。每个正则表达式可以带零个或多个 flags（标记），用于控制正则表达式
的行为。下面给出了表示匹配模式的标记。
* **g**：全局模式，表示查找字符串的全部内容，而不是找到第一个匹配的内容就结束。
* **i**：不区分大小写，表示在查找匹配时忽略 pattern 和字符串的大小写。
* **m**：多行模式，表示查找到一行文本末尾时会继续查找。
* **y**：执行“粘性（sticky）”搜索，从目标字符串的当前位置开始匹配。。
* **s**：dotAll 模式，表示元字符.匹配任何字符（包括`\n` 或`\r`）。
* **v**	升级 u 模式，提供更多 Unicode 码特性。
* **u**	“Unicode”；将模式视为 Unicode 码位序列。	unicode
* **v**	升级 u 模式，提供更多 Unicode 码特性。	unicodeSets

```javascript
// 匹配字符串中的所有"at"
let pattern1 = /at/g;
// 匹配第一个"bat"或"cat"，忽略大小写
let pattern2 = /[bc]at/i;
// 匹配所有以"at"结尾的三字符组合，忽略大小写
let pattern3 = /.at/gi; 
```
与其他语言中的正则表达式类似，所有元字符在模式中也必须转义，包括： `( [ { \ ^ $ | ) ] } ? * + . `。

元字符在正则表达式中都有一种或多种特殊功能，所以要匹配上面这些字符本身，就必须使用反斜杠来转义。下面是几个例子：
```javascript
// 匹配第一个"bat"或"cat"，忽略大小写
let pattern1 = /[bc]at/i;
// 匹配第一个"[bc]at"，忽略大小写
let pattern2 = /\[bc\]at/i;
// 匹配所有以"at"结尾的三字符组合，忽略大小写
let pattern3 = /.at/gi;
// 匹配所有".at"，忽略大小写
let pattern4 = /\.at/gi; 
```
这里的 pattern1 匹配"bat"或"cat"，不区分大小写。

#### [2.1 构造函数](#)
正则表达式也可以使用 RegExp 构造函数来创建，它接收两个参数：模式字符串和（可选的）标记字符串。 任何
使用字面量定义的正则表达式也可以通过构造函数来创建，比如：
```javascript
// 匹配第一个"bat"或"cat"，忽略大小写
let pattern1 = /[bc]at/i;
// 跟 pattern1 一样，只不过是用构造函数创建的
let pattern2 = new RegExp("[bc]at", "i"); 
```

#### [2.2 实例属性](#)
每个 RegExp 实例都有下列属性，提供有关模式的各方面信息。
* global：布尔值，表示是否设置了 g 标记。
* ignoreCase：布尔值，表示是否设置了 i 标记。
* unicode：布尔值，表示是否设置了 u 标记。
* sticky：布尔值，表示是否设置了 y 标记。
* lastIndex：整数，表示在源字符串中下一次搜索的开始位置，始终从 0 开始。
* multiline：布尔值，表示是否设置了 m 标记。
* dotAll：布尔值，表示是否设置了 s 标记。
* source：正则表达式的字面量字符串（不是传给构造函数的模式字符串），没有开头和结尾的斜杠。
* flags：正则表达式的标记字符串。始终以字面量而非传入构造函数的字符串模式形式返回（没有前后斜杠）。

通过这些属性可以全面了解正则表达式的信息，不过实际开发中用得并不多，因为模式声明中包含这些信息。下面是一个例子：
```javascript
let pattern1 = /\[bc\]at/i;
console.log(pattern1.global); // false
console.log(pattern1.ignoreCase); // true
console.log(pattern1.multiline); // false
console.log(pattern1.lastIndex); // 0
console.log(pattern1.source); // "\[bc\]at"
console.log(pattern1.flags); // "i"

let pattern2 = new RegExp("\\[bc\\]at", "i");

console.log(pattern2.global); // false
console.log(pattern2.ignoreCase); // true
console.log(pattern2.multiline); // false
console.log(pattern2.lastIndex); // 0
console.log(pattern2.source); // "\[bc\]at"
console.log(pattern2.flags); // "i"
```
注意，虽然第一个模式是通过字面量创建的，第二个模式是通过 RegExp 构造函数创建的，但两个模式的source和
flags 属性是相同的。source 和 flags 属性返回的是规范化之后可以在字面量中使用的形式。

#### [2.3 exec](#)
RegExp 实例的主要方法是 exec()，主要用于配合捕获组使用。

这个方法只接收一个参数，即要应用模式的字符串。如果找到了匹配项，则返回包含第一个匹配信息的数组；如果没找到匹配项，则返回null。

返回的数组虽然是 Array 的实例，但包含两个额外的属性：
* **index** 是字符串中匹配模式的起始位置。 
* **input** 是要查找的字符串。

这个数组的第一个元素是匹配整个模式的字符串，其他元素是与表达式中的捕获组匹配的字符串。如果模式中没有捕获组，则数组只包含一个元素。

```javascript
let text = "mom and dad and baby";
let pattern = /mom( and dad( and baby)?)?/gi;
let matches = pattern.exec(text);

console.log(matches.index); // 0
console.log(matches.input); // "mom and dad and baby"
console.log(matches[0]); // "mom and dad and baby"
console.log(matches[1]); // " and dad and baby"
console.log(matches[2]); // " and baby"
```
如果模式设置了全局标记，则每次调用 exec()方法会返回一个匹配的信息。如果没有设置全局标
记，则无论对同一个字符串调用多少次 exec()，也只会返回第一个匹配的信息。

```javascript
let text = "cat, bat, sat, fat";
let pattern = /.at/;

let matches = pattern.exec(text);
console.log(matches.index); // 0
console.log(matches[0]); // cat
console.log(pattern.lastIndex); // 0

matches = pattern.exec(text);
console.log(matches.index); // 0
console.log(matches[0]); // cat 
console.log(pattern.lastIndex); // 0 
```
如果在这个模式上设置了 **g** 标记，则每次调用 exec()都会在字符串中向前搜索下一个匹配项，如下面的例子所示：
```javascript
let text = "cat, bat, sat, fat";
let pattern = /.at/g;

let matches = pattern.exec(text);
console.log(matches.index); // 0
console.log(matches[0]); // cat
console.log(pattern.lastIndex); // 3

matches = pattern.exec(text);
console.log(matches.index); // 5
console.log(matches[0]); // bat
console.log(pattern.lastIndex); // 8

matches = pattern.exec(text);
console.log(matches.index); // 10
console.log(matches[0]); // sat 
```
如果模式设置了粘附标记 y，则每次调用 exec()就只会在 lastIndex 的位置上寻找匹配项。粘附标记覆盖全局标记。
```javascript
let text = "cat, bat, sat, fat";
let pattern = /.at/y;

let matches = pattern.exec(text);
console.log(matches.index); // 0
console.log(matches[0]); // cat
console.log(pattern.lastIndex); // 3

// 以索引 3 对应的字符开头找不到匹配项，因此 exec()返回 null
// exec()没找到匹配项，于是将 lastIndex 设置为 0
matches = pattern.exec(text);
console.log(matches); // null
console.log(pattern.lastIndex); // 0

// 向前设置 lastIndex 可以让粘附的模式通过 exec()找到下一个匹配项：
pattern.lastIndex = 5;
matches = pattern.exec(text);
console.log(matches.index); // 5
console.log(matches[0]); // ba
console.log(pattern.lastIndex); // 8 
```

#### [2.4 test](#)
接收一个字符串参数。如果输入的文本与模式匹配，则参数返回 true，否则返回 false。
这个方法适用于只想测试模式是否匹配，而不需要实际匹配内容的情况。 

**test()** 经常用在 if 语句中：
```javascript
let text = "000-00-0000";
let pattern = /\d{3}-\d{2}-\d{4}/;

if (pattern.test(text)) {
 console.log("The pattern was matched.");
} 
```
无论正则表达式是怎么创建的，继承的方法 **toLocaleString()** 和 **toString()** 都返回正则表达式的字面量表示。比如：
```javascript
let pattern = new RegExp("\\[bc\\]at", "gi");

console.log(pattern.toString()); // /\[bc\]at/gi
console.log(pattern.toLocaleString()); // /\[bc\]at/gi 
```

正则表达式的 **valueOf**()方法返回正则表达式本身。

#### [2.5 构造函数属性](#)
RegExp 构造函数本身也有几个属性。换句话说，每个属性都有一个全名和一个简写。

|全 名|简 写| 说 明                            |
|:---|:---|:-------------------------------|
|input |`$_`| 最后搜索的字符串（非标准特性）                |
|lastMatch |`$&`| 最后匹配的文本|
|lastParen |`$+`| 最后匹配的捕获组（非标准特性）|
|leftContext |`$`| input 字符串中出现在 lastMatch 前面的文本|
|rightContext |`$'`|  input 字符串中出现在 lastMatch 后面的文本 |

通过这些属性可以提取出与 exec()和 test()执行的操作相关的信息。来看下面的例子：
```javascript
let text = "this has been a short summer";
let pattern = /(.)hort/g;

if (pattern.test(text)) {
    console.log(RegExp.input); // this has been a short summer
    console.log(RegExp.leftContext); // this has been a
    console.log(RegExp.rightContext); // summer
    console.log(RegExp.lastMatch); // short
    console.log(RegExp.lastParen); // s
} 
```
以上代码创建了一个模式，用于搜索任何后跟"hort"的字符，并把第一个字符放在了捕获组中。
不同属性包含的内容如下。
* **input** 属性中包含原始的字符串。
* **leftConext** 属性包含原始字符串中"short"之前的内容，rightContext 属性包含"short" 之后的内容。
* **lastMatch** 属性包含匹配整个正则表达式的上一个字符串，即"short"。
* **lastParen** 属性包含捕获组的上一次匹配，即"s"。

这些属性名也可以替换成简写形式，只不过要使用中括号语法来访问，如下面的例子所示，因为大
多数简写形式都不是合法的 ECMAScript 标识符：
```javascript
let text = "this has been a short summer";
let pattern = /(.)hort/g;
/*
* 注意：Opera 不支持简写属性名
* IE 不支持多行匹配
  */
if (pattern.test(text)) {
  console.log(RegExp.$_); // this has been a short summer
  console.log(RegExp["$`"]); // this has been a
  console.log(RegExp["$'"]); // summer
  console.log(RegExp["$&"]); // short
  console.log(RegExp["$+"]); // s
} 
```

#### [2.6 模式局限](#)
虽然 ECMAScript 对正则表达式的支持有了长足的进步，但仍然缺少 Perl 语言中的一些高级特性。
下列特性目前还没有得到 ECMAScript 的支持（想要了解更多信息，可以参考 `Regular-Expressions.info` 网站）：

* `\A` 和 `\Z` 锚（分别匹配字符串的开始和末尾）
* 联合及交叉类
* 原子组
* x（忽略空格）匹配模式
* 条件式匹配
* 正则表达式注释

虽然还有这些局限，但 ECMAScript 的正则表达式已经非常强大，可以用于大多数模式匹配任务。


### [3. 原始值包装类型](#)
为了方便操作原始值，ECMAScript 提供了 3 种特殊的引用类型：Boolean、Number 和 String。

```javascript
let s1 = "some text";
let s2 = s1.substring(2);

let s1 = "some text";
s1.color = "red";
console.log(s1.color); // undefined 

let obj = new Object("some text");
console.log(obj instanceof String); // true

let value = "25";
let number = Number(value); // 转型函数
console.log(typeof number); // "number"
let obj = new Number(value); // 构造函数
console.log(typeof obj); // "object"
```

### [4. Boolean](#)
Boolean 是对应布尔值的引用类型。要创建一个 Boolean 对象，就使用 Boolean 构造函数并传入
true 或 false，如下例所示：

```javascript
let booleanObject = new Boolean(true); 
```
Boolean 的实例会重写 valueOf 和 toString 方法。
* valueOf()方法，返回一个原始值 true 或 false。
* toString()方法被调用时也会被覆盖，返回字符串"true"或"false"。

```javascript
let falseObject = new Boolean(false);
let result = falseObject && true;

console.log(result); // true
let falseValue = false;
result = falseValue && true;

console.log(result); // false 
```
**typeof 操作符对原始值返回"boolean"，但对引用值返回"object"**。
```javascript
console.log(typeof falseObject); // object
console.log(typeof falseValue); // boolean
console.log(falseObject instanceof Boolean); // true
console.log(falseValue instanceof Boolean); // false 
```
理解原始布尔值和 Boolean 对象之间的区别非常重要，强烈建议永远不要使用后者。

### [5. Number](#)
Number 是对应数值的引用类型。要创建一个 Number 对象，就使用 Number 构造函数并传入一个数值，如下例所示：
```javascript
let numberObject = new Number(10); 
```
与 Boolean 类型一样，Number 类型重写了 valueOf()、toLocaleString()和 toString()方法。
* toString() 方法可选地接收一个表示基数的参数，并返回相应基数形式的数值字符串
```javascript
let num = 10;

console.log(num.toString()); // "10"
console.log(num.toString(2)); // "1010"
console.log(num.toString(8)); // "12"
console.log(num.toString(10)); // "10"
console.log(num.toString(16)); // "a" 
```

#### [5.1 静态方法](#)

| 方法                      | 介绍                                                                       |
|:------------------------|:-------------------------------------------------------------------------|
| isFinite(value)              | 判断传入值是否是一个有限数——也就是说，它检查给定值是一个数字，且该数字既不是正的 Infinity，也不是负的 Infinity 或 NaN。 
| isInteger(value)             |判断传入值是否为整数。|
| isNaN(value)                 |判断传入的值是否为 NaN，如果输入不是数字类型，则返回 false。它是全局 isNaN() 函数更健壮的版本。|
| isSafeInteger(testValue)         |判断提供的值是否是一个安全整数。|
| parseFloat(string)            |解析参数并返回浮点数。如果无法从参数中解析出一个数字，则返回 NaN。|
| parseInt(string， radix) |解析一个字符串参数并返回一个指定基数的整数。|


```javascript
Number.isFinite(Infinity); // false
Number.isFinite(NaN); // false
Number.isFinite(-Infinity); // false

Number.isFinite(0); // true
Number.isFinite(2e64); // true
```

#### [5.2 静态属性](#)

| 方法                      | 介绍                                |
|:------------------------|:----------------------------------|
|Number.EPSILON| 表示 1 与大于 1 的最小浮点数之间的差值            |
|Number.MAX_SAFE_INTEGER| 表示在 JavaScript 中最大的安全整数（253 – 1）。 |
|Number.MAX_VALUE| 表示在 JavaScript 中可表示的最大数值。         |
|Number.MIN_SAFE_INTEGER| 代表在 JavaScript 中最小的安全整数（-253 – 1） |
|Number.MIN_VALUE| 表示在 JavaScript 中可表示的最小正数值。        |
|Number.NaN| 表示非数字值，等同于 NaN。                   |
|Number.NEGATIVE_INFINITY|  表示负无穷值。  |
|Number.POSITIVE_INFINITY|      表示正无穷大值。         |

```javascript
function checkNumber(bigNumber) {
  if (bigNumber === Number.POSITIVE_INFINITY) {
    return 'Process number as Infinity';
  }
  return bigNumber;
}

console.log(checkNumber(Number.MAX_VALUE));
// Expected output: 1.7976931348623157e+308

console.log(checkNumber(Number.MAX_VALUE * 2));
// Expected output: "Process number as Infinity"
```

#### [5.3 实例方法](#)

| 方法 | 介绍 |
|:---|:---|
|Number.prototype.toExponential()|返回一个以指数表示法表示该数字的字符串。|
|Number.prototype.toFixed()|使用定点表示法来格式化该数值。|
|Number.prototype.toLocaleString()|回这个数字特定于语言环境的表示字符串。 |
|Number.prototype.toPrecision(precision)| 方法返回一个以指定精度表示该数字的字符串。|
|Number.prototype.toString()| 返回表示该数字值的字符串。 |
|Number.prototype.valueOf()| 返回该数字的值。|

**toExponential**
```javascript
function expo(x, f) {
  return Number.parseFloat(x).toExponential(f);
}

console.log(expo(123456, 2));
// Expected output: "1.23e+5"

console.log(expo('123456'));
// Expected output: "1.23456e+5"

console.log(expo('oink'));
// Expected output: "NaN"
```

**toFixed**
```javascript
function financial(x) {
  return Number.parseFloat(x).toFixed(2);
}

console.log(financial(123.456));
// Expected output: "123.46"

console.log(financial(0.004));
// Expected output: "0.00"

console.log(financial('1.23e+5'));
// Expected output: "123000.00"
```