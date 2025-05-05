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

### [二、特殊类型](#)
介绍 any、unknown、never、void类型。

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

#### [2.3 never 类型](#)
TypeScript中的never类型是一个比较特殊的类型，它表示那些永不存在的值的类型。具体来说，never类型是所有类型的子类型，但没有类型是never的子类型（包括它自己）。这意味着声明为never类型的变量只能被赋值为never类型的值，并且不能有任何实际的值。

**以下是never类型的一些主要用途和特点**：

**函数无返回值**：当一个函数执行过程中总是抛出异常而不会正常结束时，或者存在无限循环的情况下，该函数被认为返回never类型。
```typescript
function throwError(message: string): never {
    throw new Error(message);
}
```
**代码块永远无法到达**：在某些情况下，TypeScript编译器能够推断出一段代码逻辑永远不会被执行到，这时它会将这种情形的类型推断为never。
```typescript
type A = string | number;

function controlFlowAnalysis(a: A) {
    if (typeof a === "string") {
        console.log("It's a string");
    } else if (typeof a === "number") {
        console.log("It's a number");
    } else {
        // 这里的 'a' 被推断为类型 'never'
        const exhaustiveCheck: never = a;
    }
}
```

#### [2.4 void类型](#)
void通常用于函数返回值声明，含义：`函数不返回任何值`，**调用者也不应依赖其返回值进行任何操作**。 void 类型在 TypeScript 中主要用于清晰地标记那些预期不返回任何有意义值的函数，帮助开发者更好地理解和管理代码中的数据流和逻辑。

理解void与 undefined
- void是一个广泛的概念，用来表达“空”，而undefined则是这种“空”的具体实现之一，因此可以说undefined是void能接受的“空”状态的一种具体形式。
- 换句话说：void包含undefined，但void表达的语义超越了单纯的undefined，它是一种
意图上的约定，而不仅仅是特定值的限制。

> **注意**： 在JavaScript 如果函数中没有明确使用return关键字返回 "返回值" , 那么函数会默认返回undefined 值 ;


**函数返回值**： 最常见的用法是标记一个函数不返回任何有意义的值。
```typescript
function logMessage(message: string): void {
    console.log(message);
}

const result = logMessage("Hello, world!");
// result 的类型是 undefined，但通常不会被使用。

function logInfo(message: string): void{
    let now = new Date().toLocaleString();
    console.log(`[INFO ${now}] ${message}`);
}

let re = logInfo('Hello World');
//re void 类型
```

**变量声明**：虽然不太常见，但你也可以将变量声明为 void 类型。在这种情况下，这个变量只能被赋予 undefined 或 null（如果启用了严格的 null 检查）。不过，这种用法并不推荐，因为它很少有实际意义。
```typescript
let unusable: void = undefined;
// 如果启用了严格模式，并且编译选项中包含了 "strictNullChecks"，
// 那么还可以赋值为 null：
unusable = null; // 可能需要启用特定编译选项才有效
```

特性与限制
- 不能直接赋值：除了 undefined 和可能的 null（取决于你的 TypeScript 编译选项），你不能给一个 void 类型的变量赋予其他类型的值。
- 无返回值 vs 返回 undefined：值得注意的是，TypeScript 中的 void 类型不同于显式返回 undefined。虽然两者在实践中可能看起来相似，但是它们有不同的含义。例如，你可以有一个函数明确返回 undefined，这与该函数具有 void 返回类型是不同的概念。

```typescript
function logInfo(message: string): void{
    let now = new Date().toLocaleString();
    console.log(`[INFO ${now}] ${message}`);
}

function logInfoError(message: string): void{
    let now = new Date().toLocaleString();
    let info = `[INFO ${now}] ${message}`;
    console.log(info);
    return;
}

function logInfoUndefined(message: string): void{
    let now = new Date().toLocaleString();
    let info = `[INFO ${now}] ${message}`;
    console.log(info);
    return undefined;
}

function logInfoNull(message: string): void{
    let now = new Date().toLocaleString();
    let info = `[INFO ${now}] ${message}`;
    console.log(info);
    return null; //错误
}
```
**作为函数返回值时不要关注返回值**,调用者也不应依赖其返回值进行任何操作
```typescript
//any Type

function logInfo(message: string): void{
    let now = new Date().toLocaleString();
    console.log(`[INFO ${now}] ${message}`);
}

let t = logInfo("test");

if (t){
    //An expression of type void cannot be tested for truthiness.
    console.log(`never`);
}
```

总结：若函数返回类型为void，那么：
1. 从语法上讲：函数是可以返回undefined的，至于显示返回，还是隐式返回，这无所谓！
2. 从语义上讲：函数调用者不应关心函数返回的值，也不应依赖返回值进行任何操作！即使返回了undefined值。

