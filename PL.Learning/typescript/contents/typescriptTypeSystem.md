## [TypeScript 类型系统](#)
> **介绍**：TypeScript类型包括了 JavaScript 已经提供的内置类型，还包括了一些新类型，如元祖、any、unknown、interface...。

---


- [一、类型总结概述](#一类型总结概述)
- [二、特殊类型](#二特殊类型)
- [三基本类型](#三基本类型)
- [四、值类型和特殊类型](#四值类型和特殊类型)
- [五、数组类型](#五数组类型)
- [六、元组类型](#六元组类型)
- [七、enum](#七enum)
- [八、type命令](#八type命令)
- [九、typeof 运算符](#九typeof-运算符)


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

#### [1.1 块级类型声明](#)
TypeScript 支持块级类型声明，即类型可以声明在代码块（用大括号表示）里面，并且只在当前代码块有效。

```typescript
if (true) {
  type T = number;
  let v: T = 5;
} else {
  type T = string;
  let v: T = "hello";
}
```

#### [1.2 类型的兼容](#)
TypeScript 的类型存在兼容关系，某些类型可以兼容其他类型。

```typescript
type T = number | string;

let a: number = 1;
let b: T = a;
```
TypeScript 为这种情况定义了一个专门术语。如果类型A的值可以赋值给类型B，那么类型A就称为类型B的子类型（subtype）。

TypeScript 的一个规则是，**凡是可以使用父类型的地方，都可以使用子类型，但是反过来不行**。

```typescript
let a: "hi" = "hi";
let b: string = "hello";

b = a; // 正确
a = b; // 报错
```


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
| **是否允许原始类型**| √  | ✘ |
| **是否允许 `null`** | ✘  | ✘ |
| **推荐程度**        | **!** 不推荐用于类型安全场景   | **√** 推荐使用 |

通过合理选择 `Object` 和 `object` 类型，可以更安全、更灵活地处理 TypeScript 中的对象和值。

| 类型        | 可接受的值类型  | 是否允许原始类型 | 是否允许 `null` |
|-------------|----------------|------------------|---------|
| `Object`    | 所有值（除 `null` 和 `undefined`）| **√** | ✘ |
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

### [四、值类型和特殊类型](#)
TypeScript 规定，**单个值也是一种类型**，称为“值类型”。

```typescript
let x: "hello";

x = "hello"; // 正确
x = "world"; // 报错
```
上面示例中，变量x的类型是字符串hello，导致它只能赋值为这个字符串，赋值为其他字符串就会报错。

TypeScript 推断类型时，遇到const命令声明的变量，如果代码里面没有注明类型，就会推断该变量是值类型。
```typescript
// x 的类型是 "https"
const x = "https";

// y 的类型是 string
const y: string = "https";
```
**注意**，const命令声明的变量，如果赋值为对象，并不会推断为值类型。
```typescript
// x 的类型是 { foo: number }
const x = { foo: 1 };
//const x: {foo : number; } = { foo: 1 };
```

值类型可能会出现一些很奇怪的报错。
```typescript
const x: 5 = 4 + 1; // 报错
```
上面示例中，等号左侧的类型是数值5，等号右侧4 + 1的类型，TypeScript 推测为number。由于5是number的子类型，number是5的父类型，父类型不能赋值给子类型，所以报错了

但是，反过来是可以的，子类型可以赋值给父类型。
```typescript
let x: 5 = 5;
let y: number = 4 + 1;

x = y; // 报错
y = x; // 正确
```
如果一定要让子类型可以赋值为父类型的值，就要用到类型断言。
```typescript
const x: 5 = (4 + 1) as 5; // 正确
```

#### [4.1 联合类型](#)
联合类型（union types）指的是多个类型组成的一个新类型，使用符号 `|` 表示, 联合类型A|B表示，任何一个类型只要属于A或B，就属于联合类型A|B。

```typescript
let x: string | number;

x = 123; // 正确
x = "abc"; // 正确
```
上面示例中，变量x就是联合类型string|number，表示它的值既可以是字符串，也可以是数值。

联合类型可以与值类型相结合，表示一个变量的值有若干种可能。

```typescript
let setting: true | false;

let gender: "male" | "female";

let rainbowColor: "赤" | "橙" | "黄" | "绿" | "青" | "蓝" | "紫";
```
前面提到，打开编译选项strictNullChecks后，其他类型的变量不能赋值为undefined或null。这时，如果某个变量确实可能包含空值，就可以采用联合类型的写法。
```typescript
let name: string | null;

name = "John";
name = null;
```

如果一个变量有多种类型，读取该变量时，往往需要进行“**类型缩小**”（type narrowing），区分该值到底属于哪一种类型，然后再进一步处理。
```typescript
function printId(id: number | string) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id);
  }
}

function getPort(scheme: "http" | "https") {
  switch (scheme) {
    case "http":
      return 80;
    case "https":
      return 443;
  }
}
```

#### [4.2 交叉类型](#)
交叉类型（intersection types）指的多个类型组成的一个新类型，使用符号 `&` 表示。 交叉类型A&B表示，任何一个类型必须同时属于A和B，才属于交叉类型A&B，即交叉类型同时满足A和B的特征。
```typescript
let x: number & string;
```
上面示例中，变量x同时是数值和字符串，这当然是不可能的，所以 TypeScript 会认为x的类型实际是never。

**交叉类型的主要用途是表示对象的合成**。

```typescript
let obj: { foo: string } & { bar: string };

obj = {
  foo: "hello",
  bar: "world",
};
```
交叉类型常常用来为对象类型添加新属性。
```typescript
type A = { foo: number };

type B = A & { bar: number };
```



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

let t: Name = "tick";
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


TypeScript 将 `readonly number[]` 与 `number[]` 视为两种不一样的类型，后者是前者的子类型。

这是因为只读数组没有 `pop()`、`push()`之类会改变原数组的方法，所以 `number[]` 的方法数量要多于 `readonly number[]`，这意味着 `number[]` 其实是 `readonly number[]`的子类型。

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

### [六、元组类型](#)
元组（tuple）是 TypeScript 特有的数据类型, 元组（Tup1e）是一种特殊的数组类型，可以存储固定数量的元素，并且每个元素的类型是已知的且可以不
同。元组用于精确描述一组值的类型，`？`表示可选元素。

```typescript
let tpl: [string, number, boolean, string?];

tpl = ["JXkicker", 28, true];
```

> 元组类型的写法，与上一章的数组有一个重大差异。数组的成员类型写在方括号外面`（number[]）`，元组的成员类型是写在方括号里面`（[number]）`。


TypeScript 的区分方法是，成员类型写在方括号里面的就是元组，写在外面的就是数组。
```typescript
let a: [number] = [1];
```
上面示例中，变量a是一个元组，只有一个成员，类型是number。

#### [6.1 可选元素](#)
元组成员的类型可以添加问号后缀（?），表示该成员是可选的。
```typescript
let a: [number, number?] = [1];
```
注意，问号只能用于元组的尾部成员，也就是说，所有可选成员必须在必选成员之后。

```typescript
type myTuple = [number, number, number?, string?];
```

#### [6.2 类型推导](#)
使用元组时，必须明确给出类型声明，不能省略，否则 TypeScript 会把一个值自动推断为数组。
```typescript
// a 的类型为 (number | boolean)[]
let a = [1, true];
```

#### [6.3 不限成员数量的元组](#)
使用扩展运算符（`...`），可以表示不限成员数量的元组。
```typescript
type NamedNums = [string, ...number[]];

const a: NamedNums = ["A", 1, 2];
const b: NamedNums = ["B", 1, 2, 3];
```
扩展运算符用在元组的任意位置都可以，但是它后面只能是数组或元组。
```typescript
type t1 = [string, number, ...boolean[]];
type t2 = [string, ...boolean[], number];
type t3 = [...boolean[], string, number];
```
如果不确定元组成员的类型和数量，可以写成下面这样。
```typescript
type Tuple = [...any[]];
```

#### [6.4 读取成员类型](#)
元组可以通过方括号，读取成员类型。
```typescript
type Tuple = [string, number];
type Age = Tuple[1]; // number
```
由于元组的成员都是数值索引，即索引类型都是number，所以可以像下面这样读取。
```typescript
type Tuple = [string, number, Date];
type TupleEl = Tuple[number]; // string|number|Date
```
上面示例中，Tuple[number]表示元组Tuple的所有数值索引的成员类型，所以返回string|number|Date，即这个类型是三种值的联合类型。


#### [6.5 只读元组](#)
元组也可以是只读的，不允许修改，有两种写法。

```typescript
// 写法一
type t = readonly [number, string];

// 写法二
type t = Readonly<[number, string]>;
```
上面示例中，两种写法都可以得到只读元组，其中写法二是一个泛型，用到了工具类型 Readonly<T>。

跟数组一样，只读元组是元组的父类型。所以，元组可以替代只读元组，而只读元组不能替代元组。

```typescript
type t1 = readonly [number, number];
type t2 = [number, number];

let x: t2 = [1, 2];
let y: t1 = x; // 正确

x = y; // 报错
```

由于只读元组不能替代元组，所以会产生一些令人困惑的报错。
```typescript
function distanceFromOrigin([x, y]: [number, number]) {
  return Math.sqrt(x ** 2 + y ** 2);
}

let point = [3, 4] as const;

distanceFromOrigin(point); // 报错
```
上面示例报错的解决方法，就是使用类型断言。
```typescript
distanceFromOrigin(point as [number, number]);
```

#### [6.6 成员数量的推断](#)
如果没有可选成员和扩展运算符，TypeScript 会推断出元组的成员数量（即元组长度）。

```typescript
function f(point: [number, number]) {
  if (point.length === 3) {
    // 报错
    // ...
  }
}
```
上面示例会报错，原因是 TypeScript 发现元组point的长度是2，不可能等于3，这个判断无意义。

如果包含了可选成员，TypeScript 会推断出可能的成员数量
```typescript
function f(point: [number, number?, number?]) {
  if (point.length === 4) {
    // 报错
    // ...
  }
}
```
上面示例会报错，原因是 TypeScript 发现point.length的类型是1|2|3，不可能等于4。

如果使用了扩展运算符，TypeScript 就无法推断出成员数量。
```typescript
const myTuple: [...string[]] = ["a", "b", "c"];

if (myTuple.length === 4) {
  // 正确
  // ...
}
```

#### [6.7 扩展运算符与成员数量](#)
扩展运算符（...）将数组（注意，不是元组）转换成一个逗号分隔的序列，这时 TypeScript 会认为这个序列的成员数量是不确定的，因为数组的成员数量是不确定的。

这导致如果函数调用时，使用扩展运算符传入函数参数，可能发生参数数量与数组长度不匹配的报错。
```typescript
const arr = [1, 2];

function add(x: number, y: number) {
  // ...
}

add(...arr); // 报错
```
有些函数可以接受任意数量的参数，这时使用扩展运算符就不会报错。
```typescript
const arr = [1, 2, 3];
console.log(...arr); // 正确
```
console.log()可以接受任意数量的参数，所以传入...arr就不会报错。


解决这个问题的一个方法，就是把成员数量不确定的数组，写成成员数量确定的元组，再使用扩展运算符。
```typescript
const arr: [number, number] = [1, 2];

function add(x: number, y: number) {
  // ...
}

add(...arr); // 正确
```
上面示例中，arr是一个拥有两个成员的元组，所以 TypeScript 能够确定...arr可以匹配函数add()的参数数量，就不会报错了。

另一种写法是使用as const断言。

```typescript
const arr = [1, 2] as const;
```

### [七、enum](#)
Enum 是 TypeScript 新增的一种数据结构和类型，中文译为“枚举”, 用来将相关常量放在一个容器里面，方便使用。

```typescript
enum Color {
  Red, // 0
  Green, // 1
  Blue, // 2
}
```
使用时，调用 Enum 的某个成员，与调用对象属性的写法一样，可以使用点运算符，也可以使用方括号运算符。
```typescript
let c = Color.Green; // 1
// 等同于
let c = Color["Green"]; // 1
```

#### [7.1 Enum 成员的值](#)
Enum 成员默认不必赋值，系统会从零开始逐一递增，按照顺序为每个成员赋值，比如 0、1、2……, 但是，也可以为 Enum 成员显式赋值。

- Enum 成员值都是只读的，不能重新赋值。
- 为了让这一点更醒目，通常会在 enum 关键字前面加上const修饰，表示这是常量，不能再次赋值。

```typescript
const enum Color {
  Red,
  Green,
  Blue,
}

// 等同于
enum Color {
  Red = 0,
  Green = 1,
  Blue = 2,
}
//成员的值甚至可以相同。
enum Color {
  Red = 0,
  Green = 0,
  Blue = 0,
}
//如果只设定第一个成员的值，后面成员的值就会从这个值开始递增。
enum Color {
  Red = 7,
  Green, // 8
  Blue, // 9
}
//Enum 成员的值也可以使用计算式。
enum Permission {
  UserRead = 1 << 8,
  UserWrite = 1 << 7,
  UserExecute = 1 << 6,
  GroupRead = 1 << 5,
  GroupWrite = 1 << 4,
  GroupExecute = 1 << 3,
  AllRead = 1 << 2,
  AllWrite = 1 << 1,
  AllExecute = 1 << 0,
}
```
成员的值可以是任意数值，但不能是大整数（Bigint）。
```typescript
enum Color {
  Red = 90,
  Green = 0.5,
  Blue = 7n, // 报错
}
```


#### [7.2 同名 Enum 的合并](#)
多个同名的 Enum 结构会自动合并，同名 Enum 合并时，不能有同名成员，否则报错。

```typescript
enum Foo {
  A,
}

enum Foo {
  B = 1,
}

enum Foo {
  C = 2,
}

// 等同于
enum Foo {
  A,
  B = 1，
  C = 2
}
```

#### [7.3 字符串 Enum](#)
Enum 成员的值除了设为数值，还可以设为字符串。也就是说，Enum 也可以用作一组相关字符串的集合。

```typescript
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}
```
上面示例中，Direction就是字符串枚举，每个成员的值都是字符串。

注意，字符串枚举的所有成员值，都必须显式设置。如果没有设置，成员值默认为数值，且位置必须在字符串成员之前。
```typescript
enum Foo {
  A, // 0
  B = "hello",
  C, // 报错
}
```
上面示例中，A之前没有其他成员，所以可以不设置初始值，默认等于0；C之前有一个字符串成员，必须C必须有初始值，不赋值就报错了。

Enum 成员可以是字符串和数值混合赋值。
```typescript
enum Enum {
  One = "One",
  Two = "Two",
  Three = 3,
  Four = 4,
}
```
除了数值和字符串，Enum 成员不允许使用其他值（比如 Symbol 值）。

变量类型如果是字符串 Enum，就不能再赋值为字符串，这跟数值 Enum 不一样。
```typescript
enum MyEnum {
  One = "One",
  Two = "Two",
}

let s = MyEnum.One;
s = "One"; // 报错
```
由于这个原因，如果函数的参数类型是字符串 Enum，传参时就不能直接传入字符串，而要传入 Enum 成员。
```typescript
enum MyEnum {
  One = "One",
  Two = "Two",
}

function f(arg: MyEnum) {
  return "arg is " + arg;
}

f("One"); // 报错
```
前面说过，数值 Enum 的成员值往往不重要。但是有些场合，开发者可能希望 Enum 成员值可以保存一些有用的信息，所以 TypeScript 才设计了字符串 Enum.
```typescript
const enum MediaTypes {
  JSON = "application/json",
  XML = "application/xml",
}

const url = "localhost";

fetch(url, {
  headers: {
    Accept: MediaTypes.JSON,
  },
}).then((response) => {
  // ...
});
```

字符串 Enum 可以使用 **联合类型（union）**代替。

```typescript
function move(where: "Up" | "Down" | "Left" | "Right") {
  // ...
}
```
上面示例中，函数参数where属于联合类型，效果跟指定为字符串 Enum 是一样的。

#### [7.4 keyof 运算符](#)
keyof 运算符可以取出 Enum 结构的所有成员名，作为联合类型返回。

```typescript
enum MyEnum {
  A = "a",
  B = "b",
}

// 'A'|'B'
type Foo = keyof typeof MyEnum;
```
上面示例中，keyof typeof MyEnum可以取出MyEnum的所有成员名，所以类型Foo等同于 **联合类型** `'A'|'B'`。

注意，这里的typeof是必需的，否则keyof MyEnum相当于keyof number。
```typescript
type Foo = keyof MyEnum;
// "toString" | "toFixed" | "toExponential" |
// "toPrecision" | "valueOf" | "toLocaleString"
```
上面示例中，类型Foo等于类型number的所有原生属性名组成的联合类型。

如果要返回 Enum 所有的成员值，可以使用 `in` 运算符。
```typescript
enum MyEnum {
  A = "a",
  B = "b",
}

// { a：any, b: any }
type Foo = { [key in MyEnum]: any };
```

#### [7.5 反向映射](#)
数值 Enum 存在反向映射，即可以通过成员值获得成员名。

```typescript
enum Weekdays {
  Monday = 1,
  Tuesday,
  Wednesday,
  Thursday,
  Friday,
  Saturday,
  Sunday,
}

console.log(Weekdays[3]); // Wednesday
```
上面示例中，Enum 成员Wednesday的值等于 3，从而可以从成员值3取到对应的成员名Wednesday，这就叫反向映射。

这是因为 TypeScript 会将上面的 Enum 结构，编译成下面的 JavaScript 代码。
```typescript
var Weekdays;
(function (Weekdays) {
  Weekdays[(Weekdays["Monday"] = 1)] = "Monday";
  Weekdays[(Weekdays["Tuesday"] = 2)] = "Tuesday";
  Weekdays[(Weekdays["Wednesday"] = 3)] = "Wednesday";
  Weekdays[(Weekdays["Thursday"] = 4)] = "Thursday";
  Weekdays[(Weekdays["Friday"] = 5)] = "Friday";
  Weekdays[(Weekdays["Saturday"] = 6)] = "Saturday";
  Weekdays[(Weekdays["Sunday"] = 7)] = "Sunday";
})(Weekdays || (Weekdays = {}));
```

### [八、type命令](#)
type命令用来定义一个类型的别名。

```typescript
type Age = number;

let age: Age = 55;
```
别名的作用域是块级作用域。这意味着，代码块内部定义的别名，影响不到外部。
```typescript
type Color = "red";

if (Math.random() < 0.5) {
  type Color = "blue";
}
```
别名支持使用表达式，也可以在定义一个别名时，使用另一个别名，即别名允许嵌套。

```typescript
type World = "world";
type Greeting = `hello ${World}`;
```

### [九、typeof 运算符](#)
JavaScript 语言中，typeof 运算符是一个一元运算符，返回一个字符串，代表操作数的类型，JavaScript 里面，typeof运算符只可能返回八种结果，而且都是字符串。

```typescript
typeof undefined; // "undefined"
typeof true; // "boolean"
typeof 1337; // "number"
typeof "foo"; // "string"
typeof {}; // "object"
typeof parseInt; // "function"
typeof Symbol(); // "symbol"
typeof 127n; // "bigint"
```
TypeScript 将typeof运算符移植到了类型运算，它的操作数依然是一个值，但是返回的不是字符串，而是该值的 TypeScript 类型。
```typescript
const a = { x: 0 };

type T0 = typeof a; // { x: number }
type T1 = typeof a.x; // number
```
typeof a表示返回变量a的 TypeScript 类型（{ x: number }）。同理，typeof a.x返回的是属性x的类型（number）。

也就是说，同一段代码可能存在两种typeof运算符，一种用在值相关的 JavaScript 代码部分，另一种用在类型相关的 TypeScript 代码部分。
```typescript
let a = 1;
let b: typeof a;

if (typeof a === "number") {
  b = a;
}
```
JavaScript 的 typeof 遵守 JavaScript 规则，TypeScript 的 typeof 遵守 TypeScript 规则。它们的一个重要区别在于，编译后，前者会保留，后者会被全部删除。

由于编译时不会进行 JavaScript 的值运算，所以 TypeScript 规定，typeof 的参数只能是标识符，不能是需要运算的表达式。

```typescript
type T = typeof Date(); // 报错
```
上面示例会报错，原因是 typeof 的参数不能是一个值的运算式，而Date()需要运算才知道结果。

**另外，typeof命令的参数不能是类型**。
```typescript
type Age = number;
type MyAge = typeof Age; // 报错
```
