## [JavaScript 集合类型](#)
> **介绍**：JavaScript 提供了多种内置的集合类型，包括数组（Array）、对象（Object）、Map 和 Set。

-----
- [1. Array](#1-array)
- [2. 定型数组简介](#2-定型数组简介)
- [3. ArrayBuffer](#3-ArrayBuffer)
- [4. DataView](#4-DataView)
- [5. 定型数组](#5-定型数组)
- [6. Map](#6-map)
- [7. WeakMap](#7-weakmap)
- [8. Set](#8-set)
- [9. WeakSet](#9-weakset)

-----
### [1. Array](#)
[Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array) 应该就是 ECMAScript 中最常用的类型了,数组是最基本的集合类型之一，它可以存储任意类型的值。数组元素可以通过索引访问，索引是从 0 开始的整数。

> ECMAScript 数组也是一组有序的数据，但跟其他语言不同的是，数组中每个槽位可以存储任意类型的数据。

**例子**：
```javascript
class Student{
    constructor(_name, _age){
        this.name = _name;
        this.age = _age;
    }
    show(){
        console.log(`${this.name} is ${this.age}.`);
    }
}

let lzm = new Student("liZhiMing", 25);
let sym = Symbol.for("index");

const many = [1,2,3,42.5, lzm, sym];
many.push("banana", "apple", "peach");

console.log(many.length); // 9
console.log(many);
/*[
  1,2,3,42.5,
  Student { name: 'liZhiMing', age: 25 },
  Symbol(index),
  'banana','apple','peach'
]*/
```

* 数组是可调整大小的，并且可以包含不同的数据类型。（当不需要这些特征时，可以使用类型化数组。）
* 数组不是关联数组，因此，不能使用任意字符串作为索引访问数组元素，但必须使用非负整数（或它们各自的字符串形式）作为索引访问。
* 数组的索引从 0 开始：数组的第一个元素在索引 0 处，第二个在索引 1 处，以此类推，最后一个元素是数组的 length 属性减去 1 的值。
* 数组复制操作创建浅拷贝。（所有 JavaScript 对象的标准内置复制操作都会创建浅拷贝，而不是深拷贝）。

#### [1.1 创建数组](#)
有几种基本的方式可以创建数组。一种是使用 Array 构造函数，比如：
```javascript
let colors = new Array(); // 创建一个空数组
let colors = new Array(3);   // 创建一个包含 3 个元素的数组
let names = new Array("Greg");  // 创建一个只包含一个元素，即字符串"Greg"的数组
```
另一种创建数组的方式是使用数组字面量（array literal）表示法。数组字面量是在中括号中包含以逗号分隔的元素列表，如下面的例子所示：

```javascript
let colors = ["red", "blue", "green"]; // 创建一个包含 3 个元素的数组
let names = []; // 创建一个空数组
let values = [1,2,]; // 创建一个包含 2 个元素的数组
```
**创建语法**：
```javascript
new Array()
new Array(element0)
new Array(element0, element1)
new Array(element0, element1, /* … ,*/ elementN)
new Array(arrayLength)
```
**参数**:
* **elementN**  Array 构造函数会根据给定的元素创建一个 JavaScript 数组，但是当仅有一个参数且为数字时除外
（详见下面的 arrayLength 参数）。注意，后者仅适用于用 Array 构造函数创建数组，而不适用于用方括号创建的数组
字面量。 
* **arrayLength** 如果传递给 Array 构造函数的唯一参数是介于 0 到 232 - 1（含）之间的整数，这将返回一个新的
JavaScript 数组，其 length 属性设置为该数字（注意：这意味着一个由 arrayLength 个空槽组成的数组，而不是
具有实际 undefined 值的槽——参见稀疏数组）。

```javascript
const fruits = ["Apple", "Banana"];

console.log(fruits.length); // 2
console.log(fruits[0]); // "Apple"
```

#### [1.2 Array.from()](#)
Array 构造函数还有两个 ES6 新增的用于创建数组的静态方法：from()和 of()。from()用于将类数
组结构转换为数组实例，而of()用于将一组参数转换为数组实例。

**Array.from**() 静态方法从 **可迭代**或**类数组** 对象创建一个新的浅拷贝的数组实例。
```javascript
console.log(Array.from('foo'));
// Expected output: Array ["f", "o", "o"]

console.log(Array.from([1, 2, 3], (x) => x + x));
// Expected output: Array [2, 4, 6]

// 可以使用任何可迭代对象
const iter = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
    yield 3;
    yield 4;
  }
};
console.log(Array.from(iter)); // [1, 2, 3, 4]

const a1 = [1, 2, 3, 4];
const a2 = Array.from(a1, x => x**2);
const a3 = Array.from(a1, function(x) {return x**this.exponent}, {exponent: 2}); 
```
**语法**:
```javascript
Array.from(arrayLike)
Array.from(arrayLike, mapFn)
Array.from(arrayLike, mapFn, thisArg)
```
* **arrayLike**: 想要转换成数组的类数组或可迭代对象。
* **mapFn** `可选` 调用数组每个元素的函数。如果提供，每个将要添加到数组中的值首先会传递给该函数，然后将 mapFn 的返回值增加到数组中。使用以下参数调用该函数：
  * element 数组当前正在处理的元素。
  * index 数组当前正在处理的元素的索引。
* **thisArg** `可选` 执行 mapFn 时用作 this 的值。

```javascript
const pairs = new Map([
    [1, 2],
    [2, 4],
    [4, 8],
    [5, 1],
]);

let result = Array.from(pairs,(ele, idx)=>{
    console.log(`${idx}: <${ele.at(0)},${ele.at(1)}>`);
    return [ele[0]*10,ele[1]*10];
} ,null);

console.log(result);
```
**根据 DOM 元素的属性创建一个数组**
```javascript
// 根据 DOM 元素的属性创建一个数组
const images = document.querySelectorAll("img");
const sources = Array.from(images, (image) => image.src);
const insecureSources = sources.filter((link) => link.startsWith("http://"));
```

#### [1.3 Array.of()](#)
Array.of() 静态方法通过可变数量的参数创建一个新的 Array 实例，而不考虑参数的数量或类型。

```javascript
console.log(Array.of('foo', 2, 'bar', true));
// Expected output: Array ["foo", 2, "bar", true]

console.log(Array.of());
// Expected output: Array []
```

#### [1.4 空位、空槽](#)
数组方法在遇到空槽时有不同的行为，在稀疏数组中数组方法遇到空槽时有不同的行为。

使用数组字面量初始化数组时，可以使用一串逗号来创建空位（hole）。ECMAScript 会将逗号之间相应
索引位置的值当成空位，ES6 规范重新定义了该如何处理这些空位。

```javascript
const options = [,,,,,]; // 创建包含 5 个元素的数组

console.log(options.length); // 5
console.log(options); // [,,,,,] 
```
ES6 新增的方法和迭代器与早期 ECMAScript 版本中存在的方法行为不同。ES6 新增方法普遍将这
些空位当成存在的元素，只不过值为 **undefined**：
```javascript
const options = [1,,,,5];
for (const option of options) {
 console.log(option === undefined);
}
// false
// true
// true
// true
// false 
```
ES6 之前的方法则会忽略这个空位，但具体的行为也会因方法而异：
```javascript
const options = [1,,,,5];
// map()会跳过空位置
console.log(options.map(() => 6)); // [6, undefined, undefined, undefined, 6]
// join()视空位置为空字符串
console.log(options.join('-')); // "1----5"
```
> 由于行为不一致和存在性能隐患，因此实践中要避免使用数组空位。如果确实需要空位，则可以显式地用 undefined 值代替。

ES6 之前的方法则会忽略这个空位，但具体的行为也会因方法而异：
```javascript
const options = [1,,,,5];
// map()会跳过空位置
console.log(options.map(() => 6)); // [6, undefined, undefined, undefined, 6]
// join()视空位置为空字符串
console.log(options.join('-')); // "1----5" 
```
#### [1.5 数组索引](#)
要取得或设置数组的值，需要使用中括号并提供相应值的数字索引，如下所示：
```javascript
let colors = ["red", "blue", "green"]; // 定义一个字符串数组

console.log(colors[0]); // 显示第一项
colors[2] = "black"; // 修改第三项
colors[3] = "brown"; // 添加第四项
```
**length** **属性**的独特之处在于，它不是只读的。通过修改 length 属性，可以从数组末尾删除或添加元素。来看下面的例子：
```javascript
let colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组

colors.length = 2;
console.log(colors[2]); // undefined 

let colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
colors[99] = "black"; // 添加一种颜色（位置 99）
console.log(colors.length); // 100 
```
> 数组最多可以包含 **4 294 967 295** 个元素，这对于大多数编程任务应该足够了。

#### [1.6 检测数组](#)
一个经典的 ECMAScript 问题是判断一个对象是不是数组。在只有一个网页（因而只有一个全局作用域）的情况下，使
用 instanceof 操作符就足矣：

```javascript
if (value instanceof Array){
 // 操作数组
}
```
为解决这个问题，ECMAScript 提供了 **Array.isArray()** 方法。这个方法的目的就是确定一个值是
否为数组，而不用管它是在哪个全局执行上下文中创建的。来看下面的例子：
```javascript
if (Array.isArray(value)){
// 操作数组
}

console.log(Array.isArray([1, 3, 5]));
// Expected output: true

console.log(Array.isArray('[]'));
// Expected output: false

console.log(Array.isArray(new Array(5)));
// Expected output: true

console.log(Array.isArray(new Int16Array([15, 33])));
// Expected output: false
```

#### [1.7 迭代器方法](#)
在 ES6 中，Array 的原型上暴露了 3 个用于检索数组内容的方法：keys()、values()和entries()。

* keys()返回数组索引的迭代器：
* values()返回数组元素的迭代器：
* entries()返回索引/值对的迭代器：

```javascript
const a = ["foo", "bar", "baz", "qux"];
// 因为这些方法都返回迭代器，所以可以将它们的内容
// 通过 Array.from()直接转换为数组实例
const aKeys = Array.from(a.keys());
const aValues = Array.from(a.values());
const aEntries = Array.from(a.entries());

console.log(aKeys); // [0, 1, 2, 3]
console.log(aValues); // ["foo", "bar", "baz", "qux"]
console.log(aEntries); // [[0, "foo"], [1, "bar"], [2, "baz"], [3, "qux"]] 
```
使用 ES6 的解构可以非常容易地在循环中拆分键/值对：
```javascript
const a = ["foo", "bar", "baz", "qux"];
for (const [idx, element] of a.entries()) {
    console.log(idx);
    console.log(element);
}
```

#### [1.8 复制和填充方法](#)
ES6 新增了两个方法，批量复制方法**copyWithin**()，以及填充数组方法 **fill**()。

```javascript
const zeroes = [0, 0, 0, 0, 0];
// 用 5 填充整个数组
zeroes.fill(5);
console.log(zeroes); // [5, 5, 5, 5, 5]
zeroes.fill(0); // 重置

// 用 7 填充索引大于等于 1 且小于 3 的元素
zeroes.fill(7, 1, 3);
console.log(zeroes); // [0, 7, 7, 0, 0];
//雍亲王皇四子胤禛，人品贵重，深肖朕躬

zeroes.fill(0); // 重置
// 索引部分可用，填充可用部分
zeroes.fill(4, 3, 10)
console.log(zeroes); // [0, 0, 0, 4, 4] 
```
**copyWithin(target, start, end)** 方法浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会
改变原数组的长度。
```javascript
const array1 = ['a', 'b', 'c', 'd', 'e'];

// Copy to index 0 the element at index 3
console.log(array1.copyWithin(0, 3, 4));
// Expected output: Array ["d", "b", "c", "d", "e"]

// Copy to index 1 all elements from index 3 to the end
console.log(array1.copyWithin(1, 3));
// Expected output: Array ["d", "d", "e", "d", "e"]
```

#### [1.9 转换方法](#)
前面提到过，所有对象都有 **toLocaleString**()、**toString**()和 **valueOf**()方法。
```javascript
let colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组

console.log(colors.toString()); // red,blue,green
console.log(colors.valueOf()); // red,blue,green
console.log(colors); // red,blue,green
```

**join**()方法接收一个参数，即字符串分隔符，返回包含所有项的字符串。
```javascript
let colors = ["red", "green", "blue"];

console.log(colors.join(",")); // red,green,blue
console.log(colors.join("||")); // red||green||blue 
```

#### [1.10 栈方法](#)
ECMAScript 给数组提供几个方法，让它看起来像是另外一种数据结构,ECMAScript 数组提供了 push()和 pop()方法，以
实现类似栈的行为。

* push(element...) 方法将指定的元素添加到数组的末尾，并返回新的数组长度。
* pop() 删除最后一个元素，并返回该元素的值。此方法会更改数组的长度。

```javascript
const plants = ['broccoli', 'cauliflower', 'cabbage', 'kale', 'tomato'];

console.log(plants.pop());
// Expected output: "tomato"
```

#### [1.11 队列方法](#)
就像栈是以 LIFO 形式限制访问的数据结构一样，队列以先进先出（FIFO，First-In-First-Out）形式限制访问。

* **shift**() 方法从数组中删除第一个元素，并返回该元素的值。此方法更改数组的长度。
* `unshift(element1, element2, /* …, */ elementN)` 方法将指定元素添加到数组的开头，并返回数组的新长度。

```javascript
const array1 = [1, 2, 3];
const firstElement = array1.shift();

console.log(array1);
// Expected output: Array [2, 3]
console.log(firstElement);
// Expected output: 1
```

#### [1.12 排序方法](#)
数组有两个方法可以用来对元素重新排序：reverse()和 sort()。顾名思义，reverse()方法就是将数组元素反向排列。

* sort(compareFn)  方法就地对数组的元素进行排序，并返回对相同数组的引用。
* reverse() 方法就地反转数组中的元素，并返回同一数组的引用。
* toReversed() 要在不改变原始数组的情况下反转数组中的元素，使用 toReversed() `Baseline 2023`。

```javascript
let values = [1, 2, 3, 4, 5];
values.reverse();

console.log(values); // 5,4,3,2,1 

function compare(value1, value2) {
  if (value1 < value2) {
    return -1;
  } else if (value1 > value2) {
    return 1;
  } else {
    return 0;
  }
}

let values = [0, 1, 5, 10, 15];
values.sort(compare);
console.log(values); // 0,1,5,10,15
```

**常用的排序方法**：  

<img src="./static/dddddImg.png" width="500px" />

#### [1.13 常用操作](#)
**concat**方法可以在现有数组全部元素基础上创建一个新数组。
```javascript
const array1 = ['a', 'b', 'c'];
const array2 = ['d', 'e', 'f'];
const array3 = array1.concat(array2);

console.log(array3);
// Expected output: Array ["a", "b", "c", "d", "e", "f"]
```

[**slice(start, end)**](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
方法返回一个新的数组对象，这一对象是一个由 start 和 end 决定的原数组的浅拷贝（包括 start，不包括 end），其中 start 和 end 代表了数组元素的索引。原始数组不会被改变。
```javascript
slice()
slice(start)
slice(start, end)
```
**例子**
```javascript
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

console.log(animals.slice(2));
// Expected output: Array ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// Expected output: Array ["camel", "duck"]
```

#### [1.13 splice](#)
splice()的主要目的是在数组中间插入元素，但有 3 种不同的方式使用这个方法 **违反了单一职责原则**。

```javascript
splice(start)
splice(start, deleteCount)
splice(start, deleteCount, item1)
splice(start, deleteCount, item1, item2)
splice(start, deleteCount, item1, item2, /* …, */ itemN)
```

splice()方法始终返回这样一个数组，它包含从数组中被删除的元素（如果没有删除元素，则返
回空数组）。以下示例展示了上述 3 种使用方式。

**删除**。需要给 splice()传 2 个参数：要删除的第一个元素的位置和要删除的元素数量。可以从
数组中删除任意多个元素，比如 splice(0, 2)会删除前两个元素。

**插入**。需要给 splice()传 3 个参数：开始位置、0（要删除的元素数量）和要插入的元素，可
以在数组中指定的位置插入元素。 第三个参数之后还可以传第四个、第五个参数，乃至任意多个要插入的元素。
比如，splice(2, 0, "red", "green")会从数组位置 2 开始插入字符串"red"和"green"。

**替换**。splice()在删除元素的同时可以在指定位置插入新元素，同样要传入 3 个参数：开始位
置、要删除元素的数量和要插入的任意多个元素。要插入的元素数量不一定跟删除的元素数量
一致。比如，splice(2, 1, "red", "green")会在位置 2 删除一个元素，然后从该位置开始
向数组中插入"red"和"green"。

#### [1.14 搜索和位置方法](#)
ECMAScript 提供两类搜索数组的方法：按严格相等搜索和按断言函数搜索。

**严格相等** ECMAScript 提供了 3 个严格相等的搜索方法：indexOf()、lastIndexOf()和 includes()。其
中，前两个方法在所有版本中都可用，而第三个方法是 ECMAScript 7 新增的。

```javascript
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
console.log(numbers.indexOf(4)); // 3
console.log(numbers.lastIndexOf(4)); // 5
console.log(numbers.includes(4)); // true
console.log(numbers.indexOf(4, 4)); // 5
console.log(numbers.lastIndexOf(4, 4)); // 3
console.log(numbers.includes(4, 7)); // false

let person = { name: "Nicholas" };
let people = [{ name: "Nicholas" }];
let morePeople = [person];

console.log(people.indexOf(person)); // -1
console.log(morePeople.indexOf(person)); // 0
console.log(people.includes(person)); // false
console.log(morePeople.includes(person)); // true 
```

**断言函数** ECMAScript 也允许按照定义的断言函数搜索数组，每个索引都会调用这个函数。断言函数的返回
值决定了相应索引的元素是否被认为匹配。

find()和 findIndex()方法使用了断言函数。这两个方法都从数组的最小索引开始。find()返回
第一个匹配的元素，findIndex()返回第一个匹配元素的索引。这两个方法也都接收第二个可选的参数，
用于指定断言函数内部 this 的值。

```javascript
const people = [
 {
 name: "Matt",
 age: 27
 },
 {
 name: "Nicholas",
 age: 29
 }
];
console.log(people.find((element, index, array) => element.age < 28));
// {name: "Matt", age: 27}
console.log(people.findIndex((element, index, array) => element.age < 28));
// 0 
```
找到匹配项后，这两个方法都不再继续搜索。
```javascript
const evens = [2, 4, 6];
// 找到匹配后，永远不会检查数组的最后一个元素
evens.find((element, index, array) => {
console.log(element);
console.log(index);
console.log(array);
return element === 4;
});
// 2
// 0
// [2, 4, 6]
// 4
// 1
// [2, 4, 6]
```

#### [1.15 迭代方法](#)
ECMAScript 为数组定义了 5 个迭代方法。每个方法接收两个参数：以每一项为参数运行的函数，
以及可选的作为函数运行上下文的作用域对象（影响函数中 this 的值）。传给每个方法的函数接收 3
个参数：数组元素、元素索引和数组本身。

* every(callbackFn, thisArg)：对数组每一项都运行传入的函数，如果对每一项函数都返回 true，则这个方法返回 true。
* filter(callbackFn, thisArg)：对数组每一项都运行传入的函数，函数返回 true 的项会组成数组之后返回。
* forEach(callbackFn, thisArg)：对数组每一项都运行传入的函数，没有返回值。
* map(callbackFn, thisArg)：对数组每一项都运行传入的函数，返回由每次函数调用的结果构成的数组。
* some(callbackFn, thisArg)：对数组每一项都运行传入的函数，如果有一项函数返回 true，则这个方法返回 true。


**callbackFn(element, index, array) -> bool**;
  * element 数组中当前正在处理的元素。
  * index 正在处理的元素在数组中的索引。
  * array 调用了 every() 的数组本身。

```javascript
const many = [11.2,20,30,42,52];

let new_many = many.filter((element, idx ,array)=>{
  return element > 25;
});

console.log(new_many); //[ 30, 42, 52 ]

function isBiggerThan10(element, index, array) {
  return element > 10;
}

let r = [20, 50, 80, 11, 40].every(isBiggerThan10); // true
console.log(r);
let m = [12, 5, 8, 1, 4].some(isBiggerThan10); // true
console.log(m);
```

#### [1.16 归并方法](#)
ECMAScript 为数组提供了两个归并方法：**reduce**()和 **reduceRight**()。这两个方法都会迭代数
组的所有项，并在此基础上构建一个最终返回值。reduce()方法从数组第一项开始遍历到最后一项。而reduceRight()从最后一项开始遍历至第一项。

reduce()和 reduceRight()的函数接收 4 个参数：**上一个归并值**、**当前项**、**当前项的索引**和数组本身。

```javascript
let values = [1, 2, 3, 4, 5];
let sum = values.reduce((prev, cur, index, array) => prev + cur);
console.log(sum); // 15 
```

#### [1.17 操作方法列表](#)
数组操作的基本方法：

| 方法        | 描述                                                                                   |
|-------------|----------------------------------------------------------------------------------------|
| concat()    | 连接两个或多个数组，并返回已连接数组的副本。                                             |
| copyWithin()| 将数组中的数组元素复制到指定位置或从指定位置复制。                                       |
| entries()   | 返回键/值对数组迭代对象。                                                               |
| every()     | 检查数组中的每个元素是否通过测试。                                                       |
| fill()      | 用静态值填充数组中的元素。                                                               |
| filter()    | 使用数组中通过测试的每个元素创建新数组。                                                 |
| find()      | 返回数组中第一个通过测试的元素的值。                                                     |
| findIndex() | 返回数组中通过测试的第一个元素的索引。                                                   |
| forEach()   | 为每个数组元素调用函数。                                                                 |
| from()      | 从对象创建数组。                                                                         |
| includes()  | 检查数组是否包含指定的元素。                                                             |
| indexOf()   | 在数组中搜索元素并返回其位置。                                                           |
| isArray()   | 检查对象是否为数组。                                                                     |
| join()      | 将数组的所有元素连接成一个字符串。                                                       |
| keys()      | 返回 Array Iteration 对象，包含原始数组的键。                                           |
| lastIndexOf()| 在数组中搜索元素，从末尾开始，并返回其位置。                                             |
| map()       | 使用为每个数组元素调用函数的结果创建新数组。                                             |
| pop()       | 删除数组的最后一个元素，并返回该元素。                                                   |
| push()      | 将新元素添加到数组的末尾，并返回新的长度。                                               |
| reduce()    | 将数组的值减为单个值（从左到右）。                                                       |
| reduceRight()| 将数组的值减为单个值（从右到左）。                                                       |
| reverse()   | 反转数组中元素的顺序。                                                                   |
| shift()     | 删除数组的第一个元素，并返回该元素。                                                     |
| slice()     | 选择数组的一部分，并返回新数组。                                                         |
| some()      | 检查数组中的任何元素是否通过测试。                                                       |
| sort()      | 对数组的元素进行排序。                                                                   |
| splice()    | 从数组中添加/删除元素。                                                                 |
| toString()  | 将数组转换为字符串，并返回结果。                                                         |
| unshift()   | 将新元素添加到数组的开头，并返回新的长度。                                               |
| valueOf()   | 返回数组的原始值。                                                                       |
| at()        | 2021.1新提案，解决方括号的限制，可以输入负数。                                           |

### [2. 定型数组简介](#)
定型数组（typed array）是 ECMAScript 新增的结构，目的是提升向原生库传输数据的效率。
实际上，JavaScript 并没有“TypedArray”类型，它所指的其实是一种特殊的包含数值类型的数组。

在 JavaScript 中，“定型数组”（Typed Arrays）是指一组内置的数组类型，它们用于表示二进制数据。与普通的
JavaScript 数组不同，定型数组专为处理二进制数据而设计，并且可以更高效地访问和操作这些数据。定型数组允许直接访问和操作内存中的原始字节，这在处理大量的数值数据或直接与底层数据交互时非常有用。

### [3. ArrayBuffer](#)
[ArrayBuffer](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) 对象用来表示通用的原始二进制数据缓冲区。

```javascript
const buf = new ArrayBuffer(16); // 在内存中分配 16 字节
console.log(buf.byteLength); // 16
```
ArrayBuffer 一经创建就不能再调整大小。不过，可以使用 slice()复制其全部或部分到一个新
实例中：
```javascript
const buf1 = new ArrayBuffer(16);
const buf2 = buf1.slice(4, 12);
console.log(buf2.byteLength); // 8 
```
**ArrayBuffer 分配的堆内存可以被当成垃圾回收，不用手动释放**。

不能仅通过对 ArrayBuffer 的引用就读取或写入其内容。要读取或写入 ArrayBuffer，就必须
通过视图。视图有不同的类型，但引用的都是 ArrayBuffer 中存储的二进制数据。

### [4. DataView](#)
第一种允许你读写 ArrayBuffer 的视图是 DataView。这个视图专为文件 I/O 和网络 I/O 设计，其
API 支持对缓冲数据的高度控制，但相比于其他类型的视图性能也差一些。DataView 对缓冲内容没有
任何预设，也不能迭代。

必须在对已有的 ArrayBuffer 读取或写入时才能创建 DataView 实例。这个实例可以使用全部或
部分 ArrayBuffer，且维护着对该缓冲实例的引用，以及视图在缓冲中开始的位置。

```javascript
const buf = new ArrayBuffer(16);

// DataView 默认使用整个 ArrayBuffer
const fullDataView = new DataView(buf);
console.log(fullDataView.byteOffset); // 0
console.log(fullDataView.byteLength); // 16
console.log(fullDataView.buffer === buf); // true

// 构造函数接收一个可选的字节偏移量和字节长度
// byteOffset=0 表示视图从缓冲起点开始
// byteLength=8 限制视图为前 8 个字节
const firstHalfDataView = new DataView(buf, 0, 8);
console.log(firstHalfDataView.byteOffset); // 0
console.log(firstHalfDataView.byteLength); // 8
console.log(firstHalfDataView.buffer === buf); // true 
```

#### [4.1 实例属性](#)

* DataView.prototype`[Symbol.toStringTag]` `[Symbol.toStringTag]` 属性的初始值为字符串 "DataView"。该属性用于 Object.prototype.toString()。
* DataView.prototype.buffer  ArrayBuffer 是引用该缓冲区的视图。在构造时会被固定，因此该属性只读。
* DataView.prototype.byteLength  视图的长度（以字节为单位）。在构造时会被固定，因此该属性只读。
* DataView.prototype.byteOffset 至 ArrayBuffer 的视图开始位置的字节偏移量。在构造时会被固定，因此该属性只读。

#### [4.2 实例方法](#)
DataView 对存储在缓冲内的数据类型没有预设。它暴露的 API 强制开发者在读、写时指定一个
ElementType，然后 DataView 就会忠实地为读、写而完成相应的转换。

* DataView.prototype.getInt8() 从视图开始的指定字节偏移处获取一个带符号 8 位整数（byte）。
* DataView.prototype.getUint8() 从视图开始的指定字节偏移处获取一个无符号 8 位整数（unsigned byte）。
* DataView.prototype.getInt16() 从视图开始的指定字节偏移处获取一个带符号 16 位整数（short）。
* DataView.prototype.getUint16() 从视图开始的指定字节偏移处获取一个无符号 16 位整数（unsigned short）。
* DataView.prototype.getInt32() 从视图开始的指定字节偏移处获取一个带符号 32 位整数（long）。
* DataView.prototype.getUint32() 从视图开始的指定字节偏移处获取一个无符号 32 位整数（unsigned long）。
* DataView.prototype.getFloat32() 从视图开始的指定字节偏移处获取一个带符号 32 位浮点数（float）。
* DataView.prototype.getFloat64() 从视图开始的指定字节偏移处获取一个带符号 64 位浮点数（double）。
* DataView.prototype.getBigInt64() 从视图开始的指定字节偏移处获取一个带符号 64 位整数（long long）。
* DataView.prototype.getBigUint64() 从视图开始的指定字节偏移处获取一个无符号 64 位整数（unsigned long long）。
* DataView.prototype.setInt8() 在视图开始的指定字节偏移处存储一个带符号 8 位整数（byte）。
* DataView.prototype.setUint8() 在视图开始的指定字节偏移处存储一个无符号 8 位整数（unsigned byte）。
* DataView.prototype.setInt16() 在视图开始的指定字节偏移处存储一个带符号 16 位整数（short）。
* DataView.prototype.setUint16() 在视图开始的指定字节偏移处存储一个无符号 16 位整数（unsigned short）。
* DataView.prototype.setInt32() 在视图开始的指定字节偏移处存储一个带符号 32 位整数（long）。
* DataView.prototype.setUint32() 在视图开始的指定字节偏移处存储一个无符号 32 位整数（unsigned long）。
* DataView.prototype.setFloat32() 在视图开始的指定字节偏移处存储一个带符号 32 位浮点数（float）。
* DataView.prototype.setFloat64() 在视图开始的指定字节偏移处存储一个带符号 64 位浮点数（double）。
* DataView.prototype.setBigInt64() 在视图开始的指定字节偏移处存储一个带符号 64 位 BigInt（long long）。
* DataView.prototype.setBigUint64() 在视图开始的指定字节偏移处存储一个无符号 64 位 BigInt（unsigned long long）。

```javascript
// 在内存中分配两个字节并声明一个 DataView
const buf = new ArrayBuffer(2);
const view = new DataView(buf);
// 说明整个缓冲确实所有二进制位都是 0
// 检查第一个和第二个字符
console.log(view.getInt8(0)); // 0
console.log(view.getInt8(1)); // 0
// 检查整个缓冲
console.log(view.getInt16(0)); // 0
// 将整个缓冲都设置为 1
// 255 的二进制表示是 11111111（2^8 - 1）
view.setUint8(0, 255);
// DataView 会自动将数据转换为特定的 ElementType
// 255 的十六进制表示是 0xFF
view.setUint8(1, 0xFF);
// 现在，缓冲里都是 1 了
// 如果把它当成二补数的有符号整数，则应该是-1
console.log(view.getInt16(0)); // -1 
```

#### [4.3 字节序](#)
DataView 只支持两种约定：大端字节序和小端字节序。大端字节序也称为“网络字节序”，意思
是最高有效位保存在第一个字节，而最低有效位保存在最后一个字节。小端字节序正好相反，即最低有
效位保存在第一个字节，最高有效位保存在最后一个字节。

**DataView 的所有 API 方法都以大端字节序作为默认值，但接收一个可选的布尔值参数，设置为 true 即可启用小端字节序**。

```javascript
// 在内存中分配两个字节并声明一个 DataView
const buf = new ArrayBuffer(2);
const view = new DataView(buf);
// 填充缓冲，让第一位和最后一位都是 1
view.setUint8(0, 0x80); // 设置最左边的位等于 1
view.setUint8(1, 0x01); // 设置最右边的位等于 1
// 缓冲内容（为方便阅读，人为加了空格）
// 0x8 0x0 0x0 0x1
// 1000 0000 0000 0001
// 按大端字节序读取 Uint16
// 0x80 是高字节，0x01 是低字节
// 0x8001 = 2^15 + 2^0 = 32768 + 1 = 32769
console.log(view.getUint16(0)); // 32769
// 按小端字节序读取 Uint16
// 0x01 是高字节，0x80 是低字节
// 0x0180 = 2^8 + 2^7 = 256 + 128 = 384
console.log(view.getUint16(0, true)); // 384
// 按大端字节序写入 Uint16
view.setUint16(0, 0x0004);
// 缓冲内容（为方便阅读，人为加了空格）
// 0x0 0x0 0x0 0x4
// 0000 0000 0000 0100
console.log(view.getUint8(0)); // 0
console.log(view.getUint8(1)); // 4
// 按小端字节序写入 Uint16
view.setUint16(0, 0x0002, true);
// 缓冲内容（为方便阅读，人为加了空格）
// 0x0 0x2 0x0 0x0
// 0000 0010 0000 0000
console.log(view.getUint8(0)); // 2
console.log(view.getUint8(1)); // 0 
```
DataView 完成读、写操作的前提是必须有充足的缓冲区，否则就会抛出 RangeError：

### [5. 定型数组](#)
定型数组是另一种形式的 ArrayBuffer 视图。虽然概念上与 DataView 接近，但定型数组的区别
在于，它特定于一种类型且遵循系统原生的字节序。

```javascript
// 创建一个 12 字节的缓冲
const buf = new ArrayBuffer(12);
// 创建一个引用该缓冲的 Int32Array
const ints = new Int32Array(buf);
// 这个定型数组知道自己的每个元素需要 4 字节
// 因此长度为 3
console.log(ints.length); // 3 
```

**以下是一些常用的定型数组类型**：
* Int8Array：8位有符号整数。
* Uint8Array：8位无符号整数。
* Uint8ClampedArray：与 Uint8Array 类似，但是写入的值如果超出范围会被夹紧到 0 到 255 之间。
* Int16Array：16位有符号整数。
* Uint16Array：16位无符号整数。
* Int32Array：32位有符号整数。
* Uint32Array：32位无符号整数。
* Float32Array：32位浮点数。
* Float64Array：64位浮点数。

定型数组通过一个 ArrayBuffer 对象来存储其元素，这意味着它们可以共享同一段内存区域。这使得定型数组非常适合用于需要高性能的数据交换场景，例如 WebGL 图形编程或者音频处理。

```javascript
let array = new Int8Array([1, 2, 3]);

let buffer = new ArrayBuffer(8);

let intView = new Int32Array(buffer);
let uintView = new Uint8Array(buffer);
```

#### [5.1 创建定型数组](#)
创建定型数组的方式包括读取已有的缓冲、使用自有缓冲、填充可迭代结构，以及填充基于任意类
型的定型数组。另外，通过<ElementType>.from()和<ElementType>.of()也可以创建定型数组：

```javascript
// 创建一个 12 字节的缓冲
const buf = new ArrayBuffer(12);
// 创建一个引用该缓冲的 Int32Array
const ints = new Int32Array(buf);
// 这个定型数组知道自己的每个元素需要 4 字节
// 因此长度为 3
console.log(ints.length); // 3 
// 创建一个长度为 6 的 Int32Array
const ints2 = new Int32Array(6);
// 每个数值使用 4 字节，因此 ArrayBuffer 是 24 字节
console.log(ints2.length); // 6
// 类似 DataView，定型数组也有一个指向关联缓冲的引用
console.log(ints2.buffer.byteLength); // 24
// 创建一个包含[2, 4, 6, 8]的 Int32Array
const ints3 = new Int32Array([2, 4, 6, 8]);
console.log(ints3.length); // 4
console.log(ints3.buffer.byteLength); // 16
console.log(ints3[2]); // 6
// 通过复制 ints3 的值创建一个 Int16Array
const ints4 = new Int16Array(ints3);
// 这个新类型数组会分配自己的缓冲
// 对应索引的每个值会相应地转换为新格式
console.log(ints4.length); // 4
console.log(ints4.buffer.byteLength); // 8
console.log(ints4[2]); // 6
// 基于普通数组来创建一个 Int16Array
const ints5 = Int16Array.from([3, 5, 7, 9]);
console.log(ints5.length); // 4
console.log(ints5.buffer.byteLength); // 8
console.log(ints5[2]); // 7
// 基于传入的参数创建一个 Float32Array
const floats = Float32Array.of(3.14, 2.718, 1.618);
console.log(floats.length); // 3
console.log(floats.buffer.byteLength); // 12
console.log(floats[2]); // 1.6180000305175781 
```
定型数组的构造函数和实例都有一个 **BYTES_PER_ELEMENT** 属性，返回该类型数组中每个元素的大小：

```javascript
console.log(Int16Array.BYTES_PER_ELEMENT); // 2
console.log(Int32Array.BYTES_PER_ELEMENT); // 4

const ints = new Int32Array(1), floats = new Float64Array(1);

console.log(ints.BYTES_PER_ELEMENT); // 4
console.log(floats.BYTES_PER_ELEMENT); // 8 
```

#### [5.2 定型数组行为](#)
从很多方面看，定型数组与普通数组都很相似。定型数组支持如下操作符、方法和属性：


#### [5.3 合并、复制和修改定型数组](#)
定型数组同样使用数组缓冲来存储数据，而数组缓冲无法调整大小。因此，下列方法不适用于定型
数组：
* concat()
* pop()
* push()
* shift()
* splice()
* unshift()

不过，定型数组也提供了两个新方法，可以快速向外或向内复制数据：set()和 subarray()。

* set()从提供的数组或定型数组中把值复制到当前定型数组中指定的索引位置：
* subarray()执行与 set()相反的操作，它会基于从原始定型数组中复制的值返回一个新定型数组。 复制值时的开始索引和结束索引是可选的：

```javascript
// 创建长度为 8 的 int16 数组
const container = new Int16Array(8);
// 把定型数组复制为前 4 个值
// 偏移量默认为索引 0
container.set(Int8Array.of(1, 2, 3, 4));
console.log(container); // [1,2,3,4,0,0,0,0]
// 把普通数组复制为后 4 个值
// 偏移量 4 表示从索引 4 开始插入
container.set([5,6,7,8], 4);
console.log(container); // [1,2,3,4,5,6,7,8]
// 溢出会抛出错误
container.set([5,6,7,8], 7);
// RangeError 

const source = Int16Array.of(2, 4, 6, 8);
// 把整个数组复制为一个同类型的新数组
const fullCopy = source.subarray();
console.log(fullCopy); // [2, 4, 6, 8]
// 从索引 2 开始复制数组
const halfCopy = source.subarray(2);
console.log(halfCopy); // [6, 8]
// 从索引 1 开始复制到索引 3
const partialCopy = source.subarray(1, 3);
console.log(partialCopy); // [4, 6] 
```

### [6. Map](#)
Map 是一种新的集合类型，为这门语言带来了真正的键/值存储机制, API 请查看[ MDN Web Docs 社区 MAP](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)。

#### [6.1 创建Map](#)
使用 new 关键字和 Map 构造函数可以创建一个空映射：

```javascript
const m = new Map(); 
```
如果想在创建的同时初始化实例，可以给 Map 构造函数传入一个可迭代对象，需要包含键/值对数
组。可迭代对象中的每个键/值对都会按照迭代顺序插入到新映射实例中：
```javascript
// 使用嵌套数组初始化映射
const m1 = new Map([
  ["key1", "val1"],
  ["key2", "val2"],
  ["key3", "val3"]
]);
console.log(m1.size); // 3

// 使用自定义迭代器初始化映射
const m2 = new Map({
  [Symbol.iterator]: function*() {
    yield ["key1", "val1"];
    yield ["key2", "val2"];
    yield ["key3", "val3"];
  }
});
console.log(m2.size); // 3 

// 映射期待的键/值对，无论是否提供
const m3 = new Map([[]]);
console.log(m3.has(undefined)); // true
console.log(m3.get(undefined)); // undefined 
```
初始化之后，可以使用 set()方法再添加键/值对。另外，可以使用 get()和 has()进行查询，可
以通过 size 属性获取映射中的键/值对的数量，还可以使用 delete()和 clear()删除值。
```javascript
const m = new Map();
console.log(m.has("firstName")); // false
console.log(m.get("firstName")); // undefined
console.log(m.size); // 0

m.set("firstName", "Matt")
 .set("lastName", "Frisbie");

console.log(m.has("firstName")); // true
console.log(m.get("firstName")); // Matt
console.log(m.size); // 2

m.delete("firstName"); // 只删除这一个键/值对
console.log(m.has("firstName")); // false
console.log(m.has("lastName")); // true
console.log(m.size); // 1

m.clear(); // 清除这个映射实例中的所有键/值对
console.log(m.has("firstName")); // false
console.log(m.has("lastName")); // false
console.log(m.size); // 0
```
set()方法返回映射实例，因此可以把多个操作连缀起来，包括初始化声明：
```javascript
const m = new Map().set("key1", "val1");
m.set("key2", "val2")
 .set("key3", "val3");
console.log(m.size); // 3 
```
Object 只能使用数值、字符串或符号作为键不同，Map 可以使用任何 JavaScript 数据类型作为
键。Map 内部使用 SameValueZero 比较操作（ECMAScript 规范内部定义，语言中不能使用），基本上相
当于使用严格对象相等的标准来检查键的匹配性。与 Object 类似，映射的值是没有限制的。

```javascript
const m = new Map();
const functionKey = function() {};
const symbolKey = Symbol();
const objectKey = new Object();

m.set(functionKey, "functionValue");
m.set(symbolKey, "symbolValue");
m.set(objectKey, "objectValue");

console.log(m.get(functionKey)); // functionValue
console.log(m.get(symbolKey)); // symbolValue
console.log(m.get(objectKey)); // objectValue
// SameValueZero 比较意味着独立实例不冲突
console.log(m.get(function() {})); // undefined 
```
> SameValueZero 是 ECMAScript 规范新增的相等性比较算法。关于 ECMAScript 的相等性比较。

#### [6.2 顺序与迭代](#)
与 Object 类型的一个主要差异是，Map 实例会维护键值对的插入顺序，因此可以根据插入顺序执行迭代操作。

映射实例可以提供一个迭代器（Iterator），能以插入顺序生成 `[key, value]` 形式的数组。可以
通过 entries()方法（或者 Symbol.iterator 属性，它引用 entries()）取得这个迭代器：
```javascript
const m = new Map([
 ["key1", "val1"],
 ["key2", "val2"],
 ["key3", "val3"]
]);

console.log(m.entries === m[Symbol.iterator]); // true 

m.forEach((val, key) => console.log(`${key} -> ${val}`));
// key1 -> val1
// key2 -> val2
// key3 -> val3 
```

### [7. WeakMap](#)
ECMAScript 6 新增的“弱映射”（WeakMap）是一种新的集合类型，为这门语言带来了增强的键/值对存储机制。

WeakMap 是 Map 的“兄弟”类型，其 API 也是 Map 的子集。WeakMap 中的“weak”（弱），描述的是 JavaScript 垃圾回收程序对待“弱映射”中键的方式。

```javascript
new WeakMap()
new WeakMap(iterable)
```
初始化之后可以使用 set()再添加键/值对，可以使用 get()和 has()查询，还可以使用 delete()删除：

```javascript
const wm = new WeakMap();
const key1 = {id: 1},
 key2 = {id: 2};

console.log(wm.has(key1)); // false
console.log(wm.get(key1)); // undefined

wm.set(key1, "Matt")
 .set(key2, "Frisbie");

console.log(wm.has(key1)); // true
console.log(wm.get(key1)); // Matt

wm.delete(key1); // 只删除这一个键/值对
console.log(wm.has(key1)); // false
console.log(wm.has(key2)); // true 
```

#### [7.1 弱键的含义](#)
WeakMap 中“weak”表示弱映射的键是“弱弱地拿着”的。意思就是，这些键不属于正式的引用，不会阻
止垃圾回收。但要注意的是，弱映射中值的引用可不是“弱弱地拿着”的。只要键存在，键/值对就会存在于映射中，并被当作对值的引用。

```javascript
const wm = new WeakMap();
const container = {
  key: {}
};

wm.set(container.key, "val");

function removeReference() {
  container.key = null;
}

console.log(`wm.has(container.key) = ${wm.has(container.key)}`);
//wm.has(container.key) = true

console.log(`wm.get(container.key) = ${wm.get(container.key)}`);
//wm.get(container.key) = val

removeReference();

console.log(`wm.has(container.key) = ${wm.has(container.key)}`);
//wm.has(container.key) = false

console.log(`wm.get(container.key) = ${wm.get(container.key)}`);
//wm.get(container.key) = undefined
```
这一次，container 对象维护着一个对弱映射键的引用，因此这个对象键不会成为垃圾回收的目
标。不过，如果调用了 removeReference()，就会摧毁键对象的最后一个引用，垃圾回收程序就可以
把这个键/值对清理掉。

#### [7.2 不可迭代键](#)
因为 WeakMap 中的键/值对任何时候都可能被销毁，所以没必要提供迭代其键/值对的能力。

#### [7.3 使用弱映射创建私有变量](#)

```javascript

const wm = new WeakMap();
class User {
  constructor(id) {
    this.idProperty = Symbol('id');
    this.setId(id);
  }
  setPrivate(property, value) {
    const privateMembers = wm.get(this) || {};
    privateMembers[property] = value;
    wm.set(this, privateMembers);
  }
  getPrivate(property) {
    return wm.get(this)[property];
  }
  setId(id) {
    this.setPrivate(this.idProperty, id);
  }
  getId() {
    return this.getPrivate(this.idProperty);
  }
}
const user = new User(123);
console.log(user.getId()); // 123
user.setId(456);
console.log(user.getId()); // 456
// 并不是真正私有的
console.log(wm.get(user)[user.idProperty]); // 456
```

慧眼独具的读者会发现，对于上面的实现，外部代码只需要拿到对象实例的引用和弱映射，就可以
取得“私有”变量了。为了避免这种访问，可以用一个闭包把 WeakMap 包装起来，这样就可以把弱映
射与外界完全隔离开了：
```javascript
const User = (() => {
    const wm = new WeakMap();
    class User {
        constructor(id) {
            this.idProperty = Symbol('id');
            this.setId(id);
        }
        setPrivate(property, value) {
            const privateMembers = wm.get(this) || {};
            privateMembers[property] = value;
            wm.set(this, privateMembers);
        }
        getPrivate(property) {
            return wm.get(this)[property];
        }
        setId(id) {
            this.setPrivate(this.idProperty, id);
        }
        getId(id) {
            return this.getPrivate(this.idProperty);
        }
    }
    return User;
})();

const user = new User(123);
console.log(user.getId()); // 123
user.setId(456);
console.log(user.getId()); // 456
```
这样，拿不到弱映射中的健，也就无法取得弱映射中对应的值。虽然这防止了前面提到的访问，但
整个代码也完全陷入了 ES6 之前的闭包私有变量模式。

#### [7.4 DOM 节点元数据](#)
因为 WeakMap 实例不会妨碍垃圾回收，所以非常适合保存关联元数据。来看下面这个例子，其中使用了常规的 Map：

```javascript
const m = new Map();
const loginButton = document.querySelector('#login');
// 给这个节点关联一些元数据
m.set(loginButton, {disabled: true}); 
```
假设在上面的代码执行后，页面被 JavaScript 改变了，原来的登录按钮从 DOM 树中被删掉了。但
由于映射中还保存着按钮的引用，所以对应的 DOM 节点仍然会逗留在内存中，除非明确将其从映射中
删除或者等到映射本身被销毁。

**如果这里使用的是弱映射，如以下代码所示，那么当节点从 DOM 树中被删除后，垃圾回收程序就可以立即释放其内存（假设没有其他地方引用这个对象）**：
```javascript
const wm = new WeakMap();
const loginButton = document.querySelector('#login');
// 给这个节点关联一些元数据
wm.set(loginButton, {disabled: true});
```

### [8. Set](#)
ECMAScript 6 新增的 Set 是一种新集合类型，为这门语言带来集合数据结构。Set 在很多方面都
像是加强的 Map，这是因为它们的大多数 API 和行为都是共有的，API 请查看[ MDN Web Docs 社区 Set](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)。 

#### [8.1 集合创建](#)
使用 new 关键字和 Set 构造函数可以创建一个空集合：
```javascript
const m = new Set();
```
如果想在创建的同时初始化实例，则可以给 Set 构造函数传入一个可迭代对象，其中需要包含插入到新集合实例中的元素：
```javascript
// 使用数组初始化集合
const s1 = new Set(["val1", "val2", "val3"]);

console.log(s1.size); // 3
// 使用自定义迭代器初始化集合
const s2 = new Set({
 [Symbol.iterator]: function*() {
 yield "val1";
 yield "val2";
 yield "val3";
 }
});

console.log(s2.size); // 3 
```
初始化之后，可以使用 add()增加值，使用 has()查询，通过 size 取得元素数量，以及使用 delete()
和 clear()删除元素：
```javascript
const s = new Set();

console.log(s.has("Matt")); // false
console.log(s.size); // 0

s.add("Matt")
 .add("Frisbie");

console.log(s.has("Matt")); // true
console.log(s.size); // 2

s.delete("Matt");
console.log(s.has("Matt")); // false
console.log(s.has("Frisbie")); // true
console.log(s.size); // 1
s.clear(); // 销毁集合实例中的所有值
console.log(s.has("Matt")); // false
console.log(s.has("Frisbie")); // false
console.log(s.size); // 0 
```

### [9. WeakSet](#)
ECMAScript 6 新增的“弱集合”（WeakSet）是一种新的集合类型，为这门语言带来了集合数据结
构。WeakSet 是 Set 的“兄弟”类型，其 API 也是 Set 的子集。WeakSet 中的“weak”（弱），描述的
是 JavaScript 垃圾回收程序对待“弱集合”中值的方式。

可以使用 new 关键字实例化一个空的 WeakSet：
```javascript
class pair{
  constructor(_key, _value){
    this.key = _key;
    this.value = _value;
  }

  [Symbol.toStringTag](){
    return `{${this.key}:${this.value}}`;
  }
}

let me = new pair("me", 73);
let lzm = new pair("lzm", 53);
let ljk = new pair("ljk", 23);

let colors = new WeakSet([me, lzm]);

console.log(colors); //WeakSet { <items unknown> }

console.log(colors.has(me)); // true
console.log(colors.has(lzm)); // true

console.log(me);//pair { key: 'me', value: 73 }
```
**弱集合中的值只能是 Object 或者继承自 Object 的类型，尝试使用非对象设置值会抛出 TypeError**。
```javascript
colors.set(undefined)
//TypeError: colors.set is not a function
```
WeakSet 中“weak”表示弱集合的值是“弱弱地拿着”的。意思就是，这些值不属于正式的引用，不会阻止垃圾回收。
因为 WeakSet 中的值任何时候都可能被销毁，所以没必要提供迭代其值的能力。

