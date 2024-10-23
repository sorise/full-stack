## [Javasript 函数](#)
> **介绍**：JavaScript函数是ECMAScript中最有意思的部分之一，这主要是因为函数实际上是对象。

-----
- [1. 基本概念](#1-基本概念)

-----
### [1. 基本概念](#)
每个函数都是Function 类型的实例，而 Function 也有属性和方法，跟其他引用类型一样。
因为函数是对象，所以函数名就是指向函数对象的指针，而且不一定与函数本身紧密绑定。

```javascript
function sum (num1, num2) {
 return num1 + num2;
} 
```
另一种定义函数的语法是函数表达式。函数表达式与函数声明几乎是等价的：
```javascript
let sum = function(num1, num2) {
    return num1 + num2;
}; 
```
还有一种定义函数的方式与函数表达式很像，叫作“箭头函数”（arrow function），如下所示：
```javascript
let sum = (num1, num2) => {
    return num1 + num2;
}; 
```
