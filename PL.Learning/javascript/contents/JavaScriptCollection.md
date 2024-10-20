## [Javasript 集合类型](#)
> **介绍**：JavaScript 提供了多种内置的集合类型，包括数组（Array）、对象（Object）、Map 和 Set。

-----
- [1. Array](#1-array)


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
/*
[
  1,
  2,
  3,
  42.5,
  Student { name: 'liZhiMing', age: 25 },
  Symbol(index),
  'banana',
  'apple',
  'peach'
]
* */
```

* 数组是可调整大小的，并且可以包含不同的数据类型。（当不需要这些特征时，可以使用类型化数组。）
* 数组不是关联数组，因此，不能使用任意字符串作为索引访问数组元素，但必须使用非负整数（或它们各自的字符串形式）作为索引访问。
* 数组的索引从 0 开始：数组的第一个元素在索引 0 处，第二个在索引 1 处，以此类推，最后一个元素是数组的 length 属性减去 1 的值。
* 数组复制操作创建浅拷贝。（所有 JavaScript 对象的标准内置复制操作都会创建浅拷贝，而不是深拷贝）。

#### [1.1 创建数组](#)
有几种基本的方式可以创建数组。一种是使用 Array 构造函数，比如：
```javascript
let colors = new Array();
```