### [三、基本类型](#)
JavaScript 语言（注意，不是 TypeScript）将值分成 8 种类型（boolean、string、number、bigint、symbol、object（function）、undefined、nul）。 TypeScript 继承了 JavaScript 的类型设计，以上 8 种类型可以看作 TypeScript 的基本类型。

> **注意**，上面所有类型的名称都是小写字母，首字母大写的Number、String、Boolean等在 JavaScript 语言中都是内置对象，而不是类型名称。

**JavaScript的8种类型之中**，undefined和null其实是两个特殊值，object属于复合类型，剩下的五种属于原始类型（primitive value），代表最基本的、不可再分的值。

- **boolean**
- **string**
- **number**
- **bigint**
- **symbol**

五种包装对象之中，symbol 类型和 bigint 类型无法直接获取它们的包装对象（即Symbol()和BigInt()不能作为构造函数使用），但是剩下三种可以。
* Boolean()
* String()
* Number()

#### [3.1 包装对象类型与字面量类型](#)
由于包装对象的存在，导致每一个原始类型的值都有包装对象和字面量两种情况。

为了区分这两种情况，TypeScript 对五种原始类型分别提供了大写和小写两种类型。

- Boolean 和 boolean
- String 和 string
- Number 和 number
- BigInt 和 bigint
- Symbol 和 symbol

其中，大写类型同时包含包装对象和字面量两种情况，小写类型只包含字面量，不包含包装对象。
```typescript
const s1: String = "hello"; // 正确
const s2: String = new String("hello"); // 正确

const s3: string = "hello"; // 正确
const s4: string = new String("hello"); // 报错
```

#### [3.2 Object 类型与 object 类型](#)
TypeScript 的对象类型也有大写Object和小写object两种。

| **特性**  | Object 类型（大写）  | object 类型（小写） |
|---------------------|-----------------------------------------------|------------------------------------|
| **范围**   | 所有值（除 `null` 和 `undefined`）| 仅非原始类型（对象、数组、函数等） |
| **类型安全性**| 较低（允许原始类型）| 较高（仅允许对象）  |
| **适用场景** | 需要兼容所有值时 | 需要确保变量是对象时 |
| **是否允许原始类型**| √  | X |
| **是否允许 `null`** | X  | X |
| **推荐程度**        | ! 不推荐用于类型安全场景   | V 推荐使用 |

通过合理选择 `Object` 和 `object` 类型，可以更安全、更灵活地处理 TypeScript 中的对象和值。

| 类型        | 可接受的值类型  | 是否允许原始类型 | 是否允许 `null` |
|-------------|----------------|------------------|---------|
| `Object`    | 所有值（除 `null` 和 `undefined`）| √ | ✘ |
| `object`    | 仅非原始类型（对象、数组、函数等）| ✘  | ✘ |

#### [3.3 Object 类型](#)
大写的 `Object` 类型代表 JavaScript 语言里面的广义对象。所有可以转成对象的值，都是Object类型，这囊括了几乎所有的值。
**Object是能存储的类型是可以调用到object方法的类型**。

**`Object` 类型（大写）** :
- **语义**：`Object` 是 JavaScript 中所有对象的基类（原型链的顶端），表示“广义对象”。  
- **范围**：几乎可以接受任何值（除了 `null` 和 `undefined`）。  
  - 原始类型（如 `number`、`string`、`boolean` 等）会被自动装箱为对应的包装对象（如 `Number`、`String`、`Boolean`）。  
  - 包括对象、数组、函数、日期、正则表达式等。  

```typescript
let obj: Object;

obj = true;
obj = "hi";
obj = 1;
obj = { foo: 123 };
obj = [1, 2];
obj = (a: number) => a + 1;
```
上面示例中，原始类型值、对象、数组、函数都是合法的Object类型。

事实上，除了 `undefined` 和 `null` 这两个值不能转为对象，其他任何值都可以赋值给 `Object` 类型。
```typescript
let obj: Object;

obj = undefined; // 报错
obj = null; // 报错
```

#### [3.4 object 类型](#)
小写的 `object` 类型代表 JavaScript 里面的狭义对象，即可以用字面量表示的对象，只包含对象、数组和函数，不包括原始类型的值。


**`object` 类型（小写）** :
- **语义**：`object` 是 TypeScript 中的“狭义对象”类型，表示**非原始类型**。  
- **范围**：仅包含对象、数组、函数等非原始类型，**排除所有原始类型**（`number`、`string`、`boolean`、`symbol`、`null`、`undefined`）。  

```typescript
let obj: object;

obj = { foo: 123 };
obj = [1, 2];
obj = (a: number) => a + 1;
obj = true; // 报错
obj = "hi"; // 报错
obj = 1; // 报错
```
上面示例中，object类型不包含原始类型值，只包含对象、数组和函数。

