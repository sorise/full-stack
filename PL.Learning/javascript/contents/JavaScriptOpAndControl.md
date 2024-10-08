## [Javasript 操作符、运算符、流程控制](#)
> **介绍** 各种操作符，运算符的使用！

---

- [1. 操作符](#1-操作符)
- [2. 控制语句](#2-控制语句)
- [3. for await of异步迭代器](#3-for-await-of异步迭代器)

----

### [1. 操作符](#)
ECMA-262 描述了一组可用于操作数据值的操作符，包括数学运算 `+` `-` `/` `*` `%` 位运算，关系操作符
还有相同操作符！ 操作符不仅仅可以用于数值，还可以用于布尔值，对象，字符串...，操作符在应用给对象的时候，
通常会调用 `valueOf` / `toString` 方法来取得可以计算的值！

#### [1.1 一元操作符](#)
只操作一个值的操作符叫一元操作符（unary operator）。一元操作符是 ECMAScript中最简单的操作符。

**递增/递减操作符** `++`、`--`
- 递增和递减操作符遵循如下规则。
  - 对于字符串，如果是有效的数值形式，则转换为数值再应用改变。变量类型从字符串变成数值。
  - 对于字符串，如果不是有效的数值形式，则将变量的值设置为 NaN 。变量类型从字符串变成
  数值。
  - 对于布尔值，如果是 false，则转换为 0 再应用改变。变量类型从布尔值变成数值。
  - 对于布尔值，如果是 true，则转换为 1 再应用改变。变量类型从布尔值变成数值。
  - 对于浮点值，加 1 或减 1。
  - 如果是对象，则调用其（第 5 章会详细介绍的）valueOf()方法取得可以操作的值。对得到的
  值应用上述规则。如果是 NaN，则调用 toString()并再次应用其他规则。变量类型从对象变成
  数值。

```javascript
let s1 = "2";
let s2 = "z";
let b = false;
let f = 1.1;
let o = {
    valueOf() {
        return -1;
    }
};
s1++; // 值变成数值 3
console.log(s1);
s2++; // 值变成 NaN
console.log(s2);
b++; // 值变成数值 1
console.log(b);
f--; // 值变成 0.10000000000000009（因为浮点数不精确）
console.log(f);
o--; // 值变成-2
console.log(o);
```

#### [1.2 位操作符](#)
ECMAScript 中的所有数值都以 IEEE 754 64 位格式存储，但位操作并不直接应用到 64 位表示，而是先把值转换为 32 位整数，再进行位操作，之后再把结果转换为 64 位。

主要包括：
- 按位取反 `~`
- 按位与 `&`
- 按位或 `|`
- 按位异或 `^`
- 左移、右移 `<<` `>>`
- 无符号左移、右移 `<<<` `>>>`

```javascript
let oldValue = 64; // 等于二进制 1000000
let newValue = oldValue >> 5; // 等于二进制 10，即十进制 2

let oldValue = -64; // 等于二进制 11111111111111111111111111000000
let newValue = oldValue >>> 5; // 等于十进制 134217726 00000111111111111111111111111110
```

#### [1.3 布尔操作符](#)
对于编程语言来说，布尔操作符跟相等操作符几乎同样重要。如果没有能力测试两个值的关系，那么像 if-else 和循环这样的语句也没什么用了。布尔操作符一共有 3 个：逻辑非、逻辑与和逻辑或。

```javascript
console.log(!false); // true
console.log(!"blue"); // false
console.log(!0); // true
console.log(!NaN); // true
console.log(!""); // true
console.log(!12345); // false 
```

#### [1.4 运算操作](#)
无非是，加减乘除取模 `+` `-` `*` `/` `%`

**加法** ： 如果有任一操作数是对象、数值或布尔值，则调用它们的 toString()方法以获取字符串，然后再
应用前面的关于字符串的规则。对于 undefined 和 null，则调用 String()函数，分别获取
"undefined"和"null"。

```javascript
let result1 = 5 + 5; // 两个数值
console.log(result1); // 10
let result2 = 5 + "5"; // 一个数值和一个字符串
console.log(result2); // "55"

let result3 = 5 + null;
console.log(result3); //5
let result4 = 5 + undefined;
console.log(result4); //NaN

let result6 = '5' + null;
console.log(result6); //5null
let result7 = '5' + undefined;
console.log(result7); //5undefined
```

**减法**: 如果有任一操作数是对象，则调用其 valueOf()方法取得表示它的数值。如果该值是 NaN，则
减法计算的结果是 NaN。如果对象没有 valueOf()方法，则调用其 toString()方法，然后再
将得到的字符串转换为数值。

```javascript
let result1 = 5 - true; // true 被转换为 1，所以结果是 4
let result2 = NaN - 1; // NaN
let result3 = 5 - 3; // 2
let result4 = 5 - ""; // ""被转换为 0，所以结果是 5
let result5 = 5 - "2"; // "2"被转换为 2，所以结果是 3
let result6 = 5 - null; // null 被转换为 0，所以结果是 5
```

#### [1.5 指数操作符](#)
ECMAScript 7 新增了指数操作符，Math.pow()现在有了自己的操作符**，结果是一样的：

```javascript
console.log(Math.pow(3, 2)); // 9
console.log(3 ** 2); // 9
console.log(Math.pow(16, 0.5)); // 4
console.log(16** 0.5); // 4 
```

#### [1.6 关系操作符](#)
关系操作符执行比较两个值的操作，包括小于（<）、大于（>）、小于等于（<=）和大于等于（>=），用法
跟数学课上学的一样。这几个操作符都返回布尔值，如下所示：

```javascript
let result1 = 5 > 3; // true
let result2 = 5 < 3; // false
```

与 ECMAScript 中的其他操作符一样，在将它们应用到不同数据类型时也会发生类型转换和其他行为。

* 如果操作数都是数值，则执行数值比较。
* 如果操作数都是字符串，则逐个比较字符串中对应字符的编码。
* 如果有任一操作数是数值，则将另一个操作数转换为数值，执行数值比较。
* 如果有任一操作数是对象，则调用其 valueOf()方法，取得结果后再根据前面的规则执行比较。
* 如果没有 valueOf()操作符，则调用 toString()方法，取得结果后再根据前面的规则执行比较。
* 如果有任一操作数是布尔值，则将其转换为数值再执行比较。

#### [1.7 是等于和不等于](#)
ECMAScript 中的等于操作符用两个等于号（`==`）表示，如果操作数相等，则会返回 true。不等于
操作符用叹号和等于号（`!=`）表示，如果两个操作数不相等，则会返回 true。这两个操作符都会先进
行类型转换（通常称为强制类型转换）再确定操作数是否相等。

```javascript
let t = '5' == 5;
console.log(t); //true
```
在转换操作数的类型时，相等和不相等操作符 **类型转换** 遵循如下规则。
* 如果任一操作数是布尔值，则将其转换为数值再比较是否相等。false 转换为 0，true 转换为 1。
* 如果一个操作数是字符串，另一个操作数是数值，则尝试将字符串转换为数值，再比较是否相等。
* 如果一个操作数是对象，另一个操作数不是，则调用对象的 valueOf()方法取得其原始值，再根据前面的规则进行比较。

在进行比较时，这两个操作符会**比较规则**遵循如下规则。
* null 和 undefined 相等。
* null 和 undefined 不能转换为其他类型的值再进行比较。
* 如果有任一操作数是 NaN，则相等操作符返回 false，不相等操作符返回 true。记住：即使两个操作数都是 NaN，相等操作符也返回 false，因为按照规则，NaN 不等于 NaN。
* 如果两个操作数都是对象，则比较它们是不是同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回 true。否则，两者不相等。

```javascript
null == undefined  //true
"NaN" == NaN       //false
5 == NaN           //false
NaN == NaN         //false
NaN != NaN         //true
false == 0         //true
true == 1          //true
true == 2          //false
undefined == 0     //false
null == 0          //false
"5" == 5           //true
```

#### [1.8 全等和不全等](#)
全等和不全等操作符与相等和不相等操作符类似，只不过它们在比较相等时不转换操作数。全等操作符由 3 个等于号（===）表示，只有两个操作数在不转换的前提下相等才返回 true，比如：

```javascript
let result1 = ("55" == 55); // true，转换后相等
let result2 = ("55" === 55); // false，不相等，因为数据类型不同

console.log(undefined == null)  //true
console.log(undefined === null) //false
```
另外，虽然 null == undefined 是 true（因为这两个值类似），但 null === undefined 是false，因为它们不是相同的数据类型。

#### [1.9 条件操作符](#)
条件操作符是 ECMAScript 中用途最为广泛的操作符之一，语法跟 C++、C# 中一样：
```javascript
variable = boolean_expression ? true_value : false_value;
```
上面的代码执行了条件赋值操作，即根据条件表达式 boolean_expression 的值决定将哪个值赋
给变量 variable 。如果 boolean_expression 是 true ，则赋值 true_value ；如果
boolean_expression 是 false，则赋值 false_value。比如：
```javascript
let max = (num1 > num2) ? num1 : num2;
```
在这个例子中，max 将被赋予一个最大值。这个表达式的意思是，如果 num1 大于 num2（条件表
达式为 true），则将 num1 赋给 max。否则，将 num2 赋给 max。

#### [1.10 赋值操作符](#)
简单赋值用等于号（=）表示，将右手边的值赋给左手边的变量，如下所示：
```javascript
let num = 10; 
```

* 乘后赋值（`*=`）
* 除后赋值（`/=`）
* 取模后赋值（`%=`）
* 加后赋值（`+=`）
* 减后赋值（`-=`）
* 左移后赋值（`<<=`）
* 右移后赋值（`>>=`）
* 无符号右移后赋值（`>>>=`）

#### [1.11 逗号操作符](#)
逗号操作符可以用来在一条语句中执行多个操作，如下所示：
```javascript
let num1 = 1, num2 = 2, num3 = 3;
```
在一条语句中同时声明多个变量是逗号操作符最常用的场景。不过，也可以使用逗号操作符来辅助
赋值。在赋值时使用逗号操作符分隔值，最终会返回表达式中最后一个值：
```javascript
let num = (5, 1, 4, 8, 0); // num 的值为 0
```
在这个例子中，num 将被赋值为 0，因为 0 是表达式中最后一项。逗号操作符的这种使用场景并不
多见，但这种行为的确存在。


### [2. 控制语句](#)
就是我们常说的 `if-else` `while do-while` `switch`  `for-in` `for-of` `break` `continue`

#### [2.1 if-else](#)
`if else 有几种形态`

```javascript
let age = 35;

if (age > 20){
    console.log('成年人了!');
}
```
`if else`
```javascript
let age = 35;

if (age > 20){
    console.log('合格成年人了!');
}else{
    console.log('不合格！');
}
```
`if else-if`
```javascript
let age = 35;

if (age > 20){
    console.log('合格成年人了!');
}else if (age <= 20 && age >= 18){
    console.log('成年人了!');
}else {
    console.log('未成年！');
}
```

#### [2.2 while do-while](#)
`so seay!`
```javascript
let times = 5;
while (times > 0){
    times--;
    console.log(times);
}

times = 5;
do {
    times--;
    console.log(times);
}
while (times > 0)
```

#### 2.3 switch
和if语句紧密相关的一种流程控制语句！

```javascript
const ChangeRequirementId = 'ChangeRequirementId';
const CloseNotice = 'CloseNotice';
const ShowNotice = 'ShowNotice';
const StoreRequirementSelects = 'StoreRequirementSelects';
const ChangeCountNumber = 'ChangeCountNumber';
const ChangeSortTime = 'ChangeSortTime_';
const JumpPageGetServer = 'JumpPageGetServer';

function reducer(state, action){
    switch (action.type){
        case ChangeRequirementId:
            state.condition.requirementId = parseInt(action.payload.value);
            return {...state};
        case CloseNotice:
            state.noticeIsShow = false;
            state.refuseTransactionId = -1;
            return {...state};
        case ShowNotice:
            state.noticeIsShow = true;
            state.refuseTransactionId = action.payload.value;
            return  {...state};
        case ChangeCountNumber:
            state.condition.count = parseInt(action.payload.value);
            return  {...state};
        case ChangeSortTime: /* 时间排序规则 */
            state.condition.sortTime = action.payload.value;
            return {...state};
        case StoreRequirementSelects:
            state.requirementSelects = action.payload.options;
            return  {...state};
        case JumpPageGetServer: /* 页面跳转 */
            state.tableData = action.payload.tableData;
            state.condition.pageIndex = action.payload.pageIndex;
            return {...state};
        default:
            return state;
    }
}
```

#### 2.4 经典for
`经典for，和其他语言一样！`
```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

#### 2.5 for-in
for-in 语句是一种严格的迭代语句,用于枚举对象中的非符号键属性。如果遍历的是数组，那么key 是 数组下标 0 1 2 3 4 5。
```javascript
let ary = ['name',25,3.12,true,5,6];
for (const key in ary) {
    console.log(ary[key]);
}
```
如果要迭代的变量是null或underfine，则不会执行循环体！

#### 2.6 for-of
在 ECMAScript 2015(ES6) 中 JavaScript 引入了迭代器接口（iterator）用来遍历数据。迭代器对象知道如何每次访问集合中的一项， 并跟踪该序列中的当前位置。在 JavaScript 中迭代器是一个对象，它提供了一个 next() 方法，用来返回序列中的下一项。这个方法返回包含两个属性：done 和 value。
迭代器对象一旦被建立，就能够反复调用 next()。for-of 语句是一种严格的迭代语句。用于遍历可迭代对象的元素！

**可迭代对象**: 一个定义了迭代行为的对象，好比在 for...of 中循环了哪些值。为了实现可迭代，
**一个对象必须实现 @@iterator 方法，这意味着这个对象（或其原型链中的一个对象）必须具备带 Symbol.iterator 键的属性**
* String，Array，TypedArray，Map 和 Set 都内置可迭代对象，由于它们的原型对象都有一个 Symbol.iterator 方法。
```javascript
for (const val of ['name',25,3.12,true,5,6]) {
    console.log(val);
}
```

#### 2.7 break continue
可以用于 for while switch
* break:直接结束整个循环
* continue:直接结束本轮循环，然后指向 update-expression

#### [2.8 标签语句](#)
主要是用于嵌套循环

```javascript
block: for (let i = 0; i < 4; i++) {
    for (let j = 0; j < 4; j++) {
        for (let k = 0; k < 4; k++) {
            console.log(i, "------------", j, "------------", k)
            if (i === 2 && j === 2 && k === 2) {
                continue block
            }
        }

    }
}
```

#### [2.9 with 语句](#)
with 语句的用途是将代码作用域设置为特定的对象，其语法是：
```
with (expression) statement;
``` 
使用 with 语句的主要场景是针对一个对象反复操作，这时候将代码作用域设置为该对象能提供便
利，如下面的例子所示：
```javascript
let qs = location.search.substring(1);
let hostName = location.hostname;
let url = location.href;
```
上面代码中的每一行都用到了 location 对象。如果使用 with 语句，就可以少写一些代码：
```javascript
with(location) {
  let qs = search.substring(1);
  let hostName = hostname;
  let url = href;
} 
```
这里，with 语句用于连接 location 对象。这意味着在这个语句内部，每个变量首先会被认为是
一个局部变量。如果没有找到该局部变量，则会搜索 location 对象，看它是否有一个同名的属性。如
果有，则该变量会被求值为 location 对象的属性。

> **严格模式不允许使用 with 语句，否则会抛出错误**。


### [3. for await of异步迭代器](#)
我们知道 for…of 是同步运行的, 异步迭代器(for-await-of)：循环等待每个Promise对象变为resolved状态才进入下一步。

如今有了一个异步迭代器，代码以下：
```javascript
const justjavac = {
  [Symbol.asyncIterator]: () => {
    const items = [`j`, `u`, `s`, `t`, `j`, `a`, `v`, `a`, `c`];
    return {
      next: () => Promise.resolve({
        done: items.length === 0,
        value: items.shift()
      })
    }
  }
}
```
咱们可使用以下代码进行遍历：

```javascript
for await (const item of justjavac) {
  console.log(item)
}
```
若是你遇到了 `SyntaxError: for await (... of ...) is only valid in async functions and async generators` 错误，
**那是由于 for-await-of 只能在 async 函数或者 async 生成器里面使用**。

```javascript
(async function(){
  for await (const item of justjavac) {
    console.log(item)
  }
})();
```