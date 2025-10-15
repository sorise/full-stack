### [JavaScript 迭代器（Iterators）和生成器（Generators）](#)
> **介绍**：迭代器提供了一种访问集合元素的标准方式，而生成器是一种特殊的函数，可以生成迭代器。

-----
- [一、理解迭代](#一理解迭代)
- [二、迭代器](#二迭代器)
- [三、生成器](#三生成器)
- [四、生成器作为默认迭代器](#四生成器作为默认迭代器)

---
### [一、理解迭代](#)
迭代的英文“iteration”源自拉丁文 itero，意思是“重复”或“再来”。在软件开发领域，“迭代”
的意思是按照顺序反复多次执行一段程序，通常会有明确的终止条件。

#### [1.1 迭代器模式](#)
迭代器模式（特别是在 ECMAScript 这个语境下）描述了一个方案，即可以把有些结构称为“可迭
代对象”（iterable），因为它们实现了正式的 Iterable 接口，而且可以通过迭代器 Iterator 消费。

> 可迭代对象理解成数组或集合这样的集合类型的对象。它们包含的元素都是有限的，而且都具有无歧义的遍历顺序：

**可迭代对象不一定是集合对象，也可以是仅仅具有类似数组行为的其他数据结构**。迭代器无须了解与其关联的可迭代对象的结构，只需要知道如何取得连续的
值。
```javascript
// 数组的元素是有限的
// 递增索引可以按序访问每个元素
let arr = [3, 1, 4];
// 集合的元素是有限的
// 可以按插入顺序访问每个元素
let set = new Set().add(3).add(1).add(4); 
```
每个迭代器都会关联一个可迭代对象，而迭代器会暴露迭代其关联可迭代对象的 API。

#### [1.2 可迭代协议](#)
实现 Iterable 接口（可迭代协议）要求同时具备两种能力：支持迭代的自我识别能力和创建实现Iterator 接口的对象的能力。

在 ECMAScript 中，这意味着必须暴露一个属性作为“默认迭代器”，而且这个属性必须使用特殊的 **Symbol.iterator** 作为键。
。这个默认迭代器属性必须引用一个迭代器工厂函数，调用这个工厂函数必须返回一个新迭代器。

很多内置类型都实现了 Iterable 接口：字符串、数组、集合、映射、arguments、NodeLists等DOM集合类型。

检查是否存在默认迭代器属性可以暴露这个工厂函数：
```javascript
let num = 1;
let obj = {};
// 这两种类型没有实现迭代器工厂函数
console.log(num[Symbol.iterator]); // undefined
console.log(obj[Symbol.iterator]); // undefined 
```
一些实现了迭代器的类型。
```javascript
let str = 'abc';
let arr = ['a', 'b', 'c'];
let map = new Map().set('a', 1).set('b', 2).set('c', 3);
let set = new Set().add('a').add('b').add('c');
let els = document.querySelectorAll('div');
// 这些类型都实现了迭代器工厂函数
console.log(str[Symbol.iterator]); // f values() { [native code] }
console.log(arr[Symbol.iterator]); // f values() { [native code] }
console.log(map[Symbol.iterator]); // f values() { [native code] }
console.log(set[Symbol.iterator]); // f values() { [native code] }
console.log(els[Symbol.iterator]); // f values() { [native code] }
```
调用这个工厂函数会生成一个迭代器。
```javascript
console.log(str[Symbol.iterator]()); // StringIterator {}
console.log(arr[Symbol.iterator]()); // ArrayIterator {}
console.log(map[Symbol.iterator]()); // MapIterator {}
console.log(set[Symbol.iterator]()); // SetIterator {}
console.log(els[Symbol.iterator]()); // ArrayIterator {} 
```
如果对象原型链上的父类实现了 Iterable 接口，那这个对象也就实现了这个接口：

```javascript
class FooArray extends Array {}
let fooArr = new FooArray('foo', 'bar', 'baz');
for (let el of fooArr) {
 console.log(el);
}
// foo
// bar
// baz 
```

#### [1.3 调用迭代器](#)
实际写代码过程中，不需要显式调用这个工厂函数来生成迭代器。实现可迭代协议的所有类型都会
自动兼容接收可迭代对象的任何语言特性。接收可迭代对象的原生语言特性包括：

* for-of 循环
* 数组解构
* 扩展操作符
* Array.from()
* 创建集合
* 创建映射
* Promise.all()接收由期约组成的可迭代对象
* Promise.race()接收由期约组成的可迭代对象
* `yield*` 操作符，在生成器中使用

这些原生语言结构会在后台调用提供的可迭代对象的这个工厂函数，从而创建一个迭代器
```javascript
let arr = ['foo', 'bar', 'baz'];
// for-of 循环
for (let el of arr) {
    console.log(el);
}
// 数组解构
let [a, b, c] = arr;
console.log(a, b, c); // foo, bar, baz
// 扩展操作符
let arr2 = [...arr];
console.log(arr2); // ['foo', 'bar', 'baz']
// Array.from()
let arr3 = Array.from(arr);
console.log(arr3); // ['foo', 'bar', 'baz']
// Set 构造函数
let set = new Set(arr);
console.log(set); // Set(3) {'foo', 'bar', 'baz'}
// Map 构造函数
let pairs = arr.map((x, i) => [x, i]);
console.log(pairs); // [['foo', 0], ['bar', 1], ['baz', 2]]
let map = new Map(pairs);
console.log(map); // Map(3) { 'foo'=>0, 'bar'=>1, 'baz'=>2 } 
```

### [二、迭代器](#)
迭代器（Iterators）迭代器是一种**一次性使用**的对象，用于迭代与其关联的可迭代对象。，它定义了一个序列并可能在终止时返回一个值。

**迭代器的核心**是一个 **next** 方法，该方法返回一个包含 **value**和**done**属性的对象(IteratorResult)。
* **value**属性表示迭代器返回的下一个值。
* **done**属性是一个布尔值，表示是否还可以再次调用 next()取得下一个值；。
```javascript
const iterator = {
    next: function() {
        // 返回一个对象，包含 value 和 done 属性
        return { value: 1, done: false };
    }
};

console.log(iterator.next()); // { value: 1, done: false }
```
**自定义迭代器**:
```javascript
function createIterator(items) {
    let i = 0;
    return {
        next: function() {
            if (i < items.length) {
                return { value: items[i++], done: false };
            } else {
                return { value: undefined, done: true };
            }
        }
    };
}

const myIterator = createIterator([1, 2, 3]);
console.log(myIterator.next()); // { value: 1, done: false }
console.log(myIterator.next()); // { value: 2, done: false }
console.log(myIterator.next()); // { value: 3, done: false }
console.log(myIterator.next()); // { value: undefined, done: true }
```
系统例子：
```javascript
// 可迭代对象
let arr = ['foo', 'bar'];
// 迭代器工厂函数
console.log(arr[Symbol.iterator]); // f values() { [native code] } 
// 迭代器
let iter = arr[Symbol.iterator]();
console.log(iter); // ArrayIterator {}
// 执行迭代
console.log(iter.next()); // { done: false, value: 'foo' }
console.log(iter.next()); // { done: false, value: 'bar' }
console.log(iter.next()); // { done: true, value: undefined } 
```

#### [2.1 自定义迭代器](#)
与 Iterable 接口类似，任何实现 Iterator 接口的对象都可以作为迭代器使用。下面这个例子中
的 Counter 类只能被迭代一定的次数：

**这个类实现了 Iterator 接口，但不理想。这是因为它的每个实例只能被迭代一次**：
```javascript
class Counter {
    // Counter 的实例应该迭代 limit 次
    constructor(limit) {
        this.count = 1;
        this.limit = limit;
    }
    next() {
        if (this.count <= this.limit) {
            return { done: false, value: this.count++ };
        } else {
            return { done: true, value: undefined };
        }
    }
    [Symbol.iterator]() {
        return this;
    }
}

let counter = new Counter(3);
for (let i of counter) {
    console.log(i);
}
// 1
// 2
// 3
```
为了让一个可迭代对象能够创建多个迭代器，必须每创建一个迭代器就对应一个新计数器。为此，
可以把计数器变量放到闭包里，然后 **通过闭包返回迭代器**：
```javascript
class Counter {
    // Counter 的实例应该迭代 limit 次
    constructor(limit) {
        this.count = 1;
        this.limit = limit;
    }
    [Symbol.iterator]() {
        let count = 1,
            limit = this.limit;
        return {
            next() {
                if (count <= limit) {
                    return { done: false, value: count++ };
                } else {
                    return { done: true, value: undefined };
                }
            }
        };
    }
}
```

#### [2.2 提前终止迭代器](#)
可选的 return()方法用于指定在迭代器提前关闭时执行的逻辑。执行迭代的结构在想让迭代器知
道它不想遍历到可迭代对象耗尽时，就可以“关闭”迭代器。可能的情况包括：
* for-of 循环通过 break、continue、return 或 throw 提前退出；
* 解构操作并未消费所有值。

return()方法必须返回一个有效的 IteratorResult 对象。简单情况下，可以只返回{ done: true }。**因为这个返回值只会用在生成器的上下文中**。

```javascript
class Counter {
    // Counter 的实例应该迭代 limit 次
    constructor(limit) {
        this.limit = limit;
    }
    [Symbol.iterator]() {
        let count = 1,
            limit = this.limit;
        return {
            next() {
                if (count <= limit) {
                    return { done: false, value: count++ };
                } else {
                    return { done: true };
                }
            },
            return() {
                console.log('Exiting early');
                return { done: true };
            }
        };
    }
}

let counter = new Counter(5);
for (let i of counter) {
    if (i > 2) {
        break;
    }
    console.log(i);
}
// 1
// 2
// Exiting early

let counter2 = new Counter(5);
try {
    for (let i of counter2) {
        if (i > 2) {
            throw 'err';
        }
        console.log(i);
    }
} catch(e) {}
// 1
// 2
// Exiting early

let counter3 = new Counter(5);
let [a, b] = counter3;
// Exiting early
```
因为 `return()` 方法是可选的，所以并非所有迭代器都是可关闭的。要知道某个迭代器是否可关闭，
可以测试这个迭代器实例的 return 属性是不是函数对象。不过，仅仅给一个不可关闭的迭代器增加这
个方法并不能让它变成可关闭的。这是因为调用 return()不会强制迭代器进入关闭状态。即便如此，
`return()` 方法还是会被调用。

### [三、生成器](#)
**生成器是一个极为灵活的结构，拥有在一个函数块内暂停和恢复代码执行的能力。这种能力具有深远的影响，比如使用生成器可以自定义迭代器和实现协程**。

生成器提供了一种更灵活、更可控的方式来处理异步编程。通过使用yield关键字，我们可以在函数执行过程中暂停和恢复，并且可以将异步操作以同步方式编写和理解。

生成器（Generator）是ES6引入的一种特殊的函数，它可以通过yield关键字来暂停函数的执行，并返回一个包含value和done属性的对象。生成器的概念、作用和原理如下所述：

```javascript
function* generatorFunc() {
  yield 'Hello';
  yield 'World';
}

let generator = generatorFunc();
console.log(generator.next()); // { value: 'Hello', done: false }
console.log(generator.next()); // { value: 'World', done: false }
console.log(generator.next()); // { value: undefined, done: true }
```

> 生成器第一次出现在CLU语言中。CLU语言是美国麻省理工大学的Barbara Liskov教授和她的学生们在1974年至1975年间所
> 设计和开发出来的。Python、C#和Ruby等语言都受到其影响，实现了生成器的特性，生成器在CLU和C#语言中被称为迭代
> 器（iterator），Ruby语言中称为枚举器（Enumerator）。


#### [3.1 生成器基础](#)
生成器的形式是一个函数，函数名称前面加一个星号（`*`）表示它是一个生成器。只要是可以定义函数的地方，就可以定义生成器。

```javascript
// 生成器函数声明
function* generatorFn() {}
// 生成器函数表达式
let generatorFn = function* () {}
// 作为对象字面量方法的生成器函数
let foo = {
 * generatorFn() {}
}
// 作为类实例方法的生成器函数
class Foo {
 * generatorFn() {}
}
// 作为类静态方法的生成器函数
class Bar {
 static * generatorFn() {}
} 
```
> 标识生成器函数的星号不受两侧空格的影响, **箭头函数不能用来定义生成器函数**。
```javascript
// 等价的生成器函数：
function* generatorFnA() {}
function *generatorFnB() {}
function * generatorFnC() {}
// 等价的生成器方法：
class Foo {
 *generatorFnD() {}
 * generatorFnE() {}
} 
```

#### [3.2 yield关键字](#)
生成器函数中，有一个特殊的新关键字：yield——用来标注暂停点，如下段代码所示：
```javascript
function *generator_function(){ 
  yield 1; 
  yield 2; 
  yield 3;
}
```
如何运行生成器呢？如下段代码所示：
```javascript
let generator = generator_function();
console.log(generator.next().value);//1
console.log(generator.next().value);//2
console.log(generator.next().value);//3
console.log(generator.next().done);//true

generator = generator_function();
let iterable = generator[Symbol.iterator]();
console.log(iterable.next().value);//1
console.log(iterable.next().value);//2
console.log(iterable.next().value);//3
console.log(iterable.next().done);//true
```
**常规函数返回一个值，而生成器函数返回一个生成器对象，该生成器对象有一个next方法，返回IteratorResult{value,done}**。
```javascript
for (const val of generator_function()) {
    console.log(val);
}// 1 2 3
```

```javascript
function *generate_iterator(){
    console.log("start");
    yield 1;
    console.log("start 1 ");
    yield 2;
    console.log("start 2 ");
    yield 3;
    console.log("start 3 ");
    return "done"
}

for (const val of generate_iterator()) {
    console.log(val);
}
/*
start
1
start 1
2
start 2
3
start 3
* */
```
**展开生成器对象**
```javascript
function* nTimes(n) {
    let i = 0;
    while(n--) {
        yield i++;
    }
}

function *generate_iterator(){
    yield 1;
    yield 2;
    yield 3;
    return "done"
}

console.log(...generate_iterator()); //1 2 3
```
使用 **for-of** 迭代
```javascript
function *generate_iterator(){
    yield 1;
    yield 2;
    yield 3;
    return "done"
}

let ite = generate_iterator();
for (const v of ite) {
    console.log(v);
}
```

#### [3.3 yield 返回值](#)
yield 还可以通过next返回值。
```javascript
function *generate_iterator(){
    const _name = yield "lzm";
    console.log(`Generating ${_name}`);
    yield "lhy";
}

let ite = generate_iterator();
console.log(ite.next());
console.log(ite.next("CaiXuKun")); //输入
/*
{ value: 'lzm', done: false }
Generating CaiXuKun
{ value: 'lhy', done: false }
* */
```
yield 永远也得不到第一次调用next的返回值。
```javascript
function *generator(){
    let a = yield 1;
    console.log(a); //永远也得不到第一次调用next的返回值
    let b = yield 2;
    console.log(b);
    let c = yield 3;
    console.log(c);
    return undefined;
}

let m = generator();

console.log(m.next('a in'));
console.log(m.next('b in'));
console.log(m.next('c in'));
console.log(m.next('d in'));

/*
{ value: 1, done: false }
b in
{ value: 2, done: false }
c in
{ value: 3, done: false }
d in
{ value: undefined, done: true }
* */
```
第一次调用：m.next('a in')
* 生成器开始执行，遇到第一个 yield 1。
* yield 1 暂停执行，并返回 { value: 1, done: false }。
* 注意：你传入的 'a in' 没有地方接收它，因为这是第一次执行，前面没有 yield 表达式。所以 'a in' 被完全忽略。
* 此时 let a = yield 1; 还没有完成赋值，a 的值还未确定。


第二次调用：m.next('b in')
* 生成器从上次暂停的地方（yield 1）继续执行。
* yield 1 这个表达式的返回值是 'b in'（即本次 next() 的参数）。
* 所以 let a = yield 1; 等价于 let a = 'b in';
* 然后执行 console.log(a); → 输出 'b in'
* 接着执行 let b = yield 2;，暂停并返回 { value: 2, done: false }

第三次调用：m.next('c in')
- 从 yield 2 继续执行。
- yield 2 的返回值是 'c in'，所以 let b = 'c in';
- 执行 console.log(b); → 输出 'c in'
- 然后 let c = yield 3;，暂停并返回 { value: 3, done: false }

第四次调用：`m.next('d in')`
- 从 yield 3 继续执行。
- yield 3 返回 `'d in'`，所以 `let c = 'd in'`;
- 执行 `console.log(c)`; → 输出 `'d in'`
- 然后 `return undefined;`，返回 `{ value: undefined, done: true }`

第一次调用 m.next('a in') 时传入的 'a in' 会被忽略，因为没有上一个 yield 表达式可以接收它。
如果你希望 'a in' 被赋值给 a，你应该在第二次调用 next() 时传入 'a in'：

```javascript
console.log(m.next());        // 启动，忽略参数
console.log(m.next('a in'));  // 这个 'a in' 赋值给 a
console.log(m.next('b in'));
console.log(m.next('c in'));
```


#### [3.4 增强 yield](#)
可以使用星号增强 yield 的行为，让它能够迭代一个可迭代对象，从而一次产出一个值：
```javascript
// 等价的 generatorFn：
/* function* generatorFn() {
     for (const x of [1, 2, 3]) {
         yield x;
         
     }
}*/
function* generatorFn() {
 yield* [1, 2, 3];
}
let generatorObject = generatorFn();
for (const x of generatorFn()) {
 console.log(x);
}
// 1
// 2
// 3 
```
与生成器函数的星号类似，yield 星号两侧的空格不影响其行为：
```javascript
function* generatorFn() {
 yield* [1, 2];
 yield *[3, 4];
 yield * [5, 6];
} 
```

#### [3.5 使用 yield*实现递归算法](#)
yield*最有用的地方是实现递归操作，此时生成器可以产生自身。

```javascript
function* nTimes(n) {
    if (n > 0) {
        yield* nTimes(n - 1);
        yield n - 1;
    }
}
```

#### [3.6 提前终止生成器](#)
与迭代器类似，生成器也支持“可关闭”的概念。一个实现 Iterator 接口的对象一定有 next()
方法，还有一个可选的 `return()` 方法用于提前终止迭代器。生成器对象除了有这两个方法，还有第三
个方法：`throw()`。

```javascript
function* generatorFn() {}
const g = generatorFn();

console.log(g); // generatorFn {<suspended>}
console.log(g.next); // f next() { [native code] }
console.log(g.return); // f return() { [native code] }
console.log(g.throw); // f throw() { [native code] }
```
return()和 throw()方法都可以用于强制生成器进入关闭状态。

throw()方法会在暂停的时候将一个提供的错误注入到生成器对象中。如果错误未被处理，生成器就会关闭：

```javascript
function* generatorFn() {
    for (const x of [1, 2, 3]) {
        yield x;
    }
}
const g = generatorFn();
console.log(g); // generatorFn {<suspended>}
try {
    g.throw('foo');
} catch (e) {
    console.log(e); // foo
}
console.log(g); // generatorFn {<closed>} 
```
不过，假如生成器函数内部处理了这个错误，那么生成器就不会关闭，而且还可以恢复执行。错误
处理会跳过对应的 yield，因此在这个例子中会跳过一个值。

```javascript
function* generatorFn() {
    for (const x of [1, 2, 3]) {
        try {
            yield x;
        } catch(e) {}
    }
}
const g = generatorFn();
console.log(g.next()); // { done: false, value: 1}
g.throw('foo');
console.log(g.next()); // { done: false, value: 3} 
```
在这个例子中，生成器在 `try/catch` 块中的 yield 关键字处暂停执行。在暂停期间，throw()方
法向生成器对象内部注入了一个错误：字符串"foo"。这个错误会被 yield 关键字抛出。因为错误是在
生成器的 try/catch 块中抛出的，所以仍然在生成器内部被捕获。

可是，由于 yield 抛出了那个错误，生成器就不会再产出值 2。此时，生成器函数继续执行，在下一次迭代再次遇到 yield 关键字时产出了值 3。
### [四、生成器作为默认迭代器](#)
因为生成器对象实现了 **Iterable** 接口，而且生成器函数和默认迭代器被调用之后都产生迭代器，
所以生成器格外适合作为默认迭代器。下面是一个简单的例子，这个类的默认迭代器可以用一行代码产
出类的内容：

```javascript
class Counter {
    constructor(limit) {
        this.limit = limit;
    }
    *[Symbol.iterator]() {
        let count = 1, limit = this.limit;
        while (count <= limit){
            yield count;
            count++;
        }
    }
}

let counter = new Counter(5);

for (const element of counter) {
    console.log(element);
}
```