大多数时候，我们使用对象类型，只希望包含真正的对象，不希望包含原始类型。所以，建议总是使用小写类型object，不使用大写类型Object。

注意，无论是大写的Object类型，还是小写的object类型，都只包含 JavaScript 内置对象原生的属性和方法，用户自定义的属性和方法都不存在于这两个类型之中。

```typescript
const o1: Object = { foo: 0 };
const o2: object = { foo: 0 };

o1.toString(); // 正确
o1.foo; // 报错

o2.toString(); // 正确
o2.foo; // 报错
```
上面示例中，toString()是对象的原生方法，可以正确访问。foo是自定义属性，访问就会报错。

#### [3.5 undefined 和 null 的特殊性](#)
undefined 和 null 既是**值**，**又是类型**，作为值，它们有一个特殊的地方：任何其他类型的变量都可以赋值为 `undefined` 或 `null`。

```typescript
let age: number = 24;

age = null; // 正确
age = undefined; // 正确
```
上面代码中，变量age的类型是number，但是赋值为 `null` 或`undefined`并不报错。

这并不是因为undefined和null包含在number类型里面，而是故意这样设计，任何类型的变量都可以赋值为undefined和null，以便跟 JavaScript 的行为保持一致。

JavaScript 的行为是，变量如果等于undefined就表示还没有赋值，如果等于null就表示值为空。所以，TypeScript 就允许了任何类型的变量都可以赋值为这两个值。

但是有时候，这并不是开发者想要的行为，也不利于发挥类型系统的优势。
```typescript
const obj: object = undefined;
obj.toString(); // 编译不报错，运行就报错
```
为了避免这种情况，及早发现错误，TypeScript 提供了一个编译选项strictNullChecks。
只要打开这个选项，undefined和null就不能赋值给其他类型的变量（除了any类型和unknown类型）。

```typescript
// tsc --strictNullChecks app.ts

let age: number = 24;

age = null; // 报错
age = undefined; // 报错
```
上面示例中，打开 `--strictNullChecks` 以后，number类型的变量age就不能赋值为 `undefined` 和 `null`。

这个选项在配置文件tsconfig.json的写法如下。
```json
{
  "compilerOptions": {
    "strictNullChecks": true
    // ...
  }
}
```
打开strictNullChecks以后，undefined和null这两种值也不能互相赋值了。
```typescript
// 打开 strictNullChecks

let x: undefined = null; // 报错
let y: null = undefined; // 报错
```
上面示例中，undefined类型的变量赋值为null，或者null类型的变量赋值为undefind，都会报错。

总之，打开strictNullChecks以后，undefined和null只能赋值给自身，或者any类型和unknown类型的变量。
```typescript
let x: any = undefined;
let y: unknown = null;
```

#### [3.5 声明对象类型](#)
在实际开发中，可以限制一般对象，一般使用如下形式。
```typescript
//声明
let student : {name: string, age: number};

//student只能写以上两个属性，不可以少写也不可以多写
student = {
    name: "Lzm",
    age: 5,
}
```
使用 `?`来实现 **约束可选属性**, 如下所示sex属性可以实现也可以不实现。
```typescript
let student : {name: string, age: number; sex?: boolean};

student = {
    name: "Lzm",
    age: 5,
}
```
**实现任意追加属性**, `[key:typeName]:any` ,如下所示 key必须是 string类型，值可以是任意类型。
```typescript
let student : {
    name: string
    age: number
    [key:string]: any
};

student = {
    name: "Lzm",
    age: 5,
    sex: "male",
    city: "CD",
}
```

#### [3.6 声明函数类型](#)
声明函数类型。

```typescript
function logInfo(message: string): void{
    let now = new Date().toLocaleString();
    console.log(`[INFO ${now}] ${message}`);
}

let info: (message: string) => void;

info = logInfo;


let plus: (pro: number, next: number) => number;

plus = function (a,b) {
    return a + b
};
```

### [四、值类型](#)



### [五、数组类型](#)
TypeScript 数组有一个根本特征：所有成员的类型必须相同，但是成员数量是不确定的，可以是无限数量的成员，也可以是零成员。

#### [5.1 数组声明](#)
数组的类型有两种写法。第一种写法是在数组成员的类型后面，加上一对 `[]`。
```typescript
let arr: number[] = [1, 2, 3];
```
上面示例中，数组arr的类型是 `number[]`，其中number表示数组成员类型是 `number`。

如果数组成员的类型比较复杂，可以写在圆括号里面。
```typescript
let arr: (number | string)[];

let numsName: (string|number)[];

numsName = ["tick", 75.75, "million", 45.63];
```
如果数组成员可以是任意类型，写成 `any[]`。当然，这种写法是应该避免的。
```typescript
let arr: any[];
```

