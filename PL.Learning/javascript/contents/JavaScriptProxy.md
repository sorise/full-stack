## [Javasript 代理和反射](#)
> **介绍** 在JavaScript中，"代理"（Proxy）和"反射"（Reflect）是ES6引入的两个新特性，它们共同提供了一种更安全、更灵活的方式来拦截并定义对象的基本操作。

---

- [1. 代理 Proxy](#1-代理-proxy)
----

### [1. 代理 Proxy](#)
Proxy 对象用于定义自定义行为（也称为**拦截器或陷阱**）以处理对其他对象的基本操作（如属性查找、赋值、枚举、函数调用等）。通过使用new Proxy()构造函数，可以创建一个代理对象，该对象可以拦截对目标对象的各种操作，并允许你定义如何响应这些操作。

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
最简单的代理是空代理，即除了作为一个抽象的目标对象，什么也不做。默认情况下，在代理对象上执行的所有操作都会无障碍地传播到目标对象。

代理是使用 Proxy 构造函数创建的。这个构造函数接收两个参数：目标对象和处理程序对象,缺少其中任何一个参数都会抛出 TypeError。

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
**Proxy.prototype 是 undefined**,因此不能使用 instanceof 操作符。

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

#### [1.3 捕获器参数和反射 API](#)
所有捕获器都可以访问相应的参数，基于这些参数可以重建被捕获方法的原始行为。

### [2. 反射 Reflect](#)
Reflect 是一个内置对象，它提供了许多与代理相同的静态方法，用于直接操作对象，但没有代理的拦截功能。这些方法包括但不限于get(), set(), construct(), deleteProperty()等。使用Reflect方法可以更容易地实现函数式编程风格，并且在某些情况下，这些方法的行为比直接使用语言的运算符更加一致。