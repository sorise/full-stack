## [TypeScript 函数](#)
**介绍**: TypeScript 作为 JavaScript 的超集，不仅包含了所有 JavaScript 的功能，还增加了一些特性来增强类型安全性和开发体验。

----

- []() 

---


### [一、函数和Funtion类型](#)

#### [1.1 函数的类型声明](#)
函数的类型声明，需要在声明函数时，给出参数的类型和返回值的类型。

```typescript
function hello_world(name: string,txt: string): void {
    const message: string = `Hello World!, ${name} wanna tell you : ${txt}`;
    console.log(message);
}
```
- 返回值的类型通常可以不写，因为 TypeScript 自己会推断出来。
- 如果不指定参数类型，TypeScript 就会推断参数类型，如果缺乏足够信息，就会推断该参数的类型为any。
    - 如果没有return语句，TypeScript 会推断出函数hello()没有返回值。

如果变量被赋值为一个函数，变量的类型有两种写法。
```typescript
// 写法一
const hello = function (txt: string) {
  console.log("hello " + txt);
};

// 写法二
const hello: (txt: string) => void = function (txt) {
  console.log("hello " + txt);
};
```
- 写法一是通过等号右边的函数类型，推断出变量hello的类型；
- 写法二则是 **使用箭头函数的形式**，为变量hello指定类型，参数的类型写在箭头左侧，返回值的类型写在箭头右侧。


**不可以省略参数名**，有的语言的函数类型可以不写参数名（比如 C 语言），但是 TypeScript 不行。如果写成(string) => void，TypeScript 会理解成函数有一个名叫 string 的参数，并且这个string参数的类型是any。
```typescript
type MyFunc = (string, number) => number;
// (string: any, number: any) => number
```
函数类型没写参数名，导致 TypeScript 认为参数类型都是any。


**函数类型里面的参数名与实际参数名，可以不一致**。
```typescript
let f: (x: number) => number;

f = function (y: number) {
  return y;
};
```

如果函数的类型定义很冗长，或者多个函数使用同一种类型，写法二用起来就很麻烦。因此，往往用type命令为函数类型定义一个别名，便于指定给其他变量。
```typescript
type MyFunc = (txt: string) => void;

const hello: MyFunc = function (txt) {
  console.log("hello " + txt);
};
```

#### [1.2 函数声明的参数个数问题](#)
这是因为 JavaScript 函数在声明时往往有多余的参数，实际使用时可以只传入一部分参数。比如，数组的forEach()方法的参数是一个函数，该函数默认有三个参数(item, index, array) => void，实际上往往只使用第一个参数(item) => void。因此，TypeScript 允许函数传入的参数不足。

下面示例中，函数x只有一个参数，函数y有两个参数，x可以赋值给y，反过来就不行。
```typescript
let x = (a: number) => 0;
let y = (b: number, s: string) => 0;

y = x; // 正确
x = y; // 报错
```
如果一个变量要套用另一个函数类型，有一个小技巧，就是使用typeof运算符。
```typescript
function add(x: number, y: number) {
  return x + y;
}

const myAdd: typeof add = function (x, y) {
  return x + y;
};
```

函数的实际参数个数，可以少于类型指定的参数个数，但是不能多于，即 TypeScript 允许省略参数。
```typescript
let myFunc: (a: number, b: number) => number;

myFunc = (a: number) => a; // 正确

myFunc = (a: number, b: number, c: number) => a + b + c; // 报错
```

#### [1.3 函数声明的对象写法](#)
变量add的类型就写成了一个对象。

```typescript
let add: {
  (x: number, y: number): number;
};

add = function (x, y) {
  return x + y;
};
```
函数类型的对象写法如下。
```
{
  (参数列表): 返回值
}
```
注意，这种写法的函数参数与返回值之间，间隔符是冒号:，而不是正常写法的箭头=>，因为这里采用的是对象类型的写法，对象的属性名与属性值之间使用的是冒号。

这种写法平时很少用，但是非常合适用在一个场合：函数本身存在属性。
```typescript
function f(x: number) {
  console.log(x);
}

f.version = "1.0";
```
上面示例中，函数f()本身还有一个属性version。这时，f完全就是一个对象，类型就要使用对象的写法。
```typescript
let foo: {
  (x: number): void;
  version: string;
} = f;
```

函数类型也可以使用 Interface 来声明
```typescript
interface myfn {
  (a: number, b: number): number;
}

var add: myfn = (a, b) => a + b;
```
