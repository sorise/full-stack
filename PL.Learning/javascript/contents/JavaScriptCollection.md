## [Javasript 集合类型](#)
> **介绍**：JavaScript 提供了多种内置的集合类型，包括数组（Array）、对象（Object）、Map 和 Set。

-----
- [1. Array](#1-array)


-----
### [1. Array](#)
Array 应该就是 ECMAScript 中最常用的类型了,数组是最基本的集合类型之一，它可以存储任意类型的值。数组元素可以通过索引访问，索引是从 0 开始的整数。

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
alert(colors.length); // 100 
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
    alert(idx);
    alert(element);
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
alert(values); // 0,1,5,10,15
```

**常用的排序方法**：  

<img src="./static/dddddImg.png" width="500px" />




#### [1.1x 操作方法](#)
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

**concat**方法可以在现有数组全部元素基础上创建一个新数组。
```javascript
const array1 = ['a', 'b', 'c'];
const array2 = ['d', 'e', 'f'];
const array3 = array1.concat(array2);

console.log(array3);
// Expected output: Array ["a", "b", "c", "d", "e", "f"]
```


