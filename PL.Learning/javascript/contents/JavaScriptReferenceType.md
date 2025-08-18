### [JavaScript 基本引用类型](#)
>**介绍**：在 ECMAScript 中，引用类型是把数据和功能组织到一起的结构，经常被人错误地称作“类”，此章节认识常见的**内置引用类型**。

---

- [1. Date 日期类型](#1-date-日期类型)
- [2. RegExp 正则表达式](#2-regexp-正则表达式)
- [3. 原始值包装类型](#3-原始值包装类型)
- [4. Boolean](#4-boolean)
- [5. Number](#5-number)
- [6. String](#6-string)
- [7. 单例内置对象](#7-单例内置对象)
- [8. Global](#8-global)
- [9. Math](#9-math)


----

### [1. Date 日期类型](#)
ECMAScript 的 Date 类型参考了 Java 早期版本中的 **java.util.Date**。为此，Date 类型将日期
保存为自协调世界时（UTC，Universal Time Coordinated）时间1970年1月1日午夜（零时）至今所经过的**毫秒数**。

> 使用这种存储格式，Date 类型可以精确表示1970年1月1日之前及之后285616年的日期, [官方文档：MDN Web Docs - JavaScript Date](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date) 。

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

> UTC时间，全称是“协调世界时”（Coordinated Universal Time），是世界上最主要的标准时间系统之一。 它基于原子钟，并且被广泛应用于全球各个领域，包括科学研究、商业交易以及互联网通信等。 UTC时间的主要特点是其稳定性和全球统一性。 它不考虑地球上的时区变化，因此为全球各地提供了一个统一的时间参考。 

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
**正则表达式是一个字符序列，用于在文本匹配/搜索的字符串中匹配字符组合**。 ECMAScript 通过 [RegExp](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp) 类型支持正则表达式，正则表达式使用类似 Perl
的简洁语法来创建, [JavaScript 菜鸟教程、正则表达式](https://www.runoob.com/regexp/regexp-tutorial.html)。

```
let expression = /pattern/flags; 
```
这个正则表达式的 pattern（模式）可以是任何简单或复杂的正则表达式，包括字符类、限定符、
分组、向前查找和反向引用。每个正则表达式可以带零个或多个 flags（标记），用于控制正则表达式
的行为。下面给出了表示匹配模式的标记。
* **g**：全局模式，表示查找字符串的全部内容，而不是找到第一个匹配的内容就结束。
* **i**：不区分大小写，表示在查找匹配时忽略 pattern 和字符串的大小写。
* **m**：多行模式，表示查找到一行文本末尾时会继续查找。
* **y**：执行“粘性（sticky）”搜索，从目标字符串的当前位置开始匹配。`<下面解释>`
* **s**：dotAll 模式，表示元字符 `.` 匹配任何字符（包括`\n` 或`\r`）。
* **u**	“Unicode”；将模式视为 Unicode 码位序列。	unicode，**非常有用，建议打开！**
* **d**：表示匹配的结果应该包含每个捕获组开头和末尾的索引。

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
* global：布尔值，表示是否设置了 `g` 标记。
* ignoreCase：布尔值，表示是否设置了 `i` 标记。
* unicode：布尔值，表示是否设置了 `u` 标记。
* sticky：布尔值，表示是否设置了 `y` 标记。
* lastIndex：整数，表示在源字符串中下一次搜索的开始位置，始终从 0 开始。
* multiline：布尔值，表示是否设置了 `m` 标记。
* dotAll：布尔值，表示是否设置了 `s` 标记。
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
RegExp 实例的主要方法是 exec()，主要用于配合捕获组使用，返回一个结果数组或 null。

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

#### [2.3 exec续 返回值](#)
exec() 方法在一个指定字符串中执行一个搜索匹配。返回一个结果数组或 null。

**返回值**
* 如果匹配失败，exec() 方法返回 null，并将正则表达式的 lastIndex 重置为 0。
* 如果匹配成功，exec() 方法返回一个数组，并更新正则表达式对象的 lastIndex 属性。完全匹配成功的文本将作为返回数组的第一项，从第二项起，后续每项都对应一个匹配的捕获组。数组还具有以下额外的属性：

* `index`: 匹配到的字符位于原始字符串的基于 0 的索引值。
* `input`: 匹配的原始字符串。
* `groups` : 一个命名捕获组对象，其键是名称，值是捕获组。若没有定义命名捕获组，则 groups 的值为 undefined。参阅捕获组以了解更多信息。
* `indices` `可选` : 此属性仅在设置了 d 标志位时存在。它是一个数组，其中每一个元素表示一个子字符串的边界。每个子字符串匹配本身就是一个数组，其中第一个元素表示起始索引，第二个元素表示结束索引。

```javascript
// Match "quick brown" followed by "jumps", ignoring characters in between
// Remember "brown" and "jumps"
// Ignore case
const re = /quick\s(?<color>brown).+?(jumps)/dgi;
const result = re.exec("The Quick Brown Fox Jumps Over The Lazy Dog");
```
下表列出这个脚本的返回值（result）：

|属性	|值|
|:---|:---|
|[0]	|"Quick Brown Fox Jumps"|
|[1]	|"Brown"|
|[2]	|"Jumps"|
|index|	4|
|indices|	`[[4, 25], [10, 15], [20, 25]] groups: { color: [10, 15 ]}` |
|input	|"The Quick Brown Fox Jumps Over The Lazy Dog"|
|groups|	`{ color: "brown" }` |

另外，由于正则表达式是全局的（global），`re.lastIndex` 会被设置为 25。
#### [2.4 test](#)
接收一个字符串参数。如果输入的文本与模式匹配，**则参数返回 true，否则返回 false**。这个方法适用于只想测试模式是否匹配，而不需要实际匹配内容的情况。 

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

#### [2.5 静态属性](#)
**RegExp** 构造函数本身也有几个属性。换句话说，每个属性都有一个全名和一个简写。

|全 名|简 写| 说 明                            |
|:---|:---|:-------------------------------|
|input |`$_`| 最后搜索的字符串（非标准特性）       `已废弃`         |
|lastMatch |`$&`| 最后匹配的文本 `已废弃` |
|lastParen |`$+`| 最后匹配的捕获组（非标准特性） `已废弃` |
|leftContext |`$`| input 字符串中出现在 lastMatch 前面的文本 `已废弃` |
|rightContext |`$'`|  input 字符串中出现在 lastMatch 后面的文本  `已废弃` |

通过这些属性可以提取出与 `exec()` 和 `test()` 执行的操作相关的信息。来看下面的例子：
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
以上代码创建了一个模式，用于搜索任何后跟 "hort" 的字符，并把第一个字符放在了捕获组中。
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
直接通过类名就可以访问的方法

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
直接通过类名就可以访问的属性

| 方法                                                                                                                | 介绍                                |
|:------------------------------------------------------------------------------------------------------------------|:----------------------------------|
| [Number.EPSILON](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/EPSILON) | 表示 1 与大于 1 的最小浮点数之间的差值            |
| Number.MAX_SAFE_INTEGER                                                                                           | 表示在 JavaScript 中最大的安全整数（253 – 1）。 |
| Number.MAX_VALUE                                                                                                  | 表示在 JavaScript 中可表示的最大数值。         |
| Number.MIN_SAFE_INTEGER                                                                                           | 代表在 JavaScript 中最小的安全整数（-253 – 1） |
| Number.MIN_VALUE                                                                                                  | 表示在 JavaScript 中可表示的最小正数值。        |
| Number.NaN                                                                                                        | 表示非数字值，等同于 NaN。                   |
| [Number.NEGATIVE_INFINITY](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/NEGATIVE_INFINITY)|  表示负无穷值。  |
| Number.POSITIVE_INFINITY                                                                                          |      表示正无穷大值。         |

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
实例调用的方法。

| 方法                                          | 介绍 |
|:--------------------------------------------|:---|
| Number.prototype.toExponential()            |返回一个以指数表示法表示该数字的字符串。|
| Number.prototype.toFixed()                  |使用定点表示法来格式化该数值。|
| Number.prototype.toLocaleString()           |回这个数字特定于语言环境的表示字符串。 |
| [Number.prototype.toPrecision(precision)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toPrecision) | 方法返回一个以指定精度表示该数字的字符串。|
| Number.prototype.toString()                 | 返回表示该数字值的字符串。 |
| Number.prototype.valueOf()                  | 返回该数字的值。|

**toExponential(fractionDigits)** 返回一个以指数表示法表示该数字的字符串。
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

**toFixed(digits)** 使用定点表示法来格式化该数值, 小数点后的位数。
应该是一个介于 0 和 100 之间的值，包括 0 和 100。如果这个参数被省略，则被视为 0。
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

**toPrecision(precision)** 返回一个以指定精度表示该数字的字符串。 

**异常 RangeError**  如果 precision 不在 1 和 100 之间（包含两端），则抛出该错误。

```javascript
let numObj = 5.123456;

console.log(numObj.toPrecision()); // 输出 '5.123456'
console.log(numObj.toPrecision(5)); // 输出 '5.1235'
console.log(numObj.toPrecision(2)); // 输出 '5.1'
console.log(numObj.toPrecision(1)); // 输出 '5'

numObj = 0.000123;

console.log(numObj.toPrecision()); // 输出 '0.000123'
console.log(numObj.toPrecision(5)); // 输出 '0.00012300'
console.log(numObj.toPrecision(2)); // 输出 '0.00012'
console.log(numObj.toPrecision(1)); // 输出 '0.0001'

// 请注意，在某些情况下可能会返回指数表示法字符串
console.log((1234.5).toPrecision(2)); // 输出 '1.2e+3'
```

### [6. String](#)
String 是对应字符串的引用类型。要创建一个 String 对象，使用 String 构造函数并传入一个数值，如下例所示：

```javascript
let stringObject = new String("hello world");
```
每个 String 对象都有一个 length 属性，表示字符串中字符的数量。来看下面的例子：
```javascript
let stringValue = "hello world";
console.log(stringValue.length); // "11"
```

#### [6.1 JavaScript 字符](#)
JavaScript 字符串由 16 位码元（code unit）组成。对多数字符来说，每 16 位码元对应一个字符。换句话说，字符串的 length 属性表示字符串包含多少 16 位码元：

**charAt()** 方法返回给定索引位置的字符，由传给方法的整数参数指定。具体来说，这个方法查找指定索引位置的 16 位码元，并返回该码元对应的字符：
```javascript
let message = "abcde";
console.log(message.charAt(2)); // "c" 
```
JavaScript 字符串使用了两种 Unicode 编码混合的策略：UCS-2 和 UTF-16。对于可以采用 16 位编码的字符（U+0000~U+FFFF），这两种编码实际上是一样的。

使用 charCodeAt()方法可以查看指定码元的字符编码。这个方法返回指定索引位置的码元值，索
引以整数指定。比如：
```javascript
let message = "abcde";
// Unicode "Latin small letter C"的编码是 U+0063
console.log(message.charCodeAt(2)); // 99
// 十进制 99 等于十六进制 63
console.log(99 === 0x63); // true
```
fromCharCode()方法用于根据给定的 UTF-16 码元创建字符串中的字符。这个方法可以接受任意多个数值，并返回将所有数值对应的字符拼接起来的字符串：
```javascript
// Unicode "Latin small letter A"的编码是 U+0061
// Unicode "Latin small letter B"的编码是 U+0062
// Unicode "Latin small letter C"的编码是 U+0063
// Unicode "Latin small letter D"的编码是 U+0064
// Unicode "Latin small letter E"的编码是 U+0065
console.log(String.fromCharCode(0x61, 0x62, 0x63, 0x64, 0x65)); // "abcde"
// 0x0061 === 97
// 0x0062 === 98
// 0x0063 === 99
// 0x0064 === 100
// 0x0065 === 101
console.log(String.fromCharCode(97, 98, 99, 100, 101)); // "abcde" 
```
对于 U+0000~U+FFFF 范围内的字符，**length、charAt()、charCodeAt()和 fromCharCode()** 返回的结果都跟预期是一样的。

这是因为在这个范围内，每个字符都是用 16 位表示的，而这几个方法也都基于 16 位码元完成操作。只要字符编码大小与码元大小一一对应，这些方法就能如期工作。

16 位只能唯一表示65 536 个字符。这对于大多数语言字符集是足够了，在 Unicode 中称为 **基本多语言平面**（**BMP**）。

为了表示更多的字符，Unicode 采用了一个策略，即每个字符使用另外 16 位去选择一个**增补平面**。这种每个字符使用 **两**个 16 位码元的策略称为代理对。

在涉及增补平面的字符时，前面讨论的字符串方法就会出问题。比如，下面的例子中使用了一个笑脸表情符号，也就是一个使用代理对编码的字符：

```javascript
let message = "ab𠮷de";
console.log(message.length); // 6
console.log(message.charAt(1)); // b

console.log(message.charAt(2)); // <?>
console.log(message.charAt(3)); // ☺
console.log(message.charAt(4)); // d
console.log(message.charCodeAt(1)); // 98
console.log(message.charCodeAt(2)); // 55362
console.log(message.charCodeAt(3)); // 57271
console.log(message.charCodeAt(4)); // 101


console.log(String.fromCodePoint(134071)); // ☺
console.log(String.fromCharCode(97, 98,134071, 100, 101)); // abஷde
```
为正确解析既包含单码元字符又包含代理对字符的字符串，可以使用 **codePointAt()** 来代替charCodeAt()。
跟使用 charCodeAt()时类似，codePointAt()接收 16 位码元的索引并返回该索引位置上的码点（code point）。
码点是 Unicode 中一个字符的完整标识。比如，"c"的码点是 0x0063，而"☺"的码点是 0x1F60A。码点可能
是 16 位，也可能是 32 位，而 codePointAt()方法可以从指定码元位置识别完整的码点。

```javascript
let message = "ab☺de";
console.log(message.codePointAt(1)); // 98
console.log(message.codePointAt(2)); // 128522
console.log(message.codePointAt(3)); // 56842
console.log(message.codePointAt(4)); // 100
```
注意，如果传入的码元索引并非代理对的开头，就会返回错误的码点。这种错误只有检测单个字符的时候
才会出现，可以通过从左到右按正确的码元数遍历字符串来规避。迭代字符串可以智能地识别代理对的码点：

```javascript
console.log([..."ab☺de"]); // ["a", "b", "☺", "d", "e"]
```
与 charCodeAt()有对应的 codePointAt()一样，fromCharCode()也有一个对应的 fromCodePoint()。
这个方法接收任意数量的码点，返回对应字符拼接起来的字符串：
```javascript
console.log(String.fromCharCode(97, 98, 55357, 56842, 100, 101)); // ab☺de
console.log(String.fromCodePoint(97, 98, 128522, 100, 101)); // ab☺de 
```

#### [6.2 normalize](#)
某些 Unicode 字符可以有多种编码方式。有的字符既可以通过一个 BMP 字符表示，也可以通过一个代理对表示。比如：

```javascript
// U+00C5：上面带圆圈的大写拉丁字母 A
console.log(String.fromCharCode(0x00C5)); // Å
// U+212B：长度单位“埃”
console.log(String.fromCharCode(0x212B)); // Å
// U+004：大写拉丁字母 A
// U+030A：上面加个圆圈
console.log(String.fromCharCode(0x0041, 0x030A)); // Å 
```

比较操作符不在乎字符看起来是什么样的，因此这 3 个字符互不相等。
```javascript
let a1 = String.fromCharCode(0x00C5),
a2 = String.fromCharCode(0x212B),
a3 = String.fromCharCode(0x0041, 0x030A);

console.log(a1, a2, a3); // Å, Å, Å
console.log(a1 === a2); // false
console.log(a1 === a3); // false
console.log(a2 === a3); // false
```

为解决这个问题，Unicode提供了 4种规范化形式，可以将类似上面的字符规范化为一致的格式，无论底层字符的代码是什么。
* NFD（Normalization Form D）
* NFC（Normalization Form C）
* NFKD（Normalization Form KD）
* NFKC（Normalization Form KC）

可以使用 normalize()方法对字符串应用上述规范化形式，使用时需要传入表示哪种形式的字符串："NFD"、"NFC"、"NFKD"或"NFKC"。

#### [6.3 静态方法](#)
直接通过类名可以调用的方法：

|||
String.fromCharCode(numN...)

[String.fromCharCode(numN...)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode) 返回由指定的 UTF-16 码元序列创建的字符串。
```javascript
String.fromCharCode(num1)
String.fromCharCode(num1, num2)
String.fromCharCode(num1, num2, /* …, */ numN)
```
* **参数**：num, 一个介于 0 和 65535（0xFFFF）之间的数字，表示一个 UTF-16 码元。
* **返回值**: 一个长度为 N 的字符串，由 N 个指定的 UTF-16 码元组成。

[String.fromCodePoint()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/fromCodePoint) 将根据指定的码位序列返回一个字符串。

```javascript
String.fromCodePoint(num1)
String.fromCodePoint(num1, num2)
String.fromCodePoint(num1, num2, /* …, */ numN)

console.log(String.fromCodePoint(9731, 9733, 9842, 0x2f804));
// Expected output: "☃★♲你"
```
* **参数**：num,一个介于 0 和 0x10FFFF（包括两者）之间的整数，表示一个 Unicode 码位。
* **返回值**: 通过使用指定的码位序列创建的字符串。

[String.raw()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/raw)   静态方法是模板字符串的标签函数。它的作用类似于 Python 中的 r 前缀或 `C#` 中用于字符串字面量的 @ 前缀。
```javascript
// Create a variable that uses a Windows
// path without escaping the backslashes:
const filePath = String.raw`C:\Development\profile\aboutme.html`;

console.log(`The file was uploaded from: ${filePath}`);
// Expected output: "The file was uploaded from: C:\Development\profile\aboutme.html"
```

#### [6.4 字符串截取](#)
一共有三个方法，其中一个已经被废除。

| 方法                                          | 介绍                                               |
|:--------------------------------------------|:-------------------------------------------------|
|[String.prototype.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/slice) | 方法提取字符串的一部分，并将其作为新字符串返回，而不修改原始字符串。               |
|[String.prototype.substr()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substr)|  **不再推荐使用该特性** 。                                 |
|[String.prototype.substring()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substring) | 返回该字符串从起始索引到结束索引（不包括）的部分，如果未提供结束索引，则返回到字符串末尾的部分。 |


[String.prototype.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/slice)方法提取字符串的一部分，并将其作为新字符串返回，而不修改原始字符串。
```javascript
slice(indexStart)
slice(indexStart, indexEnd)
```
* **indexStart** 要返回的子字符串中包含的第一个字符的索引。
* **indexEnd** `可选` 要返回的子字符串中排除的第一个字符的索引。
```javascript
const str1 = "The morning is upon us."; // str1 的长度是 23。
const str2 = str1.slice(1, 8);
const str3 = str1.slice(4, -2);
const str4 = str1.slice(12);
const str5 = str1.slice(30);

console.log(str2); // he morn
console.log(str3); // morning is upon u
console.log(str4); // is upon us.
console.log(str5); // ""
```
 
[String.prototype.substr()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substr) **不再推荐使用该特性** 。
```javascript
substr(start)
substr(start, length)
```
* **start** 返回子字符串中要包含的第一个字符的索引。
* **length** `可选` 要提取的字符数。

```javascript
const aString = "Mozilla";

console.log(aString.substr(0, 1)); // 'M'
console.log(aString.substr(1, 0)); // ''
console.log(aString.substr(-1, 1)); // 'a'
console.log(aString.substr(1, -1)); // ''
console.log(aString.substr(-3)); // 'lla'
console.log(aString.substr(1)); // 'ozilla'
console.log(aString.substr(-20, 2)); // 'Mo'
console.log(aString.substr(20, 2)); // ''
```

[String.prototype.substring()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substring)  返回该字符串从起始索引到结束索引（不包括）的部分，如果未提供结束索引，则返回到字符串末尾的部分。

```javascript
substring(indexStart)
substring(indexStart, indexEnd)
```
* **indexStart** 返回子字符串中第一个要**包含**的字符的索引。
* **indexEnd** `可选` 返回子字符串中第一个要**排除**的字符的索引。

```javascript
const anyString = "Mozilla";

console.log(anyString.substring(0, 1)); // 'M'
console.log(anyString.substring(1, 0)); // 'M'

console.log(anyString.substring(0, 6)); // 'Mozill'

console.log(anyString.substring(4)); // 'lla'
console.log(anyString.substring(4, 7)); // 'lla'
console.log(anyString.substring(7, 4)); // 'lla'

console.log(anyString.substring(0, 7)); // 'Mozilla'
console.log(anyString.substring(0, 10)); // 'Mozilla'
```

#### [6.5 字符串迭代与解构](#)
字符串的原型上暴露了一个@@iterator 方法，表示可以迭代字符串的每个字符。可以像下面这样
手动使用迭代器：

```javascript
let message = "abc";
let stringIterator = message[Symbol.iterator]();
console.log(stringIterator.next()); // {value: "a", done: false}
console.log(stringIterator.next()); // {value: "b", done: false}
console.log(stringIterator.next()); // {value: "c", done: false}
console.log(stringIterator.next()); // {value: undefined, done: true}
```

在 for-of 循环中可以通过这个迭代器按序访问每个字符：
```javascript
for (const c of "abcde") {
    console.log(c);
}
```
有了这个迭代器之后，字符串就可以通过解构操作符来解构了。比如，可以更方便地把字符串分割为字符数组：
```javascript
let message = "abcde";
console.log([...message]); // ["a", "b", "c", "d", "e"]
```

#### [6.6 字符串模式匹配方法](#)
String 类型专门为在字符串中实现模式匹配设计了几个方法。第一个就是 match()方法，这个方法本质上跟 RegExp 对象的 exec()方法相同。

match()方法接收一个参数，可以是一个正则表达式字符串，也可以是一个 RegExp 对象。来看下面的例子：

```javascript
let text = "cat, bat, sat, fat"; 
let pattern = /.at/; 

// 等价于 pattern.exec(text) 
let matches = text.match(pattern); 
console.log(matches.index); // 0 
console.log(matches[0]); // "cat" 
console.log(pattern.lastIndex); // 0
```
> match()方法返回的数组与 RegExp 对象的 exec()方法返回的数组是一样的：

另一个查找模式的字符串方法是 search()。这个方法唯一的参数与 match()方法一样：正则表达式字符串或 RegExp 对象。这个方法返回模式第一个匹配的位置索引，如果没找到则返回-1。
**search()始终从字符串开头向后匹配模式**。

```javascript
let text = "cat, bat, sat, fat"; 
let pos = text.search(/at/); 
console.log(pos); // 1
```
为简化子字符串替换操作，ECMAScript 提供了 replace()方法。这个方法接收两个参数，第一个参数可以是一个 RegExp 对象或一个
字符串（这个字符串不会转换为正则表达式），第二个参数可以是一个字符串或一个函数。

如果第一个参数是字符串，那么只会替换第一个子字符串。要想替换所有子字符串，第一个参数必须为正则表达式并且带全局标记，如下面的例子所示：

```javascript
let text = "cat, bat, sat, fat";
let result = text.replace("at", "ond");
console.log(result); // "cond, bat, sat, fat"
result = text.replace(/at/g, "ond");
console.log(result); // "cond, bond, sond, fond"
```
第二个参数是字符串的情况下，有几个特殊的字符序列，可以用来插入正则表达式操作的值。ECMA-262 中规定了下表中的值。

| 字符序列                                                                                         | 替换文本                                                                                         |
|:---------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------|
| `$$`                                                                                         | `$`                                                                                          |
| `$&` | 匹配整个模式的子字符串。与 RegExp.lastMatch 相同                                                            |
| `$'` | 匹配的子字符串之前的字符串。与 RegExp.rightContext 相同                                                       |
| **$`**| 匹配的子字符串之后的字符串。与 RegExp.leftContext 相同                                                        |
| `$n`| 匹配第 n 个捕获组的字符串，其中 n 是 0~9。比如，`$1` 是匹配第一个捕获组的字符串，`$2` 是匹配第二个捕获组的字符串，以此类推。如果没有捕获组，则值为空字符串       |
| `$nn`| 匹配第 nn 个捕获组字符串，其中 nn 是 01~99。比如，`$01` 是匹配第一个捕获组的字符串，`$02` 是匹配第二个捕获组的字符串，以此类推。如果没有捕获组，则值为空字符串     |

使用这些特殊的序列，可以在替换文本中使用之前匹配的内容，如下面的例子所示：
```javascript
let text = "cat, bat, sat, fat";
result = text.replace(/(.at)/g, "word ($1)");
console.log(result); // word (cat), word (bat), word (sat), word (fat)
```
这里，每个以"at"结尾的词都会被替换成"word"后跟一对小括号，其中包含捕获组匹配的内容$1。

#### [6.7  HTML方法](#)
早期的浏览器开发商认为使用 JavaScript 动态生成 HTML 标签是一个需求。因此，早期浏览器扩展了规范，增加
了辅助生成 HTML 标签的方法。下表总结了这些 HTML 方法。不过，这些方法基本上已经没有人使用了，因为结果通常不是语义化的标记。


|方 法 |输 出|
|:---|:---|
|anchor(name)| `<a name="name">string</a>`|
|big() |`<big>string</big>`|
|bold() |`<b>string</b>`|
|fixed()| `<tt>string</tt>`|
|fontcolor(color)| `<font color="color">string</font>`|
|fontsize(size)| `<font size="size">string</font>`|
|italics() |`<i>string</i>`|
|link(url)| `<a href="url">string</a>`|
|small()| `<small>string</small>`|
|strike()| `<strike>string</strike>`|
|sub() |`<sub>string</sub>`|
|sup()| `<sup>string</sup>`|

#### [6.8 实例方法](#)
详情请查看 [String](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)。

| 方法                                          | 介绍                                                                                      |
|:--------------------------------------------|:----------------------------------------------------------------------------------------|
|String.prototype.anchor()已弃用| 创建一个带有名称的 `<a>` 元素字符串                                                                   | 
|String.prototype.at()| 方法接受一个整数值，并返回一个新的 String，该字符串由位于指定偏移量处的单个 UTF-16 码元组成。该方法允许正整数和负整数。负整数从字符串中的最后一个字符开始倒数。 |
|String.prototype.charAt()| 返回一个由给定索引处的单个 UTF-16 码元构成的新字符串。                                                         |
|String.prototype.charCodeAt()| charCodeAt() 方法返回一个整数，表示给定索引处的 UTF-16 码元，其值介于 0 和 65535 之间。                             |
|String.prototype.codePointAt()| codePointAt() 方法返回一个非负整数，该整数是从给定索引开始的字符的 Unicode 码位值。                                   |
|String.prototype.concat()| 方法将字符串参数连接到调用的字符串，并返回一个新的字符串。                                                           |
|String.prototype.endsWith()| endsWith() 方法用于判断一个字符串是否以指定字符串结尾，如果是则返回 true，否则返回 false。                                |
|String.prototype.includes()| includes() 方法执行区分大小写的搜索，以确定是否可以在一个字符串中找到另一个字符串，并根据情况返回 true 或 false。                    |
|String.prototype.indexOf()| indexOf() 方法在字符串中搜索指定子字符串，并返回其第一次出现的位置索引。                                               |
|String.prototype.isWellFormed()| 返回一个表示该字符串是否包含单独代理项的布尔值。                                                                |
|String.prototype.lastIndexOf()| 搜索该字符串并返回指定子字符串最后一次出现的索引。                                                               |
|String.prototype.localeCompare()| 返回一个数字，表示参考字符串在排序顺序中是在给定字符串之前、之后还是与之相同。                                                 |
|String.prototype.match()| 检索字符串与正则表达式进行匹配的结果。                                                                     |
|String.prototype.matchAll()| 返回一个迭代器，该迭代器包含了检索字符串与正则表达式进行匹配的所有结果。                                                    |
|String.prototype.normalize()|  方法返回该字符串的 Unicode 标准化形式。   |
|String.prototype.padEnd()| padEnd() 方法会将当前字符串从末尾开始填充给定的字符串|
|String.prototype.padStart()|   padStart() 方法用另一个字符串填充当前字符串（如果需要会重复填充|
|String.prototype.repeat()| repeat() 方法构造并返回一个新字符串，其中包含指定数量的所调用的字符串副本，这些副本连接在一起。                                    |
|String.prototype.replace()| 返回一个新字符串，其中一个、多个或所有匹配的 pattern 被替换为 replacement。                                        |
|String.prototype.replaceAll()| 方法返回一个新字符串，其中所有匹配 pattern 的部分都被替换为 replacement。                                         |
|String.prototype.search()| 用于在 String 对象中执行正则表达式的搜索，寻找匹配项。                                                         |
|String.prototype.slice()| 方法提取字符串的一部分，并将其作为新字符串返回，而不修改原始字符串。                                                      |
|String.prototype.small()已弃用| 创建一个 `<small>` 元素字符串                                                                    |
|String.prototype.split()| split() 方法接受一个模式，通过搜索模式将字符串分割成一个有序的子串列表，将这些子串放入一个数组，并返回该数组。                             |
|String.prototype.startsWith()| startsWith() 方法用来判断当前字符串是否以另外一个给定的子字符串开头，并根据判断结果返回 true 或 false。                        |
|String.prototype.strike()已弃用| strike() 方法创建一个 `<strike>` 元素字符串                                                        |
|String.prototype.sub()已弃用| 值的 sub() 方法创建一个 `<sub>` 元素字符串                                                           |
|String.prototype.substr()已弃用| substr() 方法返回该字符串的一部分，从指定的索引开始，然后扩展到给定数量的字符。                                            |
|String.prototype.substring()| 返回该字符串从起始索引到结束索引（不包括）的部分，如果未提供结束索引，则返回到字符串末尾的部分。                                        |
|String.prototype.sup()已弃用| String 值的 sup() 方法创建一个 `<sup>` 元素字符串                                                    |
|String.prototype`[Symbol.iterator]()`| 允许字符串与大多数期望传入可迭代对象的语法一起使用                                                               |
|String.prototype.toLocaleLowerCase()| toLocaleLowerCase() 方法会根据特定区域设置的大小写映射规则，将字符串转换为小写形式并返回。                                 |
|String.prototype.toLocaleUpperCase()| toLocaleUpperCase() 方法会根据特定区域设置的大小写映射规则，将字符串转换为大写形式并返回。                                 |                                                                                        |
|String.prototype.toLowerCase()| 小写                                                                                      |
|String.prototype.toString()| 字符串                                                                                     |
|String.prototype.toUpperCase()| 大写                                                                                      |
|String.prototype.toWellFormed()| 方法返回一个字符串，其中该字符串的所有单独代理项都被替换为 Unicode 替换字符 U+FFFD。                                      |
|String.prototype.trim()| 从字符串的两端移除空白字符，并返回一个新的字符串                                                                |
|String.prototype.trimEnd()| 方法会从字符串的结尾移除空白字符，并返回一个新的字符串，而不会修改原始字符串。                                                 |
|String.prototype.trimStart()| 会从字符串的开头移除空白字符，并返回一个新的字符串，而不会修改原始字符串。                                                   |
|String.prototype.valueOf() | 返回 String 对象的字符串值。                                                                      |


### [7. 单例内置对象](#)
ECMA-262 对内置对象的定义是“任何由 ECMAScript 实现提供、与宿主环境无关，并在 ECMAScript 程序开始执行时就存在的对象”。这就意味着，开发者不用显式地实例化内置对象，因为它们已经实例
化好了。前面我们已经接触了大部分内置对象，包括 Object、Array 和 String。

ECMA-262 定义的另外两个单例内置对象：**Global** 和 **Math**。

### [8. Global](#)
Global 对象是 ECMAScript 中最特别的对象，因为代码不会显式地访问它。

**ECMA-262 规定 Global 对象为一种兜底对象，它所针对的是不属于任何对象的属性和方法。事实上，不存在全局变量或全局函数这种东西。在全局作用域中定义的变量和函数都会变成 Global 对象的属性**。

包括 isNaN()、isFinite()、parseInt()和 parseFloat()，实际上都是 Global 对象的方法。除了这些，Global 对象上还有另外一些方法。
#### [8.1 URL 编码方法](#) 
encodeURI()和 encodeURIComponent()方法用于编码统一资源标识符（URI），以便传给浏览器。

```javascript
let uri = "http://www.wrox.com/illegal value.js#start";
// "http://www.wrox.com/illegal%20value.js#start"
console.log(encodeURI(uri).toString());
// "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.js%23start"
console.log(encodeURIComponent(uri));
```
与 encodeURI()和 encodeURIComponent()相对的是 decodeURI()和 decodeURIComponent()。
* decodeURI()只对使用 encodeURI()编码过的字符解码。
* 类似地，decodeURIComponent()解码所有被 encodeURIComponent()编码的字符，基本上就是解码所有特殊值。

```javascript
let uri = "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.js%23start";
// http%3A%2F%2Fwww.wrox.com%2Fillegal value.js%23start
console.log(decodeURI(uri));
// http:// www.wrox.com/illegal value.js#start
console.log(decodeURIComponent(uri)); 
```

#### [8.2 eval](#)
最后一个方法可能是整个 ECMAScript 语言中最强大的了，它就是 eval()。这个方法就是一个完
整的 **ECMAScript 解释器**，它接收一个参数，即一个要执行的 ECMAScript（JavaScript）字符串。

```javascript
eval("console.log('hi')");
//上面这行代码的功能与下一行等价：
console.log("hi"); 
```
可以在 eval()内部定义一个函数或变量，然后在外部代码中引用
```javascript
eval("function sayHi() { console.log('hi'); }");
sayHi(); 
```
在严格模式下，在 eval()内部创建的变量和函数无法被外部访问。换句话说，最后两个例子会报错。同样，在严格模式下，赋值给 eval 也会导致错误：
```javascript
"use strict";

eval("function sayHi() { console.log('hi'); }");
sayHi(); //ReferenceError: sayHi is not defined
```
#### [8.3 Global 对象属性](#)
Global 对象有很多属性，其中一些前面已经提到过了。
* 像 undefined、NaN 和 Infinity 等特殊值都是 Global 对象的属性。
* 所有原生引用类型构造函数，比如 Object 和 Function，也都是Global 对象的属性。

|属 性|  说 明 |
|:---|:-----|
|undefined| 特殊值 undefined|
|NaN |特殊值 NaN|
|Infinity| 特殊值 Infinity|
|Object |Object 的构造函数|
|Array |Array 的构造函数|
|Function |Function 的构造函数|
|Boolean |Boolean 的构造函数|
|String |String 的构造函数|
|Number| Number 的构造函数|
|Date |Date 的构造函数|
|RegExp |RegExp 的构造函数|
|Symbol| Symbol 的伪构造函数|
|Error |Error 的构造函数|
|EvalError |EvalError 的构造函数|
|RangeError |RangeError 的构造函数|
|ReferenceError| ReferenceError 的构造函数|
|SyntaxError| SyntaxError 的构造函数|
|TypeError |TypeError 的构造函数|
|URIError |URIError 的构造函数|

#### [8.4 window 对象](#)
虽然 ECMA-262 没有规定直接访问 Global 对象的方式，但浏览器将 window 对象实现为 Global对象的代理。

> 因此，所有全局作用域中声明的变量和函数都变成了 window 的属性。

```javascript
var color = "red";
function sayColor() {
 console.log(window.color);
}
window.sayColor(); // "red"
```

另一种获取 Global 对象的方式是使用如下的代码：
```javascript
let global = function() {
    return this;
}(); 
```
这段代码创建一个立即调用的函数表达式，返回了 this 的值。

**当一个函数在没有明确（通过成为某个对象的方法，或者通过 call()/apply()）指定this值的情况下执行时，this值等于Global对象**。

因此，调用一个简单返回 this 的函数是在任何执行上下文中获取 Global 对象的通用方式。

### [9. Math](#)
ECMAScript 提供了 Math 对象作为保存数学公式、信息和计算的地方。Math 对象提供了一些辅助计算的属性和方法。

> Math 对象上提供的计算要比直接在 JavaScript 实现的快得多，因为 Math 对象上的
> 计算使用了 JavaScript 引擎中更高效的实现和处理器指令。但使用 Math 计算的问题是精
> 度会因浏览器、操作系统、指令集和硬件而异

#### [9.1 Math 对象属性](#)
Math 对象有一些属性，主要用于保存数学中的一些特殊值。

| 属 性            | 说 明           |
|:---------------|:----------------|
| Math.E         | 自然对数的基数 e 的值 |
| Math.LN10      | 10 为底的自然对数      |
| Math.LN2       | 2 为底的自然对数       |
| Math.LOG2E     | 以 2 为底 e 的对数    |
| Math.LOG10E    | 以 10 为底 e 的对数   |
| Math.PI        | π 的值            |
| Math.SQRT1_2   | 1/2 的平方根        |
| Math.SQRT2     | 2 的平方根          |

#### [9.2 常用方法](#)
Math 对象还有很多涉及各种简单或高阶数运算的方法,请查看[MDN Math](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math) 文档。

| 方 法                      | 说 明           |
|:-------------------------|:----------------|
| Math.abs(x)              |返回一个数的绝对值。 |
| Math.ceil(x)             | 静态方法总是向上舍入，并返回大于等于给定数字的最小整数。 |
| Math.floor(x)            | 静态方法返回小于等于一个给定数字的最大整数。 |
| Math.pow(base, exponent) | 返回基数（base）的指数（exponent）次幂，即 base^exponent。 |
| Math.round(x)            | 返回给定数字的值四舍五入到最接近的整数。 |
| Math.sqrt(x)             | 函数返回一个数的平方根。 |
| Math.random(x)      | Math.random() 静态方法返回一个大于等于 0 且小于 1 的伪随机浮点数，并在该范围内近似均匀分布，然后你可以缩放到所需的范围。 |

#### [9.3 min()和 max()方法](#)
Math 对象也提供了很多辅助执行简单或复杂数学计算的方法。 min()和 max()方法用于确定一组数值中的最小值和最大值。这两个方法都接收任意多个参数，如

下面的例子所示：
```javascript
let max = Math.max(3, 54, 32, 16);
console.log(max); // 54
let min = Math.min(3, 54, 32, 16);
console.log(min); // 3
```
在 3、54、32 和 16 中，Math.max()返回 54，Math.min()返回 3。使用这两个方法可以避免使用额外的循环和 if 语句来确定一组数值的最大最小值。

要知道数组中的最大值和最小值，可以像下面这样使用扩展操作符：
```javascript
let values = [1, 2, 3, 4, 5, 6, 7, 8];
let max = Math.max(...val); 
```
