### [Object 终极父类](#)
>**介绍**：Object 是终极父类 Object 对象中的所有属性和方法都会出现在其他对象中。

-----
- [1. 创建Object 对象](#1-创建object-对象)
- [2. Object 静态方法](#2-object-静态方法)
- [3. 属性创建](#3-属性创建)
- [4. 使用函数创建对象](#4-使用函数创建对象)
- [5. 基本原理](#5-基本原理)
-----

### [1. 创建Object 对象](#)
到目前为止，大多数引用值的示例使用的是 Object 类型。Object 是 ECMAScript 中最常用的类型之一。

尽管JS支持对象,但是在ES6之前它并不支持接口和类的基本结构,引用类型又叫对象定义 描述属性和方法 以下我们简单的创建对象方法！

**创建对象的方法**: 
* 显式地创建 Object 的实例有两种方式。第一种是使用 new 操作符和 Object 构造函数
* 另一种方式是使用 **对象字面量（object literal）表示法**
* 总结创建方式：new Ojbect()、字面量{key:value,key2:value2}、 Object.create(obj)。

**其他使用方法**
* 使用方式：对象.属性=value，对象["属性名"]=value，属性(Key)存在则赋值，不存在则创建并赋值。
* 删除属性：delete obj.prop
* 检测属性："key" in obj，返回bool
* 遍历属性：for(let key in obj) **循环所有key**。
```javascript
//1.0 构造函数
let person = new Object();
person.name = "kicker";
person.age = 15;
//2.0 对象字面量 更加常用
let apple = {
   color:"red"
}
//3.0
let phone = {};
phone.color = "red"

//4.0
let stu = new Object({ name: "lzm", age : 26});
let json=  JSON.stringify(stu, null, 2);
console.log(json);
/*
{
  "name": "lzm",
  "age": 26
}
* */

let vs =  "age" in stu;
console.log(vs); //true
```
自己定义一个构造函数
```javascript
function Person(Name,Age,Sex){
    this.name = Name;
    this.age = Age;
    this.sex = Sex;
    this.show = function(){
        console.log(`${this.name} ${this.sex} ${this.age}` )
    }
};

let p1 = new Person("kicker",17,true);
p1.show();

console.log(`Name:${p1["name"]}`);
console.log(`Name:${p1["age"]}`);
console.log(`Name:${p1["sex"]}`);
```

访问属性的方式,不仅仅可以通过 `.` 的方式 还可以通过方括号 [] 变量的方式引用属性



#### [1.1 Object 类型具有下列属性](#)
* **constructor**: 对创建对象的函数的引用（指针）。对于 Object 对象，该指针指向原始的 Object() 函数。
* **prototype**: 对该对象的**原型对象的引用**。对于所有的对象，它默认返回 Object 对象的一个实例。

#### [1.2 对象实例的默认方法、属性](#)

* hasOwnProperty(property):判断对象是否有某个特定的属性。必须用字符串指定该属性。（例如，o.hasOwnProperty("name")）
* propertyIsEnumerable(propertyName):判断给定的属性是否可以用 for...in 语句进行枚举。
* toString():返回对象的原始字符串表示。对于 Object 对象，ECMA-262 没有定义这个值，所以不同的 ECMAScript 实现具有不同的值
* valueOf():返回最适合该对象的原始值。对于许多对象，该方法返回的值都与 ToString() 的返回值相同。
* isPrototypeOf(obj): 检查对象是否出现在某个对象的原型链中。
* **__proto__** 指向实例化的原型对象，推荐Object.getPrototypeOf()
* **constructor**: 对创建对象的函数的引用（指针）。对于 Object 对象，该指针指向原始的 Object() 函数。

```javascript
const obj1={};
const obj2={
    name:"zhagnsan",
    "age":"3",
    school:{    //属性值也是对象
        name:"龙腾幼儿园",
        address:"那边",
    },
    sayHello:function(name){    //属性值是函数
        console.log("hello "+name);
    },
}
obj2.sayHello("sam");
obj2.birthday="2022-12-12";//赋值操作：属性(Key)存在则赋值，不存在则创建+赋值

//可以用null为原型创建一个干净的对象，不会从从Object.prototype继承任何属性方法
const nobj = Object.create(null, { name: { value: "sam", enumerable: true } });
nobj.age = 20;
nobj.toString=Object.prototype.toString; //赋予toString()方法

Object.getOwnPropertyNames(["a","b"])	//[ "0", "1", "length" ]
Object.keys(["a","b"]) //[ "0", "1" ]
Object.getPrototypeOf([]); //Array []

const obj={name:"sam",sex:"male",say:function(){}};
obj.hasOwnProperty("name"); //true
obj.propertyIsEnumerable("name"); //true
obj.toString = function(){return JSON.stringify(this)};

console.log(Person.prototype.isPrototypeOf(p1)); //true
``` 

### [2. Object 静态方法](#)
Object 对象有一些静态方法，这些方法通常用于操作对象。例如：

| 方法                                | 说明 |
|:----------------------------------|:---|
| Object.assign()                   | 复制一个或多个源对象的所有可枚举属性到目标对象。      |        
| Object.create()                   | 创建一个具有指定原型的新对象。|                       
| Object.defineProperty()           | 在一个对象上定义一个新的属性或修改现有属性，并返回该对象。 |
| Object.defineProperties()         | 在一个对象上定义新的属性或修改现有属性，并返回该对象。|
| Object.entries()                  | 返回一个数组，包含对象自身的每个可枚举属性`[key, value]`对。|
| Object.fromEntries()              | 返回一个由键值对组成的对象。|
| Object.freeze()                   | 冻结一个对象。|
| Object.getOwnPropertyDescriptor() | 返回一个对象的指定属性的描述符。|
| Object.getOwnPropertyNames()      | 返回一个数组，包含对象自身的所有属性的键。|
| Object.getOwnPropertySymbols()    | 返回一个数组，包含对象自身的所有符号属性。|
| Object.keys()                     | 返回一个数组，包含对象自身的所有可枚举属性的键。|
| Object.values()                   | 返回一个数组，包含对象自身的所有可枚举属性的值。|
| Object.is()                       | 判断两个值是否相等。|
| Object.seal()                     | 密封一个对象。|
| Object.setPrototypeOf()           | 设置一个对象的原型到另一个对象或 null。|
| Object.isSealed()                 |  静态方法用于判断一个对象是否密封。|
| Object.seal()                     |  静态方法密封一个对象。|
| Object.hasOwn()                     |  如果指定对象具有指示的属性作为其自身属性，则返回 true。如果属性是继承的，或者不存在，则该方法返回 false。|


#### [2.1 Object.create](#)
该方法用于创建新对象。第一个参数用于指定新建对象的原型对象；第二个参数是对象的属性描述对象。方法返回新建的对象。

Object.create(proto,propertiesObject)

```javascript
function Person() {}
Person.prototype.hello = function (){
    console.log("hello")
}
let person = Object.create(Person.prototype,{
    name:{
        value:"jonas",
        writable:true,
        configurable:true,
    },
    age:{
        value:18,
        writable:true,
        configurable:true,
    }
})
console.log(person)//Person {name: "jonas", age: 18}
person.hello()//hello
```
#### [2.2 Object.defineProperty](#)
Object.defineProperty(obj,prop,desc):在对象 obj 上定义新的属性，或者修改对象 obj 中的属性，结果返回对象 obj。

```javascript
let person = {}
Object.defineProperty(person,"name",{
    value : "jonas",
    writable : true,
    enumerable : true,
    configurable : true
})
console.log(person)//{name: "jonas"}
```

#### [2.3 Object.defineProperties()](#top)
一次性定义多个属性 如果有些描述属性没有给与值 那么默认为false

```javascript
var book = {};

Object.defineProperties(book,{
    _year:{
        writable:true,
        value:2004
    },
    edition:{
        writable:true, //一定要有 不然要出错 使用 defineProperties 默认值为 false
        value:1
    },
    year:{
        configurable:false, //默认就是false
        enumerable:true,
        get: function(){
            return this._year;
        },
        set: function(newValue){
            if(newValue >= 2004  ){
                this._year = newValue;
                this.edition = newValue - 2003;
            }
            
        }
    }
});
book.year = 2008;

console.log(book.edition);
```

```javascript
let kingdom = {};
Object.defineProperties(kingdom, {
    name:{ //国民
        writable:true,
        value:'中华人民共和国',
        enumerable:true,
        configurable:true
    },
    long:{ //国祚
        enumerable:true,
        configurable:true,
        get() {
            return (new Date()).getFullYear() - 1949;
        }
    },
    peopleCount:{ //民族数量
        enumerable:true,
        configurable:true,
        value:56,
        writable:false
    }
});
```

#### [2.4 Object.freeze](#)
Object.freeze(obj):该方法用于冻结对象，一个被冻结的对象不能被修改，不能添加新的属性，不能修改属性的描述符，该对象的原型对象也不能修改。返回值为被冻结的对象。

```javascript
let person = {name:"jonas",age:18}
Object.freeze(person)
person.address = "Guangzhou"
person.age = 20
console.log(person)//{name: "jonas", age: 18}
```
#### [2.5 Object. getOwnPropertyDescriptor](#)
该方法用于返回指定对象上自有属性对应的属性描述符。
```javascript
let obj = {}
Object.defineProperty(obj,"name",{
    configurable:false,
    enumerable:true,
    writable:true,
    value:"Jonas"
})
let descriptor = Object.getOwnPropertyDescriptor(obj,"name")
console.log(descriptor)//{value: "Jonas", writable: true, enumerable: true, configurable: false}
```

#### [2.6 Object.entries](#)
Object.entries(obj):该方法返回对象 obj 自身的可枚举属性的键值对数组。结果是一个二维数组，数组中的元素是一个由两个元素 key ，value 组成的数组。

```javascript
const object1 = {
  a: 'somestring',
  b: 42,
};

for (const [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}
```

#### [2.7 Object.getOwnPropertySymbols](#)
Object.getOwnPropertySymbols(obj):该方法返回一个指定对象自身所有的 Symbol 键名的属性的数组。


#### [2.8 Object.getOwnPropertyNames](#)
Object.getOwnPropertyNames(obj):该方法返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括 Symbol 作为键名的属性）组成的数组。

```javascript
let nameSymbol = Symbol('name');
let ageSymbol  = Symbol('age');
let sexSymbol = Symbol('sex');

let user =  {
    uid: '2016110418',
    [nameSymbol]: 'remix',
    [ageSymbol]: 25,
    toString: function () {
        return `Name: ${this[nameSymbol]} age: ${this[ageSymbol]} sex: ${this[sexSymbol]?'boy':'girl'}`
    }
}

Object.defineProperty(user, sexSymbol,  {
    value: true,
    configurable: false,
    enumerable: true,
    writable: false
})


let uidSymbol = Symbol('uid');
let gradeSymbol = Symbol('grade');
Object.defineProperties(user, {
    [uidSymbol]: {
        value:'2016110418',
        configurable: false,
        enumerable: true,
        writable: false
    },
    [gradeSymbol]: {
        value:'高三',
        configurable: false,
        enumerable: true,
        writable: false
    }
});


console.log(user.toString());//Name: remix  age: 25
console.log(user[nameSymbol]);//remix
console.log(user[sexSymbol]);//true

for (const property of  Object.getOwnPropertyNames(user)) {
    console.log(property);
}//uid toString


for (const symbol of  Object.getOwnPropertySymbols(user)) {
    console.log(symbol);
}//Symbol(name) Symbol(age) Symbol(sex) Symbol(uid) Symbol(grade)
```

#### [2.9 Object.seal](#)
Object.seal(obj) 封闭对象，阻止添加新属性并将所有的属性标记为不可配置！

#### [2.10 Object.isFrozen](#)
Object.isFrozen(obj) 判断对象是否被冻结。返回布尔值。

#### [2.10 Object.isExtensible](#)
Object.isExtensible(obj) 该方法用于判断一个对象是否可以扩展（是否可以添加属性），返回布尔值。
在默认的情况下，对象是允许扩展的（无论是通过对象构造函数还是对象字面量方式创建的对象）。封闭对象，冻结对象是不可扩展的！！！

```javascript
let obj = {}
Object.freeze(obj)
console.log(Object.isExtensible(obj))//false
```

#### [2.11 Object.is](#)
Object.is(obj1,obj2) 该方法用于比较两个对象是否相同，返回布尔值。

比较规则如下：
* 如果两个值都是 undefined ，则返回 true
* 如果两个值都是 null，则返回 true
* 如果两个值都是 true 或 false ，则返回 true
* 如果两个值都是由相同个数的字符按照相同的顺序组成的字符串，则返回 true
* 如果两个值指向同一个对象，则返回 true
* 如果两个值都是 +0 ,-0，NaN，则返回 true

注意：该方法不会做隐式类型转换。

#### [2.12 Object.keys](#)
Object.keys(obj) ：遍历可枚举的属性，只包含对象本身可枚举属性，不包含原型链可枚举属性

```javascript
let arr = ["a", "b", "c"];
let obj = { foo: "bar", baz: 42 };
let ArrayLike = { 0 : "a", 1 : "b", 2 : "c"};

Object.keys(arr)        // ['0', '1', '2']
Object.keys(obj)        // ["foo","baz"]
Object.keys(ArrayLike)  // ['0', '1', '2']
```

#### [2.13 Object.values](#)
Object.values(obj) 遍历可枚举的属性值，只包含对象本身可枚举属性值，不包含原型链可枚举属性值

```javascript
let arr = ["a", "b", "c"];
let obj = { foo: "bar", baz: 42 };
let ArrayLike = { 0 : "a", 1 : "b", 2 : "c"};

Object.values(arr)      // ["a", "b", "c"]
Object.values(obj)          // ["bar",42]
Object.values(ArrayLike)    // ["a", "b", "c"]
```


#### [2.14 Object.setPrototypeOf](#)
Object.setPrototypeOf(obj, proto) 设置一个指定的对象的原型

```javascript
const obj = {a: 1}, proto = {b:2}

Object.setPrototypeOf(obj, proto)

obj.__proto__ === proto     //true
```

#### [2.15 Object.entries(obj)](#)
entries 分割对象

```javascript
const obj = { foo: 'bar', baz: 42 };
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]

// array like object
const obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.entries(obj)); // [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ]

//  string
Object.entries('abc')   // [['0', 'a'], ['1', 'b'], ['2', 'c']]

Object.entries(100)   // []
```

#### [2.16 Object.getPrototypeOf()](#)
Object.getPrototypeOf(obj) 获取指定对象的原型（内部[[Prototype]]属性的值）

```javascript
const prototype1 = {};
const object1 = Object.create(prototype1);

console.log(Object.getPrototypeOf(object1) === prototype1);   // true

//注意：Object.getPrototypeOf(Object) 不是 Object.prototype
Object.getPrototypeOf( Object ) === Function.prototype;  // true
```

#### [2.17 Object.getOwnPropertyDescriptors()](#)
Object.getOwnPropertyDescriptors 方法，返回指定对象所有自身属性（非继承属性）的描述对象。

```javascript
const obj = {
  foo: 123,
  get bar() { return 'abc' }
};
 
console.dir(Object.getOwnPropertyDescriptors(obj))
//   { foo:{ value: 123,
//      writable: true,
//      enumerable: true,
//      configurable: true },
//       bar:{ get: [Function: bar],
//      set: undefined,
//      enumerable: true,
//      configurable: true } 
//     }

/*
使用场景：
Object.assign() 方法只能拷贝源对象的可枚举的自身属性，同时拷贝时无法拷贝属性的特性，而且访问器属性会被转换成数据属性，也无法拷贝源对象的原型

Object.create() 方法可以实现上面说的这些，配合getPrototypeOf，以及getOwnPropertyDescriptors实现全面浅拷贝

Object.create(
  Object.getPrototypeOf(obj), 
  Object.getOwnPropertyDescriptors(obj) 
);
*/
```

#### [2.18 Object.assign](#)
Object.assign(target,...assign): 作用：将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。
```javascript
const target = {name:"jonas",age:18};
const source = {address:"Guangzhou",gender:"male"}

Object.assign(target,source);
console.log(target);//{name: "jonas", age: 18, address: "Guangzhou", gender: "male"}
```

#### [2.19 Object.isSealed](#)
**Object.isSealed(obj)** 如果对象被密封，则返回 true，否则返回 false。如果对象不可扩展，并且所有属性都是不可配置的，因此
不可移除（但并不一定是不可写入的），那么该对象就被密封。

```javascript
const object1 = {
  property1: 42,
};

console.log(Object.isSealed(object1));
// Expected output: false

Object.seal(object1);

console.log(Object.isSealed(object1));
// Expected output: true
```

#### [2.20 Object.seal()](#)
Object.seal() 静态方法密封一个对象。密封一个对象阻止扩展并使现有属性不可配置。密封的对象具有固定的属性集：无法添加新属性、无法删除现有属性、无法更改其可枚举性和可配置性，以及无法重新分配其原型。只要可写，现有属性的值仍然可以更改。seal() 返回与传入的相同对象。

```javascript
const object1 = {
  property1: 42,
};

Object.seal(object1);
object1.property1 = 33;

console.log(object1.property1);
// Expected output: 33

delete object1.property1; // Cannot delete when sealed
console.log(object1.property1);
// Expected output: 33
```

#### [2.21 Object.hasOwn()](#)
Object.hasOwn() 静态方法如果指定对象具有指示的属性作为其自身属性，则返回 true。如果属性是继承的，或者不存在，则该方法返回 false。

```javascript
const object1 = {
  prop: 'exists',
};

console.log(Object.hasOwn(object1, 'prop'));
// Expected output: true

console.log(Object.hasOwn(object1, 'toString'));
// Expected output: false

console.log(Object.hasOwn(object1, 'undeclaredPropertyValue'));
// Expected output: false
```


### [3. 属性创建](#)
ECMA 在第五版定义了只有内部采用的特性 描述了属性的各种特性,ECM-262 定义这些特性是否了实现JavaScript 是为了实现JavaScripty殷勤用的,因此Js代码中无法访问他们
为了表示特性是内部值 规范用双中括号表示 `[[enumerable]]`

- [x] `(1). ECMA中有两种属性:数据属性和访问器属性`

#### [3.1 数据属性](#)
包含一个数据值的位置，这个位置可以读取 数据属性有四个描述其行为的特性。

- `(1)`. `[[configurable]]`: **是否能够通过delete 删除属性从而重新定义属性**, 默认值为true defineProperties中默认值为false。
- `(2)`. `[[enumerable]]`:  表示能否通过for-in循环返回属性 **默认为true** defineProperties中默认值为false。
- `(3)`. `[[writeable]]`: 表示能否修改属性的值 **默认为 true** defineProperties中默认值为false。
- `(4)`. `[[value]]`: 包含这个属性的数据值，读取属性值的时候 从这个位置读取 写入属性值的 吧新值保存在这个位置。

```javascript
var person = {
  Name:"Jxkicker" //前三个特性值为 true [[Value]] 为 "Jxkicker"
}
```
#### [3.2 Object.defineProperty(obj,propertyName,obj_desc)](#)
如果要修改属性默认的特性,必须使用defineProperty方法 接收三个参数 属性所在对象 属性名称 和一个描述符对象 描述符对象的属性必须是  configurable,enumerable
writeable,value 设置一个或者多个值
 
**注意**: 那么不设置的特征值是 默认值 可以多次调用 此方法修改同一个属性但是 但是在吧configurable设置 为false之后就会有错误了 
```javascript
var person = {} ; 

Object.defineProperty(person,"name",{
    configurable:true,
    writable:false,
    value:"JxKicker"
});

person.name = "Kicker";

console.log(person.name); //JxKicker
```

#### [3.3 访问器属性](#)
它不包含值;他们包含一堆getter setter 函数 在读取属性值的时候调用getter属性 在设置属性值的时候 掉setter值 这个函数负责决定如何处理函数,访问属性有如下四个
特性值

- `(1)`. `[[configurable]]`: 是否能够通过delete 删除属性从而重新定义属性 默认值为true defineProperties中默认值为false。
- `(2)`. `[[enumerable]]`: 表示能否通过for-in循环返回属性默认为true defineProperties中默认值为false。
- `(3)`. `[[get]]`: 在读取属性的时候调用的函数 默认值 undefined。
- `(4)`. `[[set]]`: 在设置属性的时候调用的函数 默认值 undefined。

```javascript
let toolBox = {};

Object.defineProperty(toolBox, "_basic_standard_precision", {
    configurable: false,
    writable:false,
    value:3,
    enumerable:false
});

Object.defineProperty(toolBox,"pricision", {
    configurable:true,
    Enumerable:true,
    get() {
        return this._basic_standard_precision;
    },
    set(v) {
        this._basic_standard_precision = v;
    }
})

console.log(toolBox.pricision);
```

#### [3.4 Object.defineProperties()](#)
一次性定义多个属性 如果有些描述属性没有给与值 那么默认为false。

```javascript
var book = {};

Object.defineProperties(book,{
    _year:{
        writable:true,
        value:2004
    },
    edition:{
        writable:true, //一定要有 不然要出错 使用 defineProperties 默认值为 false
        value:1
    },
    year:{
        configurable:false, //默认就是false
        enumerable:true,
        get: function(){
            return this._year;
        },
        set: function(newValue){
            if(newValue >= 2004  ){
                this._year = newValue;
                this.edition = newValue - 2003;
            }
            
        }
    }
});
book.year = 2008;
console.log(book.edition);
```

```javascript
let kingdom = {};
Object.defineProperties(kingdom, {
    name:{ //国民
        writable:true,
        value:'ZH',
        enumerable:true,
        configurable:true
    },
    long:{ //国祚
        enumerable:true,
        configurable:true,
        get() {
            return (new Date()).getFullYear() - 1949;
        }
    },
    peopleCount:{ //民族数量
        enumerable:true,
        configurable:true,
        value:56,
        writable:false
    }
});
```

#### [3.5 读取属性的特性](#)
js提供了 Object.getOwnPropertyDescriptor(obj,propertyName); 第一个参数 对象 第二个参数 对象的属性名 返回一个描述对象
```javascript
let descripter =  Object.getOwnPropertyDescriptor(book,"edition");

console.log(descripter.value);  //5
console.log(descripter.enumerable); //false
console.log(descripter.configurable); //false
console.log(descripter.writable);  //true

let yeardescripter =  Object.getOwnPropertyDescriptor(book,"year");

console.log(yeardescripter.configurable); // false
console.log(yeardescripter.enumerable); // true
console.log(typeof yeardescripter.set); //function
console.log(typeof yeardescripter.get); //function
```

### [4. 使用函数创建对象](#)
以上创建对象的方式 太过于单一，无法提供一个创建对象的模板 还无法实现继承重用效果,怎么实现来 请让我细细道来。

**谨记：每一个函数本身也是一个对象哦，且函数有一个属性叫prototype指向一个对象，这个对象被称为原型！这个原型对象有一个属性叫constructor指向函数**

**必须掌握**：原型模式，其他模式了解知道。
#### [4.1 工厂模式](#)
这是最基本的模式开发人员 发明一种函数 来封装以特定接口创建对象的细节
```javascript
function PersonFatory(name = "",age = 0,sex = true,job ="" ){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.sex = sex;
    o.job = job; 
    return o;
}

var person1 = PersonFatory("JxKicker",28,true,"Worker");
var person2 = PersonFatory("Ninchol",25,false,"Coder");

console.log('person1', person1);
console.log('person2:', person2);

/*
 person1 { name: 'JxKicker', age: 28, sex: true, job: 'Worker' }
 person2: { name: 'Ninchol', age: 25, sex: false, job: 'Coder' }
 */
```
**他解决了创建多个类似对象的问题 但是没有解决对象识别的问题 如何知道一个对象的类型**。

#### [4.2 构造函数](#)
JS构造函数可以创建特定类型的对象
* **构造函数没有return**
* **直接将属性和方法赋值给this对象**
* **没有显示的创建对象**
* 实例化要使用 new 操作符返回的是一个新的的对象
* 每个对象都有一个 constructor 属性指向 **构造函数**

使用构造函数创建对象所JS所做的五个操作
* **在内存中创建一个对象**
* 这个对象内部的 `[[Prototype]] (__proto__)` 特性被赋值为构造函数的ptototype属性
* 构造函数中 this 指向新对象
* 执行构造函数内部代码，给新对象添加属性
* 如果构造函数返回非空对象，那么返回该对象，否则返回刚创建的新对象。

```javascript
function Person(name = "",age = 0,sex = true,job ="" ){
    this.name = name;
    this.age = age;
    this.sex = sex;
    this.job = job; 
    saidHello = function(){
        console.warn('Hello My Name is ', this.name);
    }
}

var person1_ = new Person("JxKicker",28,true,"Worker");
var person2_ = new Person("Ninchol",25,false,"Coder");

console.log('person1_:', person1_);
console.log('person2_:', person2_);
/*
 person1_: Person { name: 'JxKicker', age: 28, sex: true, job: 'Worker' }
 person2_: Person { name: 'Ninchol', age: 25, sex: false, job: 'Coder' }
 */
 // 可以检测出来类型
 console.log('person1_ instanceof Person : ', person1_ instanceof Person); //true
```
构造函数的问题- **同一种功能的函数被反复创建**。

#### [4.3 原型模式](#)
我们创建的每个函数都有一个prototype(原型)属性,这个属性是一个指针,指向一个对象,而这个对象的用途是包含由特定类型的所有势力共享的属性和方法。

你可以理解为静态：最大的好处: 可以让所有对象势力共享它所包含的属性和方法 也就是说说不必再构造函数中定义方法 也可以将这些方法直接添加到原型对象中。

* 默认指向Object
* 我们原型模式和构造函数相结构的方法 来创建对象  因为原型对象 里面包含的东西 是静态的 但是又是类似于对象属性 本事上来说 它是公有的 所有对象的公有属性方法
```javascript
function Person(name = "",age = 0,sex = true,job ="" ){
    this.name = name;
    this.age = age;
    this.sex = sex;
    this.job = job; 
}

Person.prototype.SaidHello = function(){
    console.log(`${this.name} siad : Hello World!`);
}


var person1_ = new Person("JxKicker",28,true,"Worker");
var person2_ = new Person("Ninchol",25,false,"Coder");

console.log('person1_:', person1_);
console.log('person2_:', person2_);


person1_.SaidHello();
person2_.SaidHello();

```
只要创建一个新函数,就会根据一组特定的规则为该函数创建一个 prototype 属性,该属性指向函数的原型对象,在默认的情况下,所有的原型对象都会自动获得

一个 `constructor`属性这个属性包含一个指向 prototype属性所在函数的指针每个使用构造函数创建的对象都具有一个属性 `__proto__` 指向构造函数 prototype指向的 对象！ 可以说是复制其值了！

Object.getPrototypeOf(obj) 得到一个对象的原型
```javascript
function Person(){}
Person.prototype= {
    name:"Jxkicker",
    age:29,
    job:"Software Engineer",
    sayName:function(){
        alert(this.name);
    }
}

var p1 = new Person();
let pro = Object.getPrototypeOf(p1);
console.log('pro:\n', pro);
/*
pro:
 { name: 'Jxkicker',
  age: 29,
  job: 'Software Engineer',
  sayName: [Function: sayName] }
*/
```
#### [4.3 寄生构造模式](#)
用来干嘛呀 用来扩展一个现有的对象 例如 Array类型， 寄生构造函数模式与工厂模式极为相似，区别在于：

- 寄生构造函数模式将工厂模式中的那个通用函数createPerson()改名为Person()，并将其看作为对象的构造函数。
- 创建对象实例时，寄生构造函数模式采用new操作符

> 两者本质上的差别仅在于new操作符（因为函数取什么名字无关紧要），工厂模式创建对象时将createPerson看作是
> 普通的函数，而寄生构造函数模式创建对象时将Person看作是构造函数，不过这对于创建出的对象来说，没有任何差别。

**如果函数里面已经有返回对象了，那么这个对象就会覆盖掉new所返回的那个对象**。
```javascript
//寄生构造函数模式 
function Person(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function() {
        alert(this.name);
    }
    return o;
}

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

function NumberArray(){
    var array = new Array();
    array.getFirst = function () {
        return this.length > 0 ? this[0]:null;
    }
    return array;
}

var myArray = new NumberArray();
myArray.push(1);
myArray.push(2);
myArray.push(3);

console.log(myArray.getFirst());
//已经不用了
```

#### [4.4 稳妥构造函数](#)
稳妥构造函数模式与前面介绍的两种设计模式具有相似的地方，都是在函数内部定义好对象之后返回该对象来创建实例。然而稳妥构造函数模式的独特之处在于具有以下特点：

* 没有通过对象定义公共属性
* 在公共方法中不使用this引用对象自身
* 不使用new操作符调用构造函数

```javascript
//4.4 稳妥构造函数
function Person(para_name, para_age, para_job) {
    //创建要返回的对象
    var o = {};

    //在这里定义私有属性和方法
    var name = para_name;
    var age = para_age;
    var job = para_job;

    o.sayAge = function() {
        console.log(age);
    };
    //在这里定义公共方法
    o.sayName = function() {
        console.log(name);
    };
    //返回对象
    return o;
}

var person1 = Person("Nicholas", 29, "Software Engineer");    //创建对象实例
person1.sayName();    //Nicholas
person1.name;         //undefined
person1.sayAge();     // 报错

console.log(person1 instanceof Person); //false
```

**问题**: 无法继承，无法判断类型！ instanceof 无法判断，本质上 Object 类型！只是实现了私有属性！

**稍作改造，使得它有类型**。
```js
function Person(para_name, para_age, para_job) {
    //创建要返回的对象
    var o = {};
    o.__proto__ = Person.prototype;
    //在这里定义私有属性和方法
    var name = para_name;
    var age = para_age;
    var job = para_job;

    o.sayAge = function() {
        console.log(age);
    };
    //在这里定义公共方法
    o.sayName = function() {
        console.log(name);
    };
    //返回对象
    return o;
}

var person1 = Person("Nicholas", 29, "Software Engineer");    //创建对象实例

console.log(person1 instanceof Person); //true
```

### [5. 基本原理](#)
例如属性访问原理，构造函数！！！

#### [5.1 属性访问属性](#)
当访问一个属性的,它是如何查找值的呢？ 这和静态语言不同 且等我嘻嘻说来
* 每当读取某个对象的某个属性的时候,都会执行一次搜索,目标是具有给定名称的属性
  * 1.首先搜索对象实例本身的属性 找到 返回值 找不到 搜索指针指向的原型对象
  * 2.在原型对象中搜素属性 找到返回 找不到 则继续向上查找 原型的原型
* 所以 如果原型对象属性 和实例对象的属性 同名的时候 会发生覆盖 并且实例属性的优先级高

#### [5.2 基本定理](#)
* **Function是最顶层的构造方法，所有对象都由Function方法构造，包括Object方法**，Function方法（有些人把这个认为是Function方法的自生性，不用纠结，不管是什么原因，只要记住Function是方法也是对象就行）
* **所有的方法（就是克隆类）都继承Function.prototype对象（包括Function方法 通过定理1可得）**
* 不要纠结是先有Object对象，还是先有Function方法，就跟先有鸡还是现有蛋一样，相互依赖，同时诞生
* 每个对象的_proto_属性（注意不是prototype属性）都指向自己的继承父类，也就是他的克隆类的原型对象，也就是Foo.prototype对象（object对象）


-----
时间: 2024/10/19 第四次修订