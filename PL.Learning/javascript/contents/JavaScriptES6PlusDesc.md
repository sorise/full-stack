## [Javasript 解构赋值](#)
> **介绍**：解构赋值可以帮助我们更加简洁地从数组和对象中提取数据，使得代码更易读、更易维护

-----

- [一、解构赋值的概念和作用](#一解构赋值的概念和作用)
- [二、函数参数的解构赋值](#二函数参数的解构赋值)
- [三、函数参数的解构赋值](#三解构的圆括号问题)
- [四、函数参数的解构赋值](#二函数参数的解构赋值)
- [五、运算符的扩展](#五运算符的扩展)
  
---

### [一、解构赋值的概念和作用](#)
解构赋值是 ES6 引入的一项重要特性，它可以使得从数组或对象中提取数据变得更加简洁、灵活。
* 通过解构赋值，可以轻松地从数组或对象中提取所需的数据，并赋值给对应的变量，而无需显式地逐个取出元素或属性。
* 这种语法上的简洁性使得代码更易读、更易写，同时也减少了不必要的中间变量，提高了代码的可维护性。

**解构赋值允许我们以更直观的方式获取数组和对象中的元素或属性，同时也提供了默认值和嵌套结构的支持**。
* 在处理复杂数据结构时，解构赋值可以帮助我们快速地提取所需数据，避免了繁琐的手动操作，提高了代码的可读性和开发效率。
* 默认值和嵌套结构的支持使得解构赋值更加灵活，能够应对各种复杂的数据情况，使得代码更具通用性和健壮性。

#### [1.1 数组解构赋值](#)
数组解构赋值是 ES6 引入的一项语法特性，用于从数组中提取元素并将其赋值给变量。

```javascript
const colors = ['red', 'green', 'blue'];
const [firstColor, secondColor, thirdColor] = colors;
```
可以进行一些更加高级的操作，比如忽略某些元素或使用剩余的元素：
```javascript
const numbers = [1, 2, 3, 4, 5];

const [firstNum, , thirdNum, ...rest] = numbers;
console.log(firstNum); // 输出 1
console.log(thirdNum); // 输出 3
console.log(rest); // 输出 [4, 5]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```
在这个例子中，我们使用了数组解构赋值来忽略第二个元素，并将剩余的元素赋值给 `rest 变量`，这样可以方便地处理数组中的多余元素。

**在函数中使用数组解构赋值** ，数组解构赋值也可以用在函数参数中，这使得我们能够更方便地从传入的数组参数中提取所需的元素。例如：
```javascript
function printFirstTwo([first, second]) {
  console.log(`The first two elements are ${first} and ${second}.`);
}

printFirstTwo(numbers); // 输出 "The first two elements are 1 and 2."

function printPersonInfo({ name, age, city }) {
    console.log(`${name} is ${age} years old and lives in ${city}.`);
}

printPersonInfo(person); // 输出 "Alice is 25 years old and lives in Shanghai."
```
通过在 `函数参数` 中使用 `数组解构赋值`，我们可以直接从传入的数组参数中提取所需的元素，使得函数更加清晰和简洁。

#### [1.2 对象的解构赋值](#)
解构不仅可以用于数组，还可以用于对象。

```javascript
const person = { name: 'Alice', age: 25, city: 'Shanghai' };
const { name, age, city } = person;

console.log(name); // 输出 'Alice'
console.log(age); // 输出 25
console.log(city); // 输出 'Shanghai'
```
在这个例子中，我们使用对象解构赋值从 person 对象中提取了 name、age 和 city 属性，并将它们分别赋值给了同名的变量。

除了基本的对象解构赋值外，我们还可以进行一些更加高级的操作，比如设置默认值或使用新的变量名：
```javascript
const { name, age, city, job = 'Engineer' } = person;
console.log(job); // 输出 'Engineer'

const { name: fullName, age: yearsOld, city: location } = person;
console.log(fullName); // 输出 'Alice'
console.log(yearsOld); // 输出 25
console.log(location); // 输出 'Shanghai'
```
如果变量名与属性名不一致，必须写成下面这样。
```javascript
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'
```
如果解构失败，变量的值等于 **undefined**。
```javascript
let {foo} = {bar: 'baz'};
foo // undefined
```
与数组一样，解构也可以用于嵌套结构的对象。

```javascript
let obj = {
    p: [
        'Hello',
        { y: 'World' }
    ]
};

let { p: [x, { y }] } = obj;
x // "Hello"
y // "World"
```

#### [1.3 属性同名](#)
对象的解构赋值是下面形式的简写, 对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。
```javascript
class user {
    constructor(_name, _age) {
        this.name = _name;
        this.age = _age;
    }

    toString() {
        return `${this.name} ${this.age}`;
    }
}

let me = new user("king", 26);
console.log(me);

let {name: King,age: Age } = me;
console.log(King);  //king
console.log(Age);   //26
```

#### [1.4 默认值](#)
对象的解构也可以指定默认值, 默认值生效的条件是，对象的属性值严格等于 `undefined`。
```javascript
var {x = 3} = {};
x // 3

var {x, y = 5} = {x: 1};
x // 1
y // 5

var {x: y = 3} = {};
y // 3

var {x: y = 3} = {x: 5};
y // 5

var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"
```

#### [1.5 字符串的解构赋值](#)
字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象。

```javascript
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```
类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。
```javascript
let {length : len} = 'hello';
len // 5
```

#### [1.6 数值和布尔值的解构赋值](#)
解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。
```javascript
//数值和布尔值的包装对象都有toString属性，因此变量s都能取到值。
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```
**解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象**。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。
```javascript
let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError
```

### [二、函数参数的解构赋值](#)
在现代 JavaScript 编程中，`函数参数中的解构赋值` 是一项强大而灵活的特性，它能够让我们轻松地从传入的 `对象` 或 `数组参数`
中提取所需的数据，并将其赋值给变量。通过这种方式，我们可以更加便捷地处理`函数的参数`，使代码变得更加清晰和易于理解。

#### [2.1 对象解构赋值作为函数参数](#)
**对象解构赋值作为函数参数** , 对象解构赋值可以直接用在函数的参数中，以便从传入的对象参数中提取所需的属性值并将其赋值给变量。

```javascript
function printPersonInfo({ name, age, city }) {
  console.log(`${name} is ${age} years old and lives in ${city}.`);
}

const person = { name: 'Alice', age: 25, city: 'Shanghai' };
printPersonInfo(person); // 输出 "Alice is 25 years old and lives in Shanghai."
```

#### [2.2 数组解构赋值作为函数参数](#)
数组解构赋值也可以用在函数的参数中，以便从传入的数组参数中提取所需的元素并将其赋值给变量。

```javascript
function printFirstTwo([first, second]) {
  console.log(`The first two elements are ${first} and ${second}.`);
}

const numbers = [1, 2, 3, 4, 5];
printFirstTwo(numbers); // 输出 "The first two elements are 1 and 2."
```

```javascript
[[1, 2], [3, 4]].map(([a, b]) => a + b);
// [ 3, 7 ]
```

#### [2.3 函数参数的解构也可以使用默认值](#)
函数参数的解构也可以使用默认值。
```javascript
function move({x = 0, y = 0} = {}) {
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]
```
和函数默认值区分开。
```javascript
function move({x, y} = {x:0, y:0}) {
    return [x, y];
}

console.log(move({x: 3, y: 8})) ; // [3, 8]
console.log(move({x: 3})); // [3, undefined]
console.log(move({})); // [ undefined, undefined ]
console.log(move()); // [0, 0]
```

### [三、解构的圆括号问题](#)
解构赋值虽然很方便，但是解析起来并不容易。对于编译器来说，一个式子到底是模式，还是表达式，没有办法从一开始就知道，必须解析到（或解析不到）等号才能知道。
由此带来的问题是，如果模式中出现圆括号怎么处理。ES6 的规则是，只要有可能导致解构的歧义，就不得使用圆括号。

但是，这条规则实际上不那么容易辨别，处理起来相当麻烦。因此，建议只要有可能，就不要在模式中放置圆括号。

不能使用圆括号的情况
**以下三种解构赋值不得使用圆括号**。

#### [3.1 变量声明语句](#)
下面 6 个语句都会报错，因为它们都是变量声明语句，模式不能使用圆括号。

```javascript
// 全部报错
let [(a)] = [1];

let {x: (c)} = {};
let ({x: c}) = {};
let {(x: c)} = {};
let {(x): c} = {};

let { o: ({ p: p }) } = { o: { p: 2 } };
```
#### [3.2 函数参数](#)
函数参数也属于变量声明，因此不能带有圆括号。

```javascript
// 报错
function f([(z)]) { return z; }
// 报错
function f([z,(x)]) { return x; }
```

#### [3.3 赋值语句的模式](#)
下面代码将整个模式放在圆括号之中，导致报错。
```javascript
// 全部报错
({ p: a }) = { p: 42 };
([a]) = [5];
```

```javascript
// 报错
[({ p: a }), { x: c }] = [{}, {}];
```
上面代码将一部分模式放在圆括号之中，导致报错。

#### [3.4 可以使用圆括号的情况](#)
可以使用圆括号的情况只有一种：赋值语句的非模式部分，可以使用圆括号。

```javascript
[(b)] = [3]; // 正确
({ p: (d) } = {}); // 正确
[(parseInt.prop)] = [3]; // 正确
```
上面三行语句都可以正确执行，因为首先它们都是赋值语句，而不是声明语句；其次它们的圆括号都不属于模式的一部分。
* 第一行语句中，模式是取数组的第一个成员，跟圆括号无关；
* 第二行语句中，模式是p，而不是d；
* 第三行语句与第一行语句的性质一致。

### [四、解构的应用](#)
解构非常有用，可以简写很多代码。

#### [4.1 交换变量的值](#)
交换变量x和y的值，这样的写法不仅简洁，而且易读，语义非常清晰。
```javascript
let x = 1;
let y = 2;

[x, y] = [y, x];
```

#### [4.2 从函数返回多个值](#)
函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象里返回。有了解构赋值，取出这些值就非常方便。

```javascript
// 返回一个数组
function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();

// 返回一个对象
function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
```

#### [4.3 函数参数的定义](#)
解构赋值可以方便地将一组参数与变量名对应起来。
```javascript
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

#### [4.4 提取 JSON 数据](#)
解构赋值对提取 JSON 对象中的数据，尤其有用。
```javascript
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]
```

#### [4.5 函数参数的默认值](#)
指定参数的默认值，就避免了在函数体内部再写 `var foo = config.foo || 'default foo'`;这样的语句。
```javascript
jQuery.ajax = function (url, {
    async = true,
    beforeSend = function () {},
    cache = true,
    complete = function () {},
    crossDomain = false,
    global = true,
    // ... more config
    } = {}) 
{
    // ... do stuff
};
```

指定参数的默认值，就避免了在函数体内部再写var foo = config.foo || 'default foo';这样的语句。

#### [4.6 遍历 Map 结构](#)
任何部署了 Iterator 接口的对象，都可以用for...of循环遍历。Map 结构原生支持 Iterator 接口，配合变量的解构赋值，获取键名和键值就非常方便。

```javascript
const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
console.log(key + " is " + value);
}
// first is hello
// second is world
```
如果只想获取键名，或者只想获取键值，可以写成下面这样。
```javascript
// 获取键名
for (let [key] of map) {
// ...
}

// 获取键值
for (let [,value] of map) {
// ...
}

```
#### [4.7 输入模块的指定方法](#)
加载模块时，往往需要指定输入哪些方法。解构赋值使得输入语句非常清晰。

```javascript
const { SourceMapConsumer, SourceNode } = require("source-map");
```

### [五、运算符的扩展](#)
ES6后新增了一些运算符。

#### [5.1 指数运算符](#)
ES2016 新增了一个指数运算符（`**`）,指数运算符可以与等号结合，形成一个新的赋值运算符（`**=`）。
```javascript
2 ** 2 // 4
2 ** 3 // 8

let a = 1.5;
a **= 2;
// 等同于 a = a * a;

let b = 4;
b **= 3;
// 等同于 b = b * b * b;
```

#### [5.2 链判断运算符](#)
编程实务中，如果读取对象内部的某个属性，往往需要判断一下，属性的上层对象是否存在。
> 比如，读取message.body.user.firstName这个属性，安全的写法是写成下面这样。

```javascript
// 错误的写法
const  firstName = message.body.user.firstName || 'default';

// 正确的写法
const firstName = (message
  && message.body
  && message.body.user
  && message.body.user.firstName) || 'default';
```
firstName属性在对象的第四层，所以需要判断四次，每一层是否有值。

三元运算符 `?:` 也常用于判断对象是否存在。
```javascript
const fooInput = myForm.querySelector('input[name=foo]')
const fooValue = fooInput ? fooInput.value : undefined
```
上面例子中，必须先判断fooInput是否存在，才能读取fooInput.value。

这样的层层判断非常麻烦，因此 ES2020 引入了“链判断运算符”（optional chaining operator）**?.**，简化上面的写法。
```javascript
const firstName = message?.body?.user?.firstName || 'default';
const fooValue = myForm.querySelector('input[name=foo]')?.value
```
上面代码使用了`?.`运算符，直接在链式调用的时候判断，左侧的对象是否为null或undefined。如果是的，就不再往下运算，而是返回undefined。
```javascript
//下面是判断对象方法是否存在，如果存在就立即执行的例子。
iterator.return?.()
```
对于那些可能没有实现的方法，这个运算符尤其有用。
```javascript
if (myForm.checkValidity?.() === false) {
  // 表单校验失败
  return;
}
```
链判断运算符`?.`有三种写法。
* `obj?.prop` // 对象属性是否存在
* `obj?.[expr]` // 同上
* `func?.(...args)` // 函数或对象方法是否存在

**短路机制** 本质上，?.运算符相当于一种短路机制，只要不满足条件，就不再往下执行。

**括号的影响** 如果属性链有圆括号，链判断运算符对圆括号外部没有影响，只对圆括号内部有影响。
```javascript
(a?.b).c
// 等价于
(a == null ? undefined : a.b).c
```
上面代码中，`?.` 对圆括号外部没有影响，不管a对象是否存在，圆括号后面的 `.c` 总是会执行。 一般来说，使用 `?.` 运算符的场合，不应该使用圆括号。

**报错场合** 以下写法是禁止的，会报错。
```javascript
// 构造函数
new a?.()
new a?.b()

// 链判断运算符的右侧有模板字符串
a?.`{b}`
a?.b`{c}`

// 链判断运算符的左侧是 super
super?.()
super?.foo

// 链运算符用于赋值运算符左侧
a?.b = c
```
**右侧不得为十进制数值** 。为了保证兼容以前的代码，允许 `foo?.3:0` 被解析成 `foo ? .3 : 0`，因此规定如果 `?.` 
后面紧跟一个十进制数字，那么 `?.` 不再被看成是一个完整的运算符，而会按照三元运算符进行处理，也就是说，那个
小数点会归属于后面的十进制数字，形成一个小数。

#### [5.3 扩展运算符（Spread Operator）](#)
扩展运算符允许你将数组或对象展开为多个元素或属性。

**数组扩展**
```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined); // 输出 [1, 2, 3, 4, 5, 6]
```
**对象扩展**
```javascript
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const combinedObj = { ...obj1, ...obj2 };
console.log(combinedObj); // 输出 { a: 1, b: 2, c: 3, d: 4 }
```

#### [5.4 Null 判断运算符](#)
读取对象属性的时候，如果某个属性的值是null或undefined，有时候需要为它们指定默认值。常见做法是通过 `||` 运算符指定默认值。

```javascript
const headerText = response.settings.headerText || 'Hello, world!';

const animationDuration = response.settings.animationDuration || 300;
const showSplashScreen = response.settings.showSplashScreen || true;
const animationDuration = response.settings?.animationDuration ?? 300;
```

#### [5.5 空值合并运算符（Nullish Coalescing Operator）](#)
空值合并运算符??用于返回其左侧操作数的值，如果该值是null或undefined，则返回右侧操作数的值。

```javascript
const value1 = null ?? 'default';
console.log(value1); // 输出 "default"

const value2 = 'hello' ?? 'default';
console.log(value2); // 输出 "hello"
```