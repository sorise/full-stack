### [JavaScript Class](#)
>**介绍**：ECMAScript 6 新引入的 class 关键字具有正式定义类的能力。类（class）是ECMAScript 中新的基础性语法糖结构。

-----
- [1. 类定义](#1-类定义)
- [2. 继承](#2-继承)
- [3. 访问性之私有化](#3-访问性之私有化)

-----

### [1. 类定义](#)
[类](https://mdn.org.cn/en-US/docs/Web/JavaScript/Reference/Classes)，它是创建对象的模板。它们将数据与处理这些数据的代码封装在一起。JS 中的类构建在原型之上，但也具有一些类独有的语法和语义。类实际上是“特殊的 函数”，就像你可以定义函数表达式和函数声明一样，类也可以通过两种方式定义：`类表达式`或`类声明`，这两种方式都使用 `class` 关键字加大括号。
```javascript
// 类声明
class Person {}
// 类表达式
const Animal = class {}; 
```
与函数表达式类似，类表达式在它们被求值前也不能引用。不过，与函数定义不同的是，虽然函数声明可以提升，**但类定义不能**。

类的体是位于花括号 `{}` 中的部分。在这里，您定义类成员，例如方法或构造函数。

类的体在 严格模式 中执行，即使没有 `"use strict"` 指令。

类元素可以由三个方面来描述
* **类型**：getter、setter、方法或字段
* **位置**：静态或实例
* **可见性**：公共或私有

也可以详细一些划分为：
- **constructor** 构造函数
- **私有属性** 字段、getter、setter
- **公共类字段**  
- **静态** 为类定义静态方法或字段，或静态初始化块
- **extends、super**


#### [1.1 类的成员](#)
类可以包含**构造函数方法**、**实例方法**、**get、set属性**、**字段**、**私有属性**和 **静态类方法、字段**，但这些都不是必需的,空的类定义照样有效。

```javascript
class Point {
    //构造函数
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  static displayName = "Point";
  
  static distance(a, b) {
    const dx = a.x - b.x;
    const dy = a.y - b.y;

    return Math.hypot(dx, dy);
  }
}

console.log(Point.displayName);

let a = new Point(0,0);
let b = new Point(10,10);

console.log(Point.distance(a, b));
```

#### [1.2 类构造函数](#)
`constructor` 关键字用于在类定义块内部创建类的构造函数。方法名 constructor 会告诉解释器
在使用 new 操作符创建类的新实例时，应该调用这个函数。构造函数的定义不是必需的，不定义构造函数相当于将构造函数定义为**空函数**。

> 在一个类中只能有一个名为“constructor”的特殊方法 - 如果类包含多个 constructor 方法，则会抛出 SyntaxError。

使用 new 操作符实例化 Person 的操作等于使用 new 调用其构造函数。唯一可感知的不同之处就
是，JavaScript 解释器知道使用 new 和类意味着应该使用 constructor 函数进行实例化。
使用 new 调用类的构造函数会执行如下操作。

1. 在内存中创建一个新对象。  
2. 这个新对象内部的 `[[Prototype]]` 指针被赋值为构造函数的 `prototype` 属性。  
3. 构造函数内部的 `this` 被赋值为这个新对象（即 `this` 指向新对象）。  
4. 执行构造函数内部的代码（给新对象添加属性）。  
5. 如果构造函数返回非空对象，则返回该对象；否则，返回刚创建的新对象。  

```javascript
class Person {
    constructor(_name, _age, _sex) {
        this.name = _name;
        this.age = _age;
        this.sex = _sex;
    }
}
```
构造函数可以使用 [super](https://mdn.org.cn/en-US/docs/Web/JavaScript/Reference/Operators/super) 关键字调用超类的构造函数，可以在构造函数中创建实例属性。
```javascript
class Foo {
  constructor(name) {
    this.name = name;
  }
  getNameSeparator() {
    return '-';
  }
}

class FooBar extends Foo {
  constructor(name, index) {
    super(name);
    this.index = index;
  }
  getFullName() {
    return this.name + super.getNameSeparator() + this.index;
  }
}
```
#### [1.3 .把类当成特殊函数](#)
从各方面来看，ECMAScript 类就是一种特殊函数。声明一个类之后，通过 typeof 操作符检测类标识符，表明它是一个函数：
```javascript
class Person {}
console.log(Person); // class Person {}
console.log(typeof Person); // function
```
类标识符有 prototype 属性，而这个原型也有一个 constructor 属性指向类自身：
```javascript
class Person{}
console.log(Person.prototype); // { constructor: f() }
console.log(Person === Person.prototype.constructor); // true
```
与普通构造函数一样，可以使用 instanceof 操作符检查构造函数原型是否存在于实例的原型链中：
```javascript
class Person {}
let p = new Person();
console.log(p instanceof Person); // true 
```
> 重点在于，类中定义的 constructor 方法不会被当成构造函数，在对它使用instanceof 操作符时会返回 false。但

**类是 JavaScript 的一等公民，因此可以像其他对象或函数引用一样把类作为参数传递**：

```javascript
// 类可以像函数一样在任何地方定义，比如在数组中
let classList = [
 class {
     constructor(id) {
        this.id_ = id;
        console.log(`instance ${this.id_}`);
     }
 }];

function createInstance(classDefinition, id) {
    return new classDefinition(id);
}

let foo = createInstance(classList[0], 3141); // instance 3141 
```
与立即调用函数表达式相似，类也可以立即实例化：
```javascript
// 因为是一个类表达式，所以类名是可选的
let p = new class Foo {
    constructor(x) {
        console.log(x);
        this.tick = x;
    }
}('bar'); // bar


console.log(p); // Foo {} 

/*
bar
Foo { tick: 'bar' }
*/

```
#### [1.4 实例成员](#)
每次通过new调用类标识符时，都会执行类构造函数。在这个函数内部，可以为新创建的实例（this）添加“自有”属性。

至于添加什么样的属性，则没有限制。另外，在构造函数执行完毕后，仍然可以给实例继续添加新成员。

```javascript
class Person {
 constructor() {
     // 这个例子先使用对象包装类型定义一个字符串
     // 为的是在下面测试两个对象的相等性
     this.name = new String('Jack');
     this.sayName = () => console.log(this.name);
     this.nicknames = ['Jake', 'J-Dog']
 }
}

let p1 = new Person(), p2 = new Person();

p1.sayName(); // Jack
p2.sayName(); // Jack

console.log(p1.name === p2.name); // false
console.log(p1.sayName === p2.sayName); // false
console.log(p1.nicknames === p2.nicknames); // false
p1.name = p1.nicknames[0];
p2.name = p2.nicknames[1];

p1.sayName(); // Jake
p2.sayName(); // J-Dog 
```
#### [1.5 方法](#)
为了在实例间共享方法，类定义语法把在类块中定义的方法作为原型方法。

```javascript
class Person {
    constructor() {
        // 添加到 this 的所有内容都会存在于不同的实例上
        this.locate = () => console.log('instance');
    }
    // 在类块中定义的所有内容都会定义在类的原型上
    locate() {
    console.log('prototype');
    }
}

let p = new Person();

p.locate(); // instance
Person.prototype.locate(); // prototype 
```
**可以把方法定义在类构造函数中或者类块中，但不能在类块中给原型添加原始值或对象作为成员数据**：

```javascript
class Person {
 name: 'Jake'
}
// Uncaught SyntaxError: Unexpected token
```
类方法等同于对象属性，因此可以使用字符串、符号或计算的值作为键：
```javascript
const symbolKey = Symbol('symbolKey');

class Person {
     stringKey() {
         console.log('invoked stringKey');
     }
     [symbolKey]() {
         console.log('invoked symbolKey');
     }
     ['computed' + 'Key']() {
         console.log('invoked computedKey');
     }
}

let p = new Person();
p.stringKey(); // invoked stringKey
p[symbolKey](); // invoked symbolKey
p.computedKey(); // invoked computedKey 
```

#### [1.6 访问器](#)
类定义也支持获取和设置访问器。语法与行为跟普通对象一样：

**get** 语法将对象属性绑定到一个函数，该函数将在查找该属性时被调用。它也可用于类。
```
{ get prop() { /* … */ } }
{ get [expression]() { /* … */ } }
```
**set** 语法将对象属性绑定到一个函数，当尝试设置该属性时将调用该函数。它也可以在 类 中使用。
```
{ set prop(val) { /* … */ } }
{ set [expression](val) { /* … */ } }
```
**例子**：
```javascript
const obj = {
  log: ['a', 'b', 'c'],
  get latest() {
    return this.log[this.log.length - 1];
  },
};

console.log(obj.latest);
// Expected output: "c"

class Person {
    set name(newName) {
        this.name_ = newName;
    }
    get name() {
        return this.name_;
    }
}

const language = {
    set current(name) {
        this.log.push(name);
    },
    log: [],
};

language.current = 'EN';
language.current = 'FA';

console.log(language.log);
// Expected output: Array ["EN", "FA"]
```

#### [1.7 类体内部定义字段和构造函数内定义字段](#)
将字段定义在类中的方式有两种：一种是在类体内部直接定义字段，另一种是在构造函数（constructor）内定义字段。这两种方式有一些区别：

**类体内部定义字段**： 
* 这种方式是 ES2022 引入的类字段（Class Fields）提案的一部分，允许你在类的主体中直接定义实例字段，而不需要通过构造函数来初始化它们。
* 类字段可以有初始值，并且可以在构造函数执行之前进行访问或修改。
* **类字段的定义在编译时会被提升至构造函数内，但是不会像函数声明那样提升到作用域顶部**。
* 类字段提供了更简洁的语法来定义静态属性和实例属性。

```javascript
class MyClass {
  myField = 'Hello, World!';
  
  constructor() {
    console.log(this.myField); // 输出 'Hello, World!'
  }
}

const obj = new MyClass();
```
**构造函数内定义字段**： 
* 在构造函数中定义字段是一种更传统的做法，它需要在每次构造函数调用时都执行赋值操作。
* 字段必须在构造函数内部初始化，除非它们是静态字段或者是继承自原型链的。
* 构造函数内的字段定义在编译时会被转换成对 this 对象的属性赋值。

```javascript
class MyClass {
  constructor() {
    this.myField = 'Hello, World!';
    console.log(this.myField); // 输出 'Hello, World!'
  }
}

const obj = new MyClass();
```

#### [1.8 静态类方法](#)
可以在类上定义静态方法。这些方法通常用于执行不特定于实例的操作，也不要求存在类的实例。

静态类成员在类定义中使用 static 关键字作为前缀。在静态成员中，this 引用类自身。其他所有约定跟原型成员一样：

```javascript
class Person {
 constructor() {
 // 添加到 this 的所有内容都会存在于不同的实例上
     this.locate = () => console.log('instance', this);
 }
 // 定义在类的原型对象上
 locate() {
     console.log('prototype', this);
 }
 // 定义在类本身上
 static locate() {
     console.log('class', this);
 }
} 
```
静态类方法非常适合作为实例工厂：
```javascript
class Person {
    constructor(age) {
        this.age_ = age;
    }
    sayAge() {
        console.log(this.age_);
    }
    static create() {
        // 使用随机年龄创建并返回一个 Person 实例
        return new Person(Math.floor(Math.random()*100));
    }
}
console.log(Person.create()); // Person { age_: ... }

```
#### [1.9 静态初始化块](#)
允许灵活地初始化**静态属性**，包括在初始化期间评估语句，同时授予对私有范围的访问权限。

可以声明多个静态块，这些块可以与静态字段和方法的声明交织在一起（所有静态项都按声明顺序进行评估）。

```javascript
class ClassWithStaticInitializationBlock {
  static staticProperty1 = 'Property 1';
  static staticProperty2;
  static {
    this.staticProperty2 = 'Property 2';
  }
}

console.log(ClassWithStaticInitializationBlock.staticProperty1);
// Expected output: "Property 1"
console.log(ClassWithStaticInitializationBlock.staticProperty2);
// Expected output: "Property 2"
```

#### [1.10 迭代器与生成器方法](#)
类定义语法支持在原型和类本身上定义生成器方法：

```javascript
class Person {
    // 在原型上定义生成器方法
    *createNicknameIterator() {
        yield 'Jack';
        yield 'Jake';
        yield 'J-Dog';
    }
    // 在类上定义生成器方法
    static *createJobIterator() {
        yield 'Butcher';
        yield 'Baker';
        yield 'Candlestick maker';
    }
}

let jobIter = Person.createJobIterator();
console.log(jobIter.next().value); // Butcher
console.log(jobIter.next().value); // Baker
console.log(jobIter.next().value); // Candlestick maker

let p = new Person();
let nicknameIter = p.createNicknameIterator();
console.log(nicknameIter.next().value); // Jack
console.log(nicknameIter.next().value); // Jake
console.log(nicknameIter.next().value); // J-Dog 
```
因为支持生成器方法，所以可以通过添加一个默认的迭代器，把类实例变成可迭代对象：

```javascript
class Person {
 constructor() {
     this.nicknames = ['Jack', 'Jake', 'J-Dog'];
 }
 *[Symbol.iterator]() {
     yield *this.nicknames.entries();
 }
}

let p = new Person();
for (let [idx, nickname] of p) {
    console.log(nickname);
}
```
也可以只返回迭代器实例：
```javascript
class Person {
 constructor() {
    this.nicknames = ['Jack', 'Jake', 'J-Dog'];
 }
 [Symbol.iterator]() {
    return this.nicknames.entries();
 }
}
let p = new Person();
for (let [idx, nickname] of p) {
 console.log(nickname);
}
// Jack
// Jake
// J-Dog
```

#### [1.11 静态方法和普通方法](#)
在 JavaScript 中，class 内定义的方法可以分为两类：静态方法和实例方法（通常称为“普通方法”）。这两者的主要区别在于它们是如何被调用的以及它们访问类状态的能力。

**普通方法（实例方法）**: 普通方法是定义在类的原型上的方法，这些方法绑定到了由该类创建的对象上。这意味
着每个实例都可以访问并调用这些方法。当一个对象实例调用一个普通方法时，this 关键字指向该对象实例本身。

```javascript
class MyClass {
  constructor(value) {
    this.value = value;
  }

  instanceMethod() {
    console.log(`Value: ${this.value}`);
  }
}

const obj1 = new MyClass(10);
obj1.instanceMethod(); // 输出 "Value: 10"

const obj2 = new MyClass(20);
obj2.instanceMethod(); // 输出 "Value: 20"
```

**静态方法** 静态方法则是直接绑定到类本身，而不是它的实例。这意味着你不能通过类的实例来调用静态方法，而是需要通过类直接调用。
此外，当在一个静态方法内部使用 this 时，它指的是类本身，而不是任何特定的对象实例。

```javascript
class MyClass {
  static staticMethod() {
    console.log('This is a static method.');
  }
}

MyClass.staticMethod(); // 输出 "This is a static method."

// 下面的代码会抛出错误，因为静态方法不能通过实例来调用
const obj = new MyClass();
obj.staticMethod(); // TypeError: obj.staticMethod is not a function
```
使用场景
* 普通方法：当你需要一个方法来操作对象的状态时，应该使用普通方法。这些方法通常依赖于对象的属性或者其他方法的结果。
* 静态方法：当你有一个方法，它的逻辑并不依赖于任何特定的实例，或者你想为其他实例提供一个工具方法时，可以使用静态方法。例如，一个用于创建类的新实例的工厂方法，或者是一个用来验证某些输入是否符合预期格式的辅助方法。

### [2. 继承](#)
虽然类继承使用的是新语法，但背后依旧使用的是原型链。

ES6 类支持单继承。使用 extends 关键字，就可以继承任何拥有 `[[Construct]]` 和原型的对象。
很大程度上，这意味着不仅**可以继承一个类，也可以继承普通的构造函数（保持向后兼容）**：
```javascript
class Vehicle {}
// 继承类
class Bus extends Vehicle {}
let b = new Bus();
console.log(b instanceof Bus); // true
console.log(b instanceof Vehicle); // true 

function Person() {}
// 继承普通构造函数
class Engineer extends Person {}
let e = new Engineer();
console.log(e instanceof Engineer); // true
console.log(e instanceof Person); // true
```
派生类都会通过原型链访问到类和原型上定义的方法，this 的值会反映调用相应方法的实例或者类：

```javascript
class Vehicle {
 identifyPrototype(id) {
    console.log(id, this);
 } 
 
 static identifyClass(id) {
    console.log(id, this);
 }
}
class Bus extends Vehicle {}
let v = new Vehicle();
let b = new Bus();

b.identifyPrototype('bus'); // bus, Bus {}
v.identifyPrototype('vehicle'); // vehicle, Vehicle {}
Bus.identifyClass('bus'); // bus, class Bus {}
Vehicle.identifyClass('vehicle'); // vehicle, class Vehicle {} 
```

> extends 关键字也可以在类表达式中使用，因此 let Bar = class extends Foo {} 是有效的语法。

派生类的方法可以通过 super 关键字引用它们的原型。这个关键字只能在派生类中使用，而且仅限于类构造函数、实例方法和静态方法内部。在类构造函数中使用 super 可以调用父类构造函数。
```javascript
class Vehicle {
    constructor() {
        this.hasEngine = true;
    }
}
class Bus extends Vehicle {
    constructor() {
    // 不要在调用 super()之前引用 this，否则会抛出 ReferenceError
    super(); // 相当于 super.constructor()
        console.log(this instanceof Vehicle); // true
        console.log(this); // Bus { hasEngine: true }
    }
}
new Bus();
```
**在静态方法中可以通过 super 调用继承的类上定义的静态方法**：
```javascript
class Vehicle {
    static identify() {
        console.log('vehicle');
    }
}

class Bus extends Vehicle {
    static identify() {
        super.identify();
    }
}
Bus.identify(); // vehicle 
```

#### [2.1 在使用 super 时要注意几个问题](#)
* super 只能在派生类构造函数和静态方法中使用。     
* 不能单独引用 super 关键字，要么用它调用构造函数，要么用它引用静态方法。
* 调用 super()会调用父类构造函数，并将返回的实例赋值给 this。
* super()的行为如同调用构造函数，如果需要给父类构造函数传参，则需要手动传入。

**如果没有定义类构造函数，在实例化派生类时会调用 super()，而且会传入所有传给派生类的参数**。
```javascript
class Vehicle {
 constructor(licensePlate) {
     this.licensePlate = licensePlate;
 }
}

class Bus extends Vehicle {}
console.log(new Bus('1337H4X')); // Bus { licensePlate: '1337H4X' } 
```
**如果在派生类中显式定义了构造函数，则要么必须在其中调用 super()，要么必须在其中返回一个对象**
```javascript
class Vehicle {}

class Car extends Vehicle {}

class Bus extends Vehicle {
 constructor() {
     super();
 }
}

class Van extends Vehicle {
 constructor() {
     return {};
 }
}
console.log(new Car()); // Car {}
console.log(new Bus()); // Bus {}
console.log(new Van()); // {} 
```

#### [2.2 抽象基类](#)
有时候可能需要定义这样一个类，它可供其他类继承，但本身不会被实例化。虽然 ECMAScript 没有专门支持这种类的语法 ，但通过 new.target 也很容易实现。`new.target` **保存通过 new 关键字调用的类或函数**。通过在实例化时检测 new.target 是不是抽象基类，可以阻止对抽象基类的实例化：

```javascript
// 抽象基类
class Vehicle {
    constructor() {
        console.log(new.target);
        if (new.target === Vehicle) {
            throw new Error('Vehicle cannot be directly instantiated');
        }
    }
}
// 派生类
class Bus extends Vehicle {}
new Bus(); // class Bus {}
new Vehicle(); // class Vehicle {}
// Error: Vehicle cannot be directly instantiated
```
另外，通过在抽象基类构造函数中进行检查，可以要求派生类必须定义某个方法。因为原型方法在
调用类构造函数之前就已经存在了，所以可以通过 this 关键字来检查相应的方法：
```javascript
// 抽象基类
class Vehicle {
    constructor() {
        if (new.target === Vehicle) {
            throw new Error('Vehicle cannot be directly instantiated');
        }
        if (!this.foo) {
            throw new Error('Inheriting class must define foo()');
        }
        console.log('success!');
    }
}
// 派生类
class Bus extends Vehicle {
    foo() {}
}

try{
// 派生类
    class Van extends Vehicle {}
    new Bus(); // success!
    new Van(); 
}catch(e){
    console.log(e.message); //Inheriting class must define foo()
}
```

#### [2.3 继承内置类型](#)
ES6 类为继承内置引用类型提供了顺畅的机制，开发者可以方便地扩展内置类型：

```javascript
class SuperArray extends Array {
    shuffle() {
// 洗牌算法
        for (let i = this.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [this[i], this[j]] = [this[j], this[i]];
        }
    }
}
let a = new SuperArray(1, 2, 3, 4, 5);
console.log(a instanceof Array); // true
console.log(a instanceof SuperArray); // true 
```
如果想覆盖这个默认行为，则可以覆盖 Symbol.species 访问器，这个访问器决定在创建返回的实例时使用的类：

```javascript
class SuperArray extends Array {
    static get [Symbol.species]() {
        return Array;
    }
}
let a1 = new SuperArray(1, 2, 3, 4, 5);
let a2 = a1.filter(x => !!(x%2))

console.log(a1); // [1, 2, 3, 4, 5]
console.log(a2); // [1, 3, 5]
console.log(a1 instanceof SuperArray); // true
console.log(a2 instanceof SuperArray); // false
```

#### [2.4 类混入](#)
把不同类的行为集中到一个类是一种常见的 JavaScript 模式。虽然 ES6 没有显式支持多类继承，但通过现有特性可以轻松地模拟这种行为。

> `Object.assign()` 方法是为了混入对象行为而设计的。只有在需要混入类的行为
> 时才有必要自己实现混入表达式。如果只是需要混入多个对象的属性，那么使用
> `Object.assign()` 就可以了。

在下面的代码片段中，extends 关键字后面是一个 JavaScript 表达式。任何可以解析为一个类或一
个构造函数的表达式都是有效的。这个表达式会在求值类定义时被求值：
```javascript
class Parent {
    className = 'parent';
}

function expression() {
    return Parent;
}

class Son extends expression() {
    constructor(id, name, age) {
        super();
        this.id = id;
        this.name = name;
        this.age = age;
    }

    //格式化输出
    toString() {
        return `{ id: ${this.id}, age: ${this.age}, name: ${this.name} }`;
    }
}

let me = new Son("2025","JX", 27);

console.log(me.toString());
```
混入模式可以通过在一个表达式中连缀多个混入元素来实现，这个表达式最终会解析为一个可以被继承的类。

一个策略是定义一组“可嵌套”的函数，每个函数分别接收一个超类作为参数，而将混入类定义为
这个参数的子类，并返回这个类。这些组合函数可以连缀调用，最终组合成超类表达式：
```javascript
class Vehicle {}
let FooMixin = (Superclass) => class extends Superclass {
    foo() {
        console.log('foo');
    }
};
let BarMixin = (Superclass) => class extends Superclass {
    bar() {
        console.log('bar');
    }
};
let BazMixin = (Superclass) => class extends Superclass {
    baz() {
        console.log('baz');
    }
};
class Bus extends FooMixin(BarMixin(BazMixin(Vehicle))) {}
let b = new Bus();
b.foo(); // foo
b.bar(); // bar
b.baz(); // baz 
```
通过写一个辅助函数，可以把嵌套调用展开：

```javascript
class Vehicle {}
let FooMixin = (Superclass) => class extends Superclass {
    foo() {
        console.log('foo');
    }
};
let BarMixin = (Superclass) => class extends Superclass {
    bar() {
        console.log('bar');
    }
};
let BazMixin = (Superclass) => class extends Superclass {
    baz() {
        console.log('baz');
    }
};
function mix(BaseClass, ...Mixins) {
    return Mixins.reduce((accumulator, current) => current(accumulator), BaseClass);
}
class Bus extends mix(Vehicle, FooMixin, BarMixin, BazMixin) {}
let b = new Bus();
b.foo(); // foo
b.bar(); // bar
b.baz(); // baz
```

> 很多 JavaScript 框架（特别是 React）已经抛弃混入模式，转向了组合模式（把方法
> 提取到独立的类和辅助对象中，然后把它们组合起来，但不使用继承）。这反映了那个众
> 所周知的软件设计原则：“组合胜过继承（composition over inheritance）。”这个设计原则被
> 很多人遵循，在代码设计中能提供极大的灵活性。
 
### [3. 访问性之私有化](#)
[私有属性](https://mdn.org.cn/en-US/docs/Web/JavaScript/Reference/Classes/Private_properties) 是常规类属性（包括类字段、类方法等）的对应项，这些属性是公开的。私有属性通过使用哈希 `#` 前缀创建，并且不能在类外部合法地引用。这些类属性的隐私封装由 JavaScript 本身强制执行。访问私有属性的唯一方法是通过点表示法，并且只能在定义私有属性的类中执行此操作。

语法


```javascript
class ClassWithPrivate {
  #privateField;
  #privateFieldWithInitializer = 42;

  #privateMethod() {
    // …
  }

  static #privateStaticField;
  static #privateStaticFieldWithInitializer = 42;

  static #privateStaticMethod() {
    // …
  }
}
```

还有一些额外的语法限制

在类中声明的所有私有标识符必须是唯一的。命名空间在静态属性和实例属性之间共享。唯一的例外是当两个声明定义 getter-setter 对时。
私有标识符不能是 #constructor。

大多数类属性都有其私有对应项

- 私有字段
- 私有方法
- 私有静态字段
- 私有静态方法
- 私有 getter
- 私有 setter
- 私有静态 getter
- 私有静态 setter

这些特性统称为私有属性。但是，构造函数在 JavaScript 中不能是私有的。为了防止在类外部构造类，您必须使用私有标志。

私有属性用 `#` 名称（发音为“哈希名称”）声明，这些名称是带有 # 前缀的标识符。哈希前缀是属性名称的固有部分——您可以将其与旧的下划线前缀约定 _privateField 联系起来——但它不是普通的字符串属性，因此您不能使用方括号表示法动态访问它。