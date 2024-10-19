### [Javascript 继承 旧版本](#)
> **介绍**：继承是面向对象编程中讨论最多的话题。很多面向对象语言都支持两种继承：接口继承和实现继承。
前者只继承方法签名，后者继承实际的方法。

-----
- [1. 原型链继承](#1-原型链继承)
- [2. 借用构造函数继承](#2-借用构造函数继承)
- [3. 组合继承](#3-组合继承)
- [4. 原型继承](#4-原型继承)
- [5. 寄生式继承](#5-寄生式继承)
- [6. 寄生组合式继承](#6-寄生组合式继承)
- [7. 自悟的完美继承方式](#7-自悟的完美继承方式)

-----

### [1. 原型链继承](#)
首先我们要回忆一下，使用构造函数创建对象所JS所做的五个操作

1. 在内存中创建一个对象
2. 这个对象内部的 `[[Prototype]]`特性被赋值为构造函数的 ptototype 属性
3. 构造函数中 this  指向新对象
4. 执行构造函数内部代码，给新对象添加属性
5. 如果构造函数返回非空对象，那么返回该对象，否则返回刚创建的新对象

#### [1.1 原型链](#)
[**原型链**](https://mdn.org.cn/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain): **构造函数、原型和实例的关系**。
* 记住凡是对象都具有 `__proto__` 属性指向自己的原型。
* 每个构造函数都有一个`prototype`指向一个原型对象，原型有一个属性 `constructor` 指回构造函数，而实例有一个内部指针 `__proto__` 指向原型。
* 而原型本身又是一个对象，他也有自己的 `__proto__` 属性。这个原型本身有一个内部指针指向另一个原型，相应地另一个原型也有一个指针指向另一个构造函
  数。
```
                                                另一个原型对象 [属性 constructor]、[属性 __proto__] 
                                                      ↑
                --------------------------------------|                  
                |
  构造函数 [属性 prototype]  ---------------->  原型对象 [属性 constructor]、[属性 __proto__] 
        ↑                                           ↑    |   
        |                                           |    |   
        --------------------------------------------|-----   
                                                    |        
   实例 [属性 __proto__] -----------------------------
```

```javascript
实例.__proto__ === 构造函数.prototype 
```

#### [1.2 一个经典的原型链继承](#)

```javascript
"use strict";

function Parent(name = 'null', age = 0){
  this.name = name;
  this.age = age;
}

Parent.prototype.sayHello = function(_name){
  console.log(`hello,${_name} ,my name is ${this.name}`);
}

function Child(id){
  this.id = id;
}

Child.prototype.showID = function(){
  console.log(`showID,${this.id}`);
}

Child.prototype = new Parent('jxkicker', 25);

let student = new Child('2016110418');

console.log(student);
console.log(student instanceof Parent); //true
console.log(student instanceof Child); //true
console.log(student.constructor); //Parent
```
值得注意的点：constructor 属性的问题，由于Child构造函数的原型上面没有定义 constructor 属性, constructor 在 Child的原型的原型上面 也就是 Parent。
也就是这条原型链的构造函数只有一个 Parent。其他子类型虽然有构造函数但是并不指向他们自己的构造函数

**默认原型** : 任何函数的prototype 指向默认都是指向一个Object 实例的。当然Object类型本身并没有属性，只有方法，但也有一个 Object Prototype。方法都定义在Object原型上面。

**继承关系**
```
  构造函数Object [prototype]  ---------------->  原型对象 [属性 constructor]  [属性 __proto__] 
        ↑                                           ↑    |   
        |                                           |    |   
        --------------------------------------------|-----   
                                                    |        
   Object实例 [属性 __proto__] ----------------------
        |                   
        ------------------------------ = ---------------------------------------------------
                                                                                           |
  构造函数Parent [prototype]  ---------------->  原型对象 [属性 constructor]  [属性 __proto__] = new Object();
        ↑                                           ↑    |   
        |                                           |    |   
        --------------------------------------------|-----   
                                                    |        
   Parent实例 [属性 __proto__] -----------------------
        |                   
        ------------------------------ = --------------------------------
                                                                        | 
  构造函数Child [prototype]  ---------------->  原型对象 [属性 __proto__] = new Parent(...parameters)
                                                    ↑     
                                                    |     
                                                    |
                                                    |        
   Child实例 [属性 __proto__] -----------------------
```

#### 1.3 原型链的问题
1. **原型的属性在所有实例间共享**, 继承的不是类型而是实例。
2. **子类在实例化时不能给父类的构造函数传递参数**。

### [2. 借用构造函数继承](#)
利用 `call(this, 参数)` 绑定this实现的, 你了解一下就行了,为啥呢 这种构造函数仍然不完美 
1. 使用借用构造函数无法 实现 子类是属于父类的判断 子类 instanceof 父类 会返回 false
2. 因为没有完成函数的复用
3. 可以向父构造器传递参数,`唯一作用` 。

```javascript
function Parent(color = "white",typeName ="卵生物种"){
    this.typeName =  typeName;
    this.color = color;
}

Parent.prototype.getColor = function () {
    return this.color;
}

function Children(color,typeName, sex,weight){
    Parent.call(this, color, typeName); //借用构造函数
    this.sex = sex;
    this.weight = weight;
}

Children.prototype.getSort = function () {
    return this.typeName;
}

let child = new Children('brown', '恐龙', true, 15000);
console.log(child instanceof Parent); //false 无法完成继承判断
console.log(child.getColor());//error 无法继承父类的方法
```

### [3. 组合继承](#)
它又名为 伪经典继承 同时使用原型继承 和构造函数继承 构造函数继承属性 原型继承原型函数对象

* 这是非常需要掌握的继承方式
* 子类 instanceof 父类 会返回 ture 实现了继承
* **缺点**: 同一个名称的属性 同时存在实例上面 和原型上面 由于搜索优先级的关系 原型上面的属性被屏蔽了

```javascript
function Parent(color = "white",typeName ="卵生物种"){
    this.typeName =  typeName;
    this.color = color;
}

Parent.prototype.getColor = function () {
    return this.color;
}

function Children(color,typeName, sex,weight){
    Parent.call(this, color, typeName); //借用构造函数
    this.sex = sex;
    this.weight = weight;
}

Children.prototype = new Parent();
Children.prototype.getSort = function () {
    return this.typeName;
}

let human = new Children('white', '人类',true , 50);

console.log(human);
//Parent { typeName: '人类', color: 'white', sex: true, weight: 50 }
console.log(human instanceof Parent); //true
```
`typeName、color同时存在实例和原型上面。`


### [4. 原型继承](#)
利用一件存在的一个对象作为原型 赋值给一个新的对象 完成继承,严格意义上来讲它不叫继承，应该叫对象扩展，
当你有了一个对象，想在它的基础上再创建一个新对象。

```javascript
function object(o){
    function F() {};
    F.prototype = o;
    return new F();
}

var person = {
    name:"JxKicker",
    friends:["xaio","ming","hong"]
};

var antherPerson = object(person);
antherPerson.name = "Kicker";
antherPerson.friends.push('liz');
/*
antherPerson 
  name: "Kicker"
  __proto__ ----> person
              name:"JxKicker"  
              friends:["xaio","ming","hong", liz]
*/

var yetPerson = object(person);
yetPerson.name = "Linda";
yetPerson.friends.push('Barce');

/*
yetPerson 
  name: "Linda"
  __proto__ ----> person
              name:"JxKicker"  
              friends:["xaio","ming","hong", "liz","Barce"]
*/

console.log(person);
/*
{ 
  name: 'JxKicker',
  friends: [ 'xaio', 'ming', 'hong', 'liz', 'Barce' ] 
}
*/
```

### [5. 寄生式继承](#)
寄生式继承是与原型式继承紧密相关第一种思路,他利用了工厂模式和原型式继承模式。

```javascript
function createAnthor(original){
  function F() {};
  F.prototype = original;
  let clone = new F();
  clone.sayHi = function(){
    alert("hi");
  }
}

/* //你可以间接
function createAnthor(original){
  var clone = object(original);
  clone.sayHi = function(){
    alert("hi");
  }
}
*/

var person = {
    name:"JxKicker",
    friends:["xaio","ming","hong"]
};

var obj = createAnthor(person);
```

### [6. 寄生组合式继承](#)
组合继承,非常不错 很常用，**但是也有一些缺点多次调用构造函**, 但是这也是最实用的继承方法了。

1. 多次调用构造函数 会两次调用父类的构造函数 
   * 子类构造函数内部
   * 超类型构造函数
2. 属性重复

寄生组合式继承:很好减少了很多的东西 通过改变引用关系 完美的实现了继承 它是最应该掌握的继承方式。
* 借用构造函数实现属性继承
* 利用原型链完成对函数方法的继承

```javascript
function object(o){
    function F() {};
    F.prototype = o;
    return new F();
}

function inheritPrototype(son,father){
    var prototype = object(father.prototype); //  var prototype = Object.create(father.prototype)；
    prototype.constructor = father;
    son.prototype = prototype;
}

/***************** 父类 *******************/
function Egg(color = "white",typeName ="卵生物种"){
    this.typeName =  typeName ,
        this.color = color
}

Egg.prototype.getType = function(){
    return this.typeName;
}
/***************** 父类 *******************/


/***************** 子类 *******************/

function chicken(MainColor,sex,weight){
    Egg.call(this,MainColor,"鸡"); //借用构造函数 实现属性 继承
    this.sex = sex;
    this.weight = weight;
}

inheritPrototype(chicken,Egg);//函数复用 和继承关系 实现

chicken.prototype.getPrice = function(Meony){
    if (typeof Meony != "number") {
        console.error('Error', 'Meony is not Number Type')
    }
    return  this.weight * Meony;
};

let littleChicken =  new chicken("White",true,2.1);

/*
for (let property in littleChicken){
    console.log(`${property}: ${littleChicken[property]} `);
}*/

console.log(littleChicken instanceof Egg); //true
```


### [7. 自悟的完美继承方式](#)
要最完美的继承，需要能够对 `__proto__` 属性实现重写。我们就使用这种方法实现继承。

```javascript
/* Parent继承自 Object */
function Parent(name = 'null', age = 0){
    this.name = name;
    this.age = age;
}

Parent.prototype.intro = function (lang) {
    if (lang.trim().toLowerCase() === 'chinese'){
        return `姓名: ${this.name} 年龄: ${this.age}`
    }
    else if (lang.trim().toLowerCase() === 'english'){
        return `name: ${this.name} age: ${this.age}`
    }
}

function Child(name, age, id){
    /*借用构造函数实现属性继承*/
    Parent.call(this,name, age);
    this.id = id;
}

let childProto = {};
Object.defineProperty(childProto, 'constructor',{
    value:Child,
    enumerable: false
});
childProto.__proto__  = Parent.prototype;
Child.prototype = childProto;

/* 定义子类的方法 */
Child.prototype.getName = function () {
    return this.name;
};

let Lzm = new Child('李志明', 23, '2016110425');

console.log(Lzm);
```