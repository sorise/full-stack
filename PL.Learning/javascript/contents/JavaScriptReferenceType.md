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
> 使用这种存储格式，Date 类型可以精确表示 1970 年 1 月 1 日之前及之后 285 616 年的日期。

创建日期对象,在不给 Date 构造函数传参数的情况下，创建的对象将保存当前日期和时间。
```javascript
let now = new Date(); 
```
要基于其他日期和时间创建日期对象，必须传入其毫秒表示（UNIX 纪元 1970 年 1 月 1 日午夜之后的毫秒数）。
ECMAScript为此提供了三个辅助**静态方法**：
* Date.parse()。 方法接收一个表示日期的字符串参数，尝试将这个字符串转换为表示该日期的毫秒数。
* Date.UTC()。
* Date.now()。

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