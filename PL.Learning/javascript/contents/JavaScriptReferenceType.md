### [Javascript 基本引用类型](#)
>**介绍**：在 ECMAScript 中，引用类型是把数据和功能组织到一起的结构，经常被人错误地称作“类”，此章节认识常见的**内置引用类型**。

---

- [1. Date 日期类型](#1-date-日期类型)
- [2. ](#2-)
- [3. ](#3-)


----

### [1. Date 日期类型](#)
ECMAScript 的 Date 类型参考了 Java 早期版本中的 **java.util.Date**。为此，Date 类型将日期
保存为自协调世界时（UTC，Universal Time Coordinated）时间 1970 年 1 月 1 日午夜（零时）至今所经过的**毫秒数**。

[官方文档：MDN Web Docs - JavaScript Date。](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date)
> 使用这种存储格式，Date 类型可以精确表示 1970 年 1 月 1 日之前及之后 285 616 年的日期。

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

* 这些方法的输出与 toLocaleString()和 toString()一样，会因浏览器而异。因此不能用于在用户界面上一致地显示日期。

#### [1.6 日期/时间组件方法](#)
Date 类型剩下的方法（见下表）直接涉及取得或设置日期值的特定部分。注意表中“UTC 日期”，指的是没有时区偏移（将日期转换为 GMT）时的日期。

**返回时间**

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

**返回UTC时间**

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


**设置时间**

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

**设置UTC时间**

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
* **Function.prototype[Symbol.hasInstance]()**
* **Function.prototype.toString()**

### [2. RegExp](#)
ECMAScript 通过 RegExp 类型支持正则表达式，正则表达式使用类似 Perl 的简洁语法来创建。

```
let expression = /pattern/flags; 
```
这个正则表达式的 pattern（模式）可以是任何简单或复杂的正则表达式，包括字符类、限定符、
分组、向前查找和反向引用。每个正则表达式可以带零个或多个 flags（标记），用于控制正则表达式
的行为。下面给出了表示匹配模式的标记。
* **g**：全局模式，表示查找字符串的全部内容，而不是找到第一个匹配的内容就结束。
* **i**：不区分大小写，表示在查找匹配时忽略 pattern 和字符串的大小写。
* **m**：多行模式，表示查找到一行文本末尾时会继续查找。
* **y**：粘附模式，表示只查找从 lastIndex 开始及之后的字符串。
* **u**：Unicode 模式，启用 Unicode 匹配。
* **s**：dotAll 模式，表示元字符.匹配任何字符（包括`\n` 或`\r`）。

```javascript
// 匹配字符串中的所有"at"
let pattern1 = /at/g;
// 匹配第一个"bat"或"cat"，忽略大小写
let pattern2 = /[bc]at/i;
// 匹配所有以"at"结尾的三字符组合，忽略大小写
let pattern3 = /.at/gi; 
```