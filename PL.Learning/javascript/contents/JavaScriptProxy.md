## [JavaScript 代理和反射](#)
> **介绍** 在JavaScript中，"代理"（Proxy）和"反射"（Reflect）是ES6引入的两个新特性，它们共同提供了一种更安全、更灵活的方式来拦截并定义对象的基本操作。

---

- [1. 代理 Proxy](#1-代理-proxy)
- [2. 捕获器参数和反射 API](#2-捕获器参数和反射-api)
- [3. 反射 Reflect](#3-反射-reflect)
- [4. 代理模式](#4-代理模式)

----

### [1. 代理 Proxy](#)
[Proxy](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy) 对象用于定
义自定义行为（也称为**拦截器或陷阱**）以处理对其他对象的基本操作（如属性查找、赋值、枚举、函数调用等）。
通过使用new Proxy()构造函数，可以创建一个代理对象，该对象可以拦截对目标对象的各种操作，并允许你定义如何响应这些操作。

```javascript
const p = new Proxy(target, handler)
```
> 代理是目标对象的抽象,代理类似 C++指针，因为它可以用作目标对象的替身，但又完全独立于目标对象。

```javascript
let handler = {
    get(target, prop, receiver) {
        return Reflect.get(target, prop, receiver);
    },
    set(target, prop, value, receiver) {
        console.log(`op: set ${receiver.__proto__.constructor.name}[${prop}] = ${value}`);
        target[prop] = value;
        return true;
    }
};

let target = {};
let proxy = new Proxy(target, handler);

proxy.name = 'John'; // 触发set陷阱
console.log(proxy.name); // 触发get陷阱，输出 "John"
```
在这个例子中，handler对象定义了当尝试获取或设置proxy对象上的属性时应执行的操作。target是我们想要代理的实际对象，而proxy则是我们通过new Proxy()创建的代理对象。

#### [1.1 创建空代理](#)
最简单的代理是空代理，即除了作为一个抽象的目标对象，什么也不做。默认情况下，在代理对象上执行的所有操作都会无障碍地传播到
目标对象。

代理是使用 **Proxy** 构造函数创建的。这个构造函数接收两个参数：目标对象和处理程序对象,缺少其中任何一个参数都会抛出 **TypeError**。

```javascript
const target = {
    id: 'target'
};
const handler = {};
const proxy = new Proxy(target, handler);
// id 属性会访问同一个值
console.log(target.id); // target
console.log(proxy.id); // target 

// 给目标属性赋值会反映在两个对象上
// 因为两个对象访问的是同一个值
target.id = 'foo';
console.log(target.id); // foo
console.log(proxy.id); // foo 

// Proxy.prototype 是 undefined
// 因此不能使用 instanceof 操作符
console.log(target instanceof Proxy); // TypeError: Function has non-object prototype
//'undefined' in instanceof check
console.log(proxy instanceof Proxy); // TypeError: Function has non-object prototype
//'undefined' in instanceof check 
```
**Proxy.prototype 是 undefined**,因此不能使用 **instanceof** 操作符。

**严格相等可以用来区分代理和目标**
```javascript
console.log(target === proxy); // false
```

#### [1.2 定义捕获器](#)
使用代理的主要目的是可以定义捕获器（trap）。捕获器就是在处理程序对象中定义的 **“基本操作的拦截器”** 。

> 每个处理程序对象可以包含零个或多个捕获器，每个捕获器都对应一种基本操作，可以直接
或间接在代理对象上调用。每次在代理对象上调用这些基本操作时，代理可以在这些操作传播到目标对
象之前先调用捕获器函数，从而拦截并修改相应的行为。

例如，可以定义一个 get()捕获器，在 ECMAScript 操作以某种形式调用 get()时触发。下面的例
子定义了一个 get()捕获器：
```javascript
const target = {
    foo: 'bar'
};
let target_proxy = new Proxy(target, {
    get(target, prop, receiver) {
        return "no get!";
    }
});
console.log(target_proxy.foo);//no get!
```
`proxy[property]`、`proxy.property` 或 `Object.create(proxy)[property]` 等操作都会触发基本的 get()操作以获取属性。

### [2. 捕获器参数和反射 API](#)
handler 对象是一个容纳一批特定属性的占位符对象。它包含有 Proxy 的各个捕获器（trap）。
所有捕获器都可以访问相应的参数，基于这些参数可以重建被捕获方法的原始行为。

代理可以捕获13 种不同的基本操作，代理对象上执行的任一种操作只会有一种捕获处理程序被调用，不存在重复捕获现象
只要在代理上操作，所有捕获器都会拦截对应的反射 API 操作。
#### [2.1 get()](#)
在获取属性值的操作中被调用，对应的反射 API 方法为 `Reflect.get(target, property, receiver)` 。
* 返回值
  * 无限制
* 拦截的操作
  * proxy.property
  * proxy[property]
  * Object.create(proxy)[property]
  * Reflect.get(proxy,property,receiver)
* 捕获器处理程序参数
  * target：目标对象
  * property：目标对象的键属性（字符串键或符号键）
  * receiver：代理对象或继承代理对象的对象
* 捕获器不变式
  * 如果target.property不可写且不可配，处理程序返回值必须与target.property匹配
  * 如果target.property不可配置且[[Get]]特性为undefined，处理程序返回值必须是undefined

```javascript
const myTarget = {
  foo: 'bar',
}
const proxy = new Proxy(myTarget, {
  get(target, property, receiver) {
    console.log(target) // { foo: 'bar' }，目标对象
    console.log(property) // 'foo'，目标对象键属性
    console.log(receiver) // { foo: 'bar' }，代理对象
    return Reflect.get(...arguments)
  },
})
console.log(proxy.foo) // 'bar'，拦截的操作

// 捕获器不变式
Object.defineProperty(myTarget, 'foo', {
  configurable: false, // 不可配置
})
const proxy2 = new Proxy(myTarget, {
  get() {
    return 'baz' // 试图重写
  },
})
console.log(proxy2.foo) // TypeError
```

#### [2.2 set()](#)
set()捕获器会在设置属性值的操作中被调用。对应的反射 API 方法为 Reflect.set(proxy, property, value, receiver)。

```javascript
const myTarget = {};
const proxy = new Proxy(myTarget, {
  set(target, property, value, receiver) {
    console.log('set()');
    return Reflect.set(...arguments)
  }
});
proxy.foo = 'bar';
// set() 
```
* 返回值
  * 返回 `true` 表示成功；返回 `false` 表示失败，严格模式下会抛出 **TypeError**。
* 拦截的操作
  * proxy.property = value
  * proxy[property] = value
  * Object.create(proxy)[property] = value
* 捕获器处理程序参数
  * target：目标对象。
  * property：引用的目标对象上的字符串键属性。
  * value：要赋给属性的值。
  * receiver：接收最初赋值的对象。
* **捕获器不变式（约束）**
  * 如果 target.property 不可写且不可配置，则不能修改目标属性的值。
  * 如果 target.property 不可配置且 `[[Set]]` 特性为 undefined，则不能修改目标属性的值。在严格模式下，处理程序中返回 false 会抛出 TypeError。

#### [2.3 has()](#)
`has()` 捕获器会在 `in` 操作符中被调用。对应的反射 API 方法为 `Reflect.has()`。

```js
const myTarget = {};

const proxy = new Proxy(myTarget, {
  has(target, property) {
    console.log('触发 has() 捕获器');
    return Reflect.has(...arguments);
  },
});

'foo' in proxy;
// 触发 has() 捕获器
```

* **返回值**
`has()` 必须返回布尔值，表示属性是否存在。返回非布尔值会被转型为布尔值。
* 触发拦截的操作
  - [ ] `property in proxy`
  - [ ] `property in Object.create(proxy)`
  - [ ] `with(proxy) {(property);}`
  - [ ] `Reflect.has(proxy, property)`
* 捕获器处理程序参数
  - [ ] `target`：目标对象。
  - [ ] `property`：引用的目标对象上的字符串键属性。
* **捕获器不变式（约束）**
  * 如果 `target.property` 存在且不可配置，则处理程序必须返回 `true`。
  * 如果 `target.property` 存在且目标对象不可扩展，则处理程序必须返回 `true`。

#### [2.4 defineProperty()](#)
`defineProperty()` 捕获器会在 `Object.defineProperty()` 中被调用。
对应的反射 API 方法为 `Reflect.defineProperty(target,property,descriptor)`。

```js
const myTarget = {};

const proxy = new Proxy(myTarget, {
  defineProperty(target, property, descriptor) {
    console.log('触发 defineProperty() 捕获器');
    return Reflect.defineProperty(...arguments);
  },
});

Object.defineProperty(proxy, 'foo', { value: 'bar' });
// 触发 defineProperty() 捕获器
```

* **返回值**
`defineProperty()` 必须返回布尔值，表示属性是否成功定义。返回非布尔值会被转型为布尔值。
* **触发拦截的操作**
  - [ ] `Object.defineProperty(proxy, property, descriptor)`
  - [ ] `Reflect.defineProperty(proxy, property, descriptor)`
* **捕获器处理程序参数**
  - [ ] `target`：目标对象。
  - [ ] `property`：引用的目标对象上的字符串键属性。
  - [ ] `descriptor`：包含可选的 `enumerable`、`configurable`、`writable`、`value`、`get` 和 `set` 定义的对象。
* **捕获器不变式（约束）**
  * 如果目标对象不可扩展，则无法定义属性。
  * 如果目标对象有一个可配置的属性，则不能添加同名的不可配置属性。
  * 如果目标对象有一个不可配置的属性，则不能添加同名的可配置属性。

#### [2.5 getOwnPropertyDescriptor()](#)
`getOwnPropertyDescriptor()` 捕获器会在 `Object.getOwnPropertyDescriptor()` 中被调用。对应的反射 API 方法为 `Reflect.getOwnPropertyDescriptor()`。

```js
const myTarget = {};

const proxy = new Proxy(myTarget, {
  getOwnPropertyDescriptor(target, property) {
    console.log('触发 getOwnPropertyDescriptor() 捕获器');
    return Reflect.getOwnPropertyDescriptor(...arguments);
  },
});

Object.getOwnPropertyDescriptor(proxy, 'foo');
// 触发 getOwnPropertyDescriptor() 捕获器
```

* **返回值**
`getOwnPropertyDescriptor()` 必须返回对象，或者在属性不存在时返回 `undefined`。
* **触发拦截的操作**
  - [ ] `Object.getOwnPropertyDescriptor(proxy, property)`
  - [ ] `Reflect.getOwnPropertyDescriptor(proxy, property)`
* **捕获器处理程序参数**
  - [ ] `target`：目标对象。
  - [ ] `property`：引用的目标对象上的字符串键属性。
* **捕获器不变式**（约束）
  * 如果自有的 `target.property` 存在且不可配置，则处理程序必须返回一个表示该属性存在的对象。
  * 如果自有的 `target.property` 存在且可配置，则处理程序必须返回表示该属性可配置的对象。
  * 如果自有的 `target.property` 存在且 `target` 不可扩展，则处理程序必须返回一个表示该属性存在的对象。
  * 如果 `target.property` 不存在且 `target` 不可扩展，则处理程序必须返回 `undefined` 表示该属性不存在。
  * 如果 `target.property` 不存在，则处理程序不能返回表示该属性可配置的对象。

#### [2.6 deleteProperty()](#)
`deletProperty()` 捕获器会在 `delete` 操作符中被调用。对应的反射 API 方法为 `Reflect.deleteProperty()`。

```js
const myTarget = {};

const proxy = new Proxy(myTarget, {
  deleteProperty(target, property) {
    console.log('触发 deleteProperty() 捕获器');
    return Reflect.deleteProperty(...arguments);
  },
});

delete proxy.foo;
// 触发 deleteProperty() 捕获器
```

**返回值** `deleteProperty()` 必须返回布尔值，表示删除属性是否成功。返回非布尔值会被转型为布尔值。

**触发拦截的操作**
- [ ] `delete proxy.property`
- [ ] `delete proxy[property]`
- [ ] `Reflect.deleteProperty(proxy, property)`

**捕获器处理程序参数**
- [ ] `target`：目标对象。
- [ ] `property`：引用的目标对象上的字符串键属性。

**捕获器不变式（约束）**
如果自有的 `target.property` 存在且不可配置，则处理程序不能删除这个属性。

#### [2.7 ownKeys()](#)
`ownKeys()` 捕获器会在 `Object.keys()` 及类似方法中被调用。对应的反射 API 方法为 `Reflect.ownKeys()`。

```js
const myTarget = {};

const proxy = new Proxy(myTarget, {
  ownKeys(target) {
    console.log('触发 ownKeys() 捕获器');
    return Reflect.ownKeys(...arguments);
  },
});

Object.keys(proxy);
// 触发 ownKeys() 捕获器
```

**返回值**
`ownKeys()` 必须返回包含字符串或符号的可枚举对象。

**触发拦截的操作**
- [ ] `Object.getOwnPropertyNames(proxy)`
- [ ] `Object.getOwnPropertySymbols(proxy)`
- [ ] `Object.keys(proxy)`
- [ ] `Reflect.ownKeys(proxy)`

**捕获器处理程序参数**
- [ ] `target`：目标对象。

**捕获器不变式**（约束）

返回的可枚举对象必须包含 `target` 的所有不可配置的自有属性。

如果 `target` 不可扩展，则返回可枚举对象必须准确地包含自有属性键。

#### [2.8 getPrototypeOf()](#)
`getPrototypeOf()` 捕获器会在 `Object.getPrototypeOf()` 中被调用。对应的反射 API 方法为 `Reflect.getPrototypeOf()`。

```js
const myTarget = {};

const proxy = new Proxy(myTarget, {
  getPrototypeOf(target) {
    console.log('触发 getPrototypeOf() 捕获器');
    return Reflect.getPrototypeOf(...arguments);
  },
});

Object.getPrototypeOf(proxy);
// 触发 getPrototypeOf() 捕获器
```

**返回值** `getPrototypeOf()` 必须返回对象或 `null`。

**触发拦截的操作**
- [ ] `Object.getPrototypeOf(proxy)`
- [ ] `Reflect.getPrototypeOf(proxy)`
- [ ] `proxy.__proto__`
- [ ] `Object.prototype.isPrototypeOf(proxy)`
- [ ] `proxy instanceof Object`

**捕获器处理程序参数**
- [ ] `target`：目标对象。

**捕获器不变式（约束）**
如果 `target` 不可扩展，则 `Object.getPrototypeOf(proxy)` 唯一有效的返回值就是 `Object.getPrototypeOf(target)` 的返回值。

#### [2.9 setPrototypeOf()](#)
`setPrototypeOf()` 捕获器会在 `Object.setPrototypeOf()` 中被调用。对应的反射 API 方法为 `Reflect.setPrototypeOf()`。

```js
const myTarget = {};

const proxy = new Proxy(myTarget, {
  setPrototypeOf(target, prototype) {
    console.log('触发 setPrototypeOf() 捕获器');
    return Reflect.setPrototypeOf(...arguments);
  },
});

Object.setPrototypeOf(proxy, Object);
// 触发 setPrototypeOf() 捕获器
```

**返回值** setPrototypeOf()必须返回布尔值，表示原型赋值是否成功。返回非布尔值会被转型为布尔值。

**触发拦截的操作**
- [ ] `Object.setPrototypeOf(proxy)`
- [ ] `Reflect.setPrototypeOf(proxy)`

**捕获器处理程序参数**
- [ ] `target`：目标对象。
- [ ] `prototype`：target 的替代原型，如果是顶级原型则为 `null`。

**捕获器不变式**（约束）
如果 `target` 不可扩展，则唯一有效的 `prototype` 参数就是 `Object.getPrototypeOf(target)` 的返回值。

#### [2.10 isExtensible()](#)
`isExtensible()` 捕获器会在 `Object.isExtensible()` 中被调用。对应的反射 API 方法为 `Reflect.isExtensible()`。

```js
const myTarget = {};

const proxy = new Proxy(myTarget, {
  isExtensible(target) {
    console.log('触发 isExtensible() 捕获器');
    return Reflect.isExtensible(...arguments);
  },
});

Object.isExtensible(proxy);
// 触发 isExtensible() 捕获器
```

**返回值** `isExtensible()` 必须返回布尔值，表示 `target` 是否可扩展。返回非布尔值会被转型为布尔值。

**触发拦截的操作**
- [ ] `Object.isExtensible(proxy)`
- [ ] `Reflect.isExtensible(proxy)`

**捕获器处理程序参数**
- [ ] `target`：目标对象。

**捕获器不变式**（约束）
* 如果 `target` 可扩展，则处理程序必须返回 `true`。
* 如果 `target` 不可扩展，则处理程序必须返回 `false`。

#### [2.11 preventExtensions()](#) 
`preventExtensions()` 捕获器会在 `Object.preventExtensions()` 中被调用。对应的反射 API方法为 `Reflect.preventExtensions()`。

```js
const myTarget = {};

const proxy = new Proxy(myTarget, {
  preventExtensions(target) {
    console.log('触发 preventExtensions() 捕获器');
    return Reflect.preventExtensions(...arguments);
  },
});

Object.preventExtensions(proxy);
// 触发 preventExtensions() 捕获器
```

**返回值** `preventExtensions()` 必须返回布尔值，表示 `target` 是否已经不可扩展。返回非布尔值会被转型为布尔值。

**触发拦截的操作**
- [ ] `Object.preventExtensions(proxy)`
- [ ] `Reflect.preventExtensions(proxy)`

**捕获器处理程序参数**
- [ ] `target`：目标对象。

**捕获器不变式**（约束）  如果 `Object.isExtensible(proxy)` 是 false，则处理程序必须返回 true。

#### [2.12 apply()](#) 
`apply()` 捕获器会在调用函数时中被调用。对应的反射 API 方法为 `Reflect.apply()`。

```js
const myTarget = () => {};

const proxy = new Proxy(myTarget, {
  apply(target, thisArg, ...argumentsList) {
    console.log('触发 apply() 捕获器');
    return Reflect.apply(...arguments);
  },
});

proxy();
// 触发 apply() 捕获器
```

**返回值** `apply` 方法可以返回任何值。

**触发拦截的操作**
- [ ] `proxy(...argumentsList)`
- [ ] `Function.prototype.apply(thisArg, argumentsList)`
- [ ] `Function.prototype.call(thisArg, ...argumentsList)`
- [ ] `Reflect.apply(target, thisArgument, argumentsList)`

**捕获器处理程序参数**
- [ ] `target`：目标对象。
- [ ] `thisArg`：调用函数时的 this 参数。
- [ ] `argumentsList`：调用函数时的参数列表

**捕获器不变式**（约束） `target` 必须是一个函数对象。

#### [2.13 construct()](#) 
`construct()` 捕获器会在 `new` 操作符中被调用。对应的反射 API 方法为 `Reflect.construct()`。

```js
const myTarget = function () {};

const proxy = new Proxy(myTarget, {
  construct(target, argumentsList, newTarget) {
    console.log('触发 construct() 捕获器');
    return Reflect.construct(...arguments);
  },
});

new proxy();
// 触发 construct() 捕获器
```
**返回值** `construct()` 必须返回一个对象。

**触发拦截的操作**
- [ ] `new proxy(...argumentsList)`
- [ ] `Reflect.construct(target, argumentsList, newTarget)`

**捕获器处理程序参数**
- [ ] `target`：目标构造函数。
- [ ] `argumentsList`：传给目标构造函数的参数列表。
- [ ] `newTarget`：最初被调用的构造函数。

**捕获器不变式**（约束）
`target` 必须可以用作构造函数。

### [3. 反射 Reflect](#)
[Reflect](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect) 是一个内置对象，它提供了许多与代理相同的静态方法，用于直接操作对象，但没有代理的拦截功能。这些方法包括但不限于get(), set(), construct(), deleteProperty()等。使用Reflect方法可以更容易地实现函数式编程风格，并且在某些情况下，这些方法的行为比直接使用语言的运算符更加一致。

```javascript
const user = {
  name: 'Jake'
};

const proxy = new Proxy(user, {
  get(target, property, receiver) {
    console.log(`Getting ${property}`);
    return Reflect.get(...arguments);
  },
  set(target, property, value, receiver) {
    console.log(`Setting ${property}=${value}`);
    return Reflect.set(...arguments);
  }
});

proxy.name; // Getting name
proxy.age = 27; // Setting age=27 
```

### [4. 代理模式](#)
使用代理可以在代码中实现一些有用的编程模式。

#### [4.1 跟踪属性访问](#)
通过捕获 `get`、`set` 和 `has` 等操作，可以知道对象属性什么时候被访问、被查询。把实现相应捕获
器的某个对象代理放到应用中，可以监控这个对象何时在何处被访问过：

```javascript
const user = {
 name: 'Jake'
};

const proxy = new Proxy(user, {
 get(target, property, receiver) {
   console.log(`Getting ${property}`);
   return Reflect.get(...arguments);
 },
 set(target, property, value, receiver) {
   console.log(`Setting ${property}=${value}`);
   return Reflect.set(...arguments);
 }
});

proxy.name; // Getting name
proxy.age = 27; // Setting age=27
```

#### [4.2 隐藏属性](#)
代理的内部实现对外部代码是不可见的，因此要隐藏目标对象上的属性也轻而易举。

```javascript
const hiddenProperties = ['foo', 'bar'];

const targetObject = {
    foo: 1,
    bar: 2,
    baz: 3
};

const proxy = new Proxy(targetObject, {
  get(target, property) {
      if (hiddenProperties.includes(property)) {
        return undefined;
      } else {
        return Reflect.get(...arguments);
      }
  },
  has(target, property) {
    if (hiddenProperties.includes(property)) {
      return false;
    }else{
      return Reflect.has(...arguments);
    }
  }
});
// get()
console.log(proxy.foo); // undefined
console.log(proxy.bar); // undefined
console.log(proxy.baz); // 3

// has()
console.log('foo' in proxy); // false
console.log('bar' in proxy); // false
console.log('baz' in proxy); // true 
```

#### [4.3 属性验证](#)
因为所有赋值操作都会触发 set()捕获器，所以可以根据所赋的值决定是允许还是拒绝赋值：

```javascript
const target = {
  onlyNumbersGoHere: 0
};

const proxy = new Proxy(target, {
  set(target, property, value) {
    if (typeof value !== 'number') {
      return false;
    } else {
      return Reflect.set(...arguments);
    }
  }
});

proxy.onlyNumbersGoHere = 1;
console.log(proxy.onlyNumbersGoHere); // 1

proxy.onlyNumbersGoHere = '2';
console.log(proxy.onlyNumbersGoHere); // 1 
```

#### [4.4 函数与构造函数参数验证](#)
跟保护和验证对象属性类似，也可对函数和构造函数参数进行审查。比如，可以让函数只接收某种类型的值：

```javascript
function median(...nums) {
  return nums.sort()[Math.floor(nums.length / 2)];
}

const proxy = new Proxy(median, {
  apply(target, thisArg, argumentsList) {
    for (const arg of argumentsList) {
      if (typeof arg !== 'number') {
        throw 'Non-number argument provided';
      }
    }
    return Reflect.apply(...arguments);
  }
});

console.log(proxy(4, 7, 1)); // 4
console.log(proxy(4, '7', 1));
// Error: Non-number argument provided
```
类似地，可以要求实例化时必须给构造函数传参：
```javascript
class User {
  constructor(id) {
    this.id_ = id;
  }
}

const proxy = new Proxy(User, {
  construct(target, argumentsList, newTarget) {
    if (argumentsList[0] === undefined) {
      throw 'User cannot be instantiated without id';
    } else {
      return Reflect.construct(...arguments);
    }
  }
});

new proxy(1);
new proxy();
// Error: User cannot be instantiated without id 
```

#### [4.5 数据绑定与可观察对象](#)
通过代理可以把运行时中原本不相关的部分联系到一起。这样就可以实现各种模式，从而让不同的代码互操作。

比如，可以将被代理的类绑定到一个全局实例集合，让所有创建的实例都被添加到这个集合中：
```javascript
const userList = [];

class User {
  constructor(name) {
    this.name_ = name;
  }
}

const proxy = new Proxy(User, {
  construct() {
    const newUser = Reflect.construct(...arguments);
    userList.push(newUser);
    return newUser;
  }
});

new proxy('John');
new proxy('Jacob');
new proxy('Jingleheimerschmidt');

console.log(userList); // [User {}, User {}, User{}] 
```
另外，还可以把集合绑定到一个事件分派程序，每次插入新实例时都会发送消息：
```javascript
const userList = [];

function emit(newValue) {
    console.log(newValue);
}

const proxy = new Proxy(userList, {
  set(target, property, value, receiver) {
    const result = Reflect.set(...arguments);
    if (result) {
        emit(Reflect.get(target, property, receiver));
    }
    return result;
  }
});

proxy.push('John');
// John
proxy.push('Jacob');
// Jacob 
```