## [TypeScript 类型系统](#)
> **介绍**：TypeScript类型包括了 JavaScript 已经提供的内置类型，还包括了一些新类型，如元祖、any、unknown、interface...。

---


- [一、类型总结概述](#一类型总结概述)
- 

---

### [一、类型总结概述](#)
TypeScript它是JavaScript的超集，增加了静态类型定义，TypeScript的类型系统非常丰富，它不仅包含了 [JavaScript中已有的各种数据类型](../../javascript/contents/JavaScriptDateType.md)，还增加了一些独特的类型定义方式。


**基本类型**（Primitive Types）
- boolean: 表示逻辑实体，如true或false。
- number: 用于表示整数和浮点数。
- string: 用于表示文本数据，使用双引号(")或单引号(')包裹。
- void: 表示没有可用值，通常用于函数返回值类型，表示该函数不返回任何值。
- null: 表示对象值的缺失。
- undefined: 用于未初始化的变量。

**特殊类型**
- any: 可以是任意类型的值，当不确定变量的具体类型时使用。
- never: 表示永远不会发生的值的类型，例如一个函数总是抛出异常而不会正常返回。

**复合类型**（Composite Types）
- 数组（Arrays）: 使用type[]或者Array<type>来定义。
- 元组（Tuples）: 允许你在一个变量中存储不同类型的元素，且每个元素的位置都有明确的类型。

**枚举（Enums）**
- 枚举为一组数值集合提供更友好的名称，使得代码更加清晰易读。默认情况下，枚举成员从0开始依次递增赋值，但也可以手动指定值。

**接口（Interfaces）**
- 接口用于定义对象的形状，即对象应该拥有哪些属性和方法以及它们的类型。接口不是实际存在的类型，而是对对象结构的描述。

**类（Classes）**
- 类可以通过构造函数创建实例，并且支持继承、公共、私有和受保护的修饰符来控制成员的访问权限。

**泛型（Generics）**
- 泛型允许你在定义函数、接口或类时，不预先指定具体的类型，而是在使用这些组件时再指定类型。这样可以在不牺牲类型安全的前提下编写可重用的代码。

**联合类型（Union Types）与交叉类型（Intersection Types）**
- union 联合类型允许一个变量属于多个类型之一，使用`|`符号分隔不同的类型。
- 交叉类型将多个类型合并成一个类型，要求满足所有类型的特性，使用`&`符号。

**字面量类型（Literal Types）**
- 字面量类型让你可以指定变量只能取特定的几个值，比如字符串字面量或数字字面量。

> 这些类型共同构成了TypeScript强大的类型系统

### [二、常用类型](#)
介绍 any、unknown、never类型。

#### [2.1 any 类型](#)
any 类型表示没有任何限制，该类型的变量可以赋予任意类型的值。

```typescript
let x: any;

x = 1; // 正确
x = "foo"; // 正确
x = true; // 正确
```
**变量类型一旦设为any，TypeScript 实际上会关闭这个变量的类型检查**。即使有明显的类型错误，只要句法正确，都不会报错。
```typescript
let x: any = "hello";

x(1); // 不报错
x.foo = 100; // 不报错
```
**由于这个原因，应该尽量避免使用any类型，否则就失去了使用 TypeScript 的意义**。

实际开发中，any类型主要适用以下两个场合。
- 出于特殊原因，需要关闭某些变量的类型检查，就可以把该变量的类型设为any。
- 为了适配以前老的 JavaScript 项目，让代码快速迁移到 TypeScript，可以把变量类型设为any。

**类型推断问题**: 对于开发者没有指定类型、TypeScript 必须自己推断类型的那些变量，如果无法推断出类型，TypeScript 就会认为该变量的类型是any。
```typescript
function add(x, y) {
  return x + y;
}

add(1, [1, 2, 3]); // 不报错
```
TypeScript 提供了一个编译选项noImplicitAny，打开该选项，只要推断出any类型就会报错。
```shell
tsc --noImplicitAny app.ts
```

**污染问题** : any类型除了关闭类型检查，还有一个很大的问题，就是它会“污染”其他变量。它可以赋值给其他任何类型的变量（因为没有类型检查），导致其他变量出错。
```typescript
//any Type

let cle_element: any = "coordinate";
cle_element = 75;
cle_element = false;

let mine: number = 75;
let sex: boolean = false;
let scores: Array<number> = [75.12, 95.26];

mine = cle_element;
sex = cle_element;
scores = cle_element;
```
污染其他具有正确类型的变量，把错误留到运行时，这就是不宜使用any类型的另一个主要原因。


#### [2.2 unknown 类型](#)
为了解决any类型“污染”其他变量的问题，TypeScript 3.0 引入了unknown类型。它与any含义相同，表示类型不确定，可能是任意类型，但是它的使用有一些限制，不像any那样自由，可以视为严格版的any。

unknown跟any的相似之处，在于所有类型的值都可以分配给unknown类型。
```typescript
let x: unknown;

x = true; // 正确
x = 42; // 正确
x = "Hello World"; // 正确
```
首先，**unknown类型的变量，不能直接赋值给其他类型的变量**（除了any类型和unknown类型）。

```typescript
let v: unknown = 123;

let v1: boolean = v; // 报错
let v2: number = v; // 报错
```
其次，**不能直接调用unknown类型变量的方法和属性**。
```typescript
let v1: unknown = { foo: 123 };
v1.foo; // 报错

let v2: unknown = "hello";
v2.trim(); // 报错

let v3: unknown = (n = 0) => n + 1;
v3(); // 报错
```
再次，unknown类型变量能够进行的运算是有限的，只能进行**比较运算**（运算符`==`、`===`、`!=`、`!==`、`||`、`&&`、`?`）、**取反运算**（运算符!）、**typeof运算符**和**instanceof运算符**这几种，其他运算都会报错。

```typescript
let a: unknown = 1;

a + 1; // 报错
a === 1; // 正确

let s: unknown = "hello";

if (typeof s === "string") {
  s.length; // 正确
}
```