数组类型的第二种写法是使用 TypeScript 内置的 Array 接口。
```typescript
let arr: Array<number> = [1, 2, 3];
```
这种写法对于成员类型比较复杂的数组，代码可读性会稍微好一些。
```typescript
let arr: Array<number | string>;
```
**数组类型声明了以后，成员数量是不限制的，任意数量的成员都可以，也可以是空数组**。
```typescript
let arr: number[];
arr = [];
arr = [1];
arr = [1, 2];
arr = [1, 2, 3];
```
正是由于成员数量可以动态变化，所以 TypeScript 不会对数组边界进行检查，越界访问数组并不会报错。
```typescript
let arr: number[] = [1, 2, 3];
let foo = arr[3]; // 正确
```

TypeScript 允许使用方括号读取数组成员的类型。
```typescript
type Names = string[];
type Name = Names[0]; // string
```
上面示例中，类型Names是字符串数组，那么Names[0]返回的类型就是string。

由于数组成员的索引类型都是number，所以读取成员类型也可以写成下面这样。
```typescript
type Names = string[];
type Name = Names[number]; // string
```
上面示例中，Names[number]表示数组Names所有数值索引的成员类型，所以返回string。


#### [5.2 数组的类型推断](#)
如果数组变量没有声明类型，TypeScript 就会推断数组成员的类型。这时，推断行为会因为值的不同，而有所不同。

如果变量的初始值是空数组，那么 TypeScript 会推断数组类型是any[]。
```typescript
// 推断为 any[]
const arr = [];
```
后面，为这个数组赋值时，TypeScript 会自动更新类型推断。
```typescript
const arr = [];
arr; // 推断为 any[]

arr.push(123);
arr; // 推断类型为 number[]

arr.push("abc");
arr; // 推断类型为 (string|number)[]
```
上面示例中，数组变量arr的初始值是空数组，然后随着新成员的加入，TypeScript 会自动修改推断的数组类型。

但是，类型推断的自动更新只发生初始值为空数组的情况。如果初始值不是空数组，**类型推断就不会更新**。
```typescript
// 推断类型为 number[]
const arr = [123];

arr.push("abc"); // 报错
```
上面示例中，数组变量arr的初始值是[123]，TypeScript 就推断成员类型为number。新成员如果不是这个类型，TypeScript 就会报错，而不会更新类型推断。

#### [5.3 多维数组](#)
TypeScript 使用 `T[][]` 的形式，表示二维数组，T是最底层数组成员的类型。

```typescript
var multi: number[][] = [
  [1, 2, 3],
  [23, 24, 25],
];
```


#### [5.4 const 数组，const断言 、readonly 只读数组](#)
JavaScript 规定，const命令声明的数组变量是可以改变成员的。
```typescript
const arr = [0, 1];
arr[0] = 2;
```
上面示例中，修改const命令声明的数组的成员是允许的。

但是，很多时候确实有声明为只读数组的需求，即不允许变动数组成员。

TypeScript 允许声明只读数组，方法是在数组类型前面加上 readonly 关键字。
```typescript
const arr: readonly number[] = [0, 1];

arr[1] = 2; // 报错
arr.push(3); // 报错
delete arr[0]; // 报错
```
上面示例中，arr是一个只读数组，删除、修改、新增数组成员都会报错。


> TypeScript 将readonly number[]与number[]视为两种不一样的类型，后者是前者的子类型。
> 这是因为只读数组没有pop()、push()之类会改变原数组的方法，所以number[]的方法数量要多于readonly number[]，这意味着number[]其实是readonly number[]的子类型。

```typescript
let a1: number[] = [0, 1];
let a2: readonly number[] = a1; // 正确

a1 = a2; // 报错
```
上面示例中，子类型number[]可以赋值给父类型readonly number[]，但是反过来就会报错。

**注意，readonly关键字不能与数组的泛型写法一起使用**。
```typescript
// 报错
const arr: readonly Array<number> = [0, 1];
```
上面示例中，readonly与数组的泛型写法一起使用，就会报错。

实际上，TypeScript 提供了两个专门的泛型，用来生成只读数组的类型。
```typescript
const a1: ReadonlyArray<number> = [0, 1];

const a2: Readonly<number[]> = [0, 1];
```
上面示例中，泛型`ReadonlyArray<T>`和`Readonly<T[]>`都可以用来生成只读数组类型。两者尖括号里面的写法不一样，`Readonly<T[]>`的尖括号里面是整个数组（number[]），而`ReadonlyArray<T>`的尖括号里面是数组成员（number）。


只读数组还有一种声明方法，就是使用“**const 断言**。
```typescript
const arr = [0, 1] as const;

arr[0] = [2]; // 报错
```
上面示例中，as const告诉 TypeScript，推断类型时要把变量arr推断为只读数组，从而使得数组成员无法改变。