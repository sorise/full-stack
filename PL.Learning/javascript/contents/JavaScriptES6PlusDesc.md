## [Javasript 解构赋值](#)
> **介绍**：解构赋值可以帮助我们更加简洁地从数组和对象中提取数据，使得代码更易读、更易维护

-----

- [一、解构赋值的概念和作用](#一解构赋值的概念和作用)
- [二、函数参数的解构赋值](#二函数参数的解构赋值)
  
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