### [JavaScript Class](#)
>**介绍**：ECMAScript 6 新引入的 class 关键字具有正式定义类的能力。类（class）是ECMAScript 中新的基础性语法糖结构。

-----
- [1. 类定义](#1-类定义)

-----

### [1. 类定义](#)
[Mdn 类](https://mdn.org.cn/en-US/docs/Web/JavaScript/Reference/Classes)，类是创建对象的模板。它们
将数据与处理这些数据的代码封装在一起。JS 中的类构建在 原型 之上，但也具有一些类独有的语法和语义。

与函数类型相似，定义类也有两种主要方式：类声明和类表达式。这两种方式都使用 class 关键字加大括号：
```javascript
// 类声明
class Person {}
// 类表达式
const Animal = class {}; 
```
与函数表达式类似，类表达式在它们被求值前也不能引用。不过，与函数定义不同的是，虽然函数声明可以提升，**但类定义不能**：

#### [1.1 类的成员](#)
类可以包含**构造函数方法**、**实例方法**、**获取函数**、**设置函数**和 **静态类方法**，但这些都不是必需的,空的类定义照样有效。

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