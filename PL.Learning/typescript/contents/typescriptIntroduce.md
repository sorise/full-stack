## [TypeScript 入门必知](#)
**介绍**: TypeScript是JavaScript的超集，具有可选的类型并可以编译为纯JavaScript。从技术上讲TypeScript就是具有静态类型的 JavaScript 。

----

- [一、入门介绍](#一-入门介绍)
- [二、编译方式](#二编译方式)
- [三、基本用法](#三-基本用法)
- [四、类型推断](#四-类型推断)

----
### [一、入门介绍](#)
TypeScript（简称 TS）是微软公司开发的一种基于 JavaScript （简称 JS）语言的编程语言, 具有强大的**静态类型检查功能**。
TS 的发展形势非常好，至今很多 JavaScript 项目都支持 TS，比如 Vue3 和 React17+ 前端两大框架都支持 TS。

**静态类型的好处**: 
- 有了静态类型，不必运行代码，就可以确定变量的类型，从而推断代码有没有错误。这就叫做代码的静态分析。
- 由于每个值、每个变量、每个运算符都有严格的类型约束，TypeScript 就能轻松发现拼写错误、语义错误和方法调用错误，节省程序员的时间。


```javascript
function hello() {
  return "hello world";
}

hello().find("hello"); // 报错
```
上面示例中，hello()返回的是一个字符串，TypeScript 发现字符串没有find()方法，所以报错了。如果是 JavaScript，只有到运行阶段才会报错。

**静态类型的缺点**:
- 丧失了动态类型的代码灵活性, 动态类型有非常高的灵活性，给予程序员很大的自由，静态类型将这些灵活性都剥夺了。
- 增加了编程工作量，有了类型之后，程序员不仅需要编写功能，还需要编写类型声明，确保类型正确。这增加了不少工作量，有时会显著拖长项目的开发时间。
- 引入了独立的编译步骤。 原生的 JavaScript 代码，可以直接在 JavaScript 引擎运行。添加类型系统以后，就多出了一个单独的编译步骤，检查类型是否正确，并将 TypeScript 代码转成 JavaScript 代码，这样才能运行。


**安装**: 命令行的TypeScript编译器可以使用Node.js包来安装:

```shell
npm install -g typescript

# 或者 tsc --version
$ tsc -v

#Version 5.1.6
```
**编译**:
```shell
tsc helloworld.ts
```

构建你的第一个TypeScript文件，在编辑器，将下面的代码输入到greeter.ts文件里。
```shell
function greeter(person) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.innerHTML = greeter(user);
```
**编译代码**： 我们使用了.ts扩展名，但是这段代码仅仅是JavaScript而已。 你可以直接从现有的JavaScript应用里复制/粘贴这段代码。
在命令行上，运行TypeScript编译器：
```shell
tsc greeter.ts
```
输出结果为一个greeter.js文件，它包含了和输入文件中相同的JavsScript代码。 一切准备就绪，我们可以运行这个使用TypeScript写的JavaScript应用了！

接下来让我们看看TypeScript工具带来的高级功能。 给person函数的参数添加: string类型注解，如下：
```javascript
function greeter(person: string) {
    return "Hello, " + person;
}

let user: string = "Jane User";

document.body.innerHTML = greeter(user);
```
**类型注解**: TypeScript里的类型注解是一种轻量级的为函数或变量添加约束的方式。 在这个例子里，我们希望 greeter函数接收一个字符串
参数。 然后尝试把 greeter的调用改成传入一个数组：
```javascript
function greeter(person: string) {
    return "Hello, " + person;
}

let user = [0, 1, 2];

document.body.innerHTML = greeter(user);
```
重新编译，你会看到产生了一个错误。
```
greeter.ts(7,26): error TS2345: Argument of type 'number[]' is not assignable to parameter of
type 'string'.
```
类似地，尝试删除greeter调用的所有参数，TypeScript会告诉你使用了非期望个数的参数调用了这个函数。 
在这两种情况中，TypeScript提供了静态的代码分析，它可以分析代码结构和提供的类型注解。

要注意的是尽管有错误，greeter.js文件还是被创建了。 就算你的代码里有错误，你仍然可以使用TypeScript。
但在这种情况下，TypeScript会警告你代码可能不会按预期执行。

**接口**

让我们开发这个示例应用。这里我们使用接口来描述一个拥有firstName和lastName字段的对象。 
在TypeScript里，只在两个类型内部的结构兼容那么这两个类型就是兼容的。 这就允许我们在
实现接口时候只要保证包含了接口要求的结构就可以，而不必明确地使用 implements语句。

```typescript
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = { firstName: "Jane", lastName: "User" };

document.body.innerHTML = greeter(user);
```

**类**

使用类来改写这个例子。 TypeScript支持JavaScript的新特性，比如支持基于类的面向对象编程。

```typescript
class Student {
    fullName: string;
    constructor(public firstName, public middleInitial, public lastName) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person : Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
```
让我们创建一个Student类，它带有一个构造函数和一些公共字段。 
注意类和接口可以一起共作，程序员可以自行决定抽象的级别。

重新运行tsc greeter.ts，你会看到生成的JavaScript代码和原先的一样，TypeScript里的类只是JavaScript里常用的基于原型面向对象编程的简写。


### [二、编译方式](#)
TypeScript 想要运行首先需要编译为JavaScript，官方提供了两种编译方式，命令行编译（手动编译），自动化编译方式。

> TypeScript 编译器（ tsc ）是随 TypeScript 安装的官方 TypeScript 编译器。它可用于将 TypeScript 文件编译为 JavaScript 文件。可以使用 --watch 选项与 tsc 一起，以监听 .ts 文件的更改，然后自动编译它们。

手动编译方式就是直接使用tsc命令,首先需要全局安装 typescript 
```shell
#安装编译器
npm i typescript -g
#编译
tsc index.ts
```

#### [2.1 自动化编译](#)
TypeScript 允许将tsc的编译参数，写在配置文件tsconfig.json。只要当前目录有这个文件，tsc就会自动读取，所以运行时可以不写参数。

首先打开目标文件夹，打开cmd，输入命令：

```shell
tsc --init
```
然后会生成一个json文件。
```json
{
  "compilerOptions": {
    "target": "es2016",
    "module": "commonjs",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true,
    "outDir": "dist"
  },
  "include": ["src"]
}
```
在 tsconfig.json 文件中找到 “outDir”，并且修改其值为刚刚新建的文件夹路径。

```shell
tsc --watch
```
当出现错误的时候不提交。
```shell
tsc --noEmitOnError --watch
```

#### [2.2 什么是 tsconfig.json](#)
目录中存在 tsconfig.json 文件表明该目录是 TypeScript 项目的根目录。tsconfig.json 文件指定编译项目所需的根文件和编译器选项。

```
{
  "compilerOptions": {
    "target": "esnext", // 编译后生成的版本
    "module": "esnext", // 用于指定 TypeScript 编译后生成的模块系统的类型
    "moduleResolution": "node", // 模块解析策略，ts默认用node的解析策略，即相对的方式导入
    "strict": true, // 严格模式
    "noImplicitThis": false // this指向可以是不确定的
    "forceConsistentCasingInFileNames": true, // 严格区分文件名称大小写
    "allowSyntheticDefaultImports": true, // 模块没有默认导出时，也可以使用import a from b
    "strictFunctionTypes": false, // 函数类型是否严格规范（是否允许传入子类型）
    // preserve:生成代码中会保留JSX以供后续的转换操作使用(比如：Babel).另外,输出文件会带有.jsx扩展名。一般我们会使用babel去处理jsx，不使用tsc去编译。
    "jsx": "preserve", // 指定 JSX 的处理方式。
    "baseUrl": ".", // 表示路径查找前缀，如果是"./src",那么在项目中引入的src目录下的文件只需要写对应的文件名称即可。不需要加上src
    "allowJs": true, // 是否允许编译javascript文件
    "sourceMap": true, // 是否生成sourcemap
    // 开启`esModuleInterop`后会默认开启`allowSyntheticDefaultImports`选项
    "esModuleInterop": true,
    "resolveJsonModule": true, // 是否可以导入json文件解析
    "noUnusedLocals": true, // 是否检查未使用的局部变量。
    "noUnusedParameters": true, // 是否检查未使用的参数。
    "experimentalDecorators": true, // 开启装饰器
    // 指定内置API声明组的列表，
    "lib": ["dom", "esnext"], // 指定要包含在编译中的库文件。就是.d.ts声明文件
    // "指定要包含的类型包名，而不需要在源文件中引用"
    "types": ["vite/client", "jest"],
    // 查找类型定义文件。指定`.d.ts`文件的查找路径。如果指定则只会查找当前指定的目录下的.d.ts文件
    "typeRoots": ["./node_modules/@types/", "./types"],
    "noImplicitAny": false, // 是否禁止隐式 any 类型。
    "skipLibCheck": true, // 是否跳过检查库文件。
    // 配置路径别名，这里配置完毕后还需要在vue.config.ts或者vite.config.ts中的resolve.alias中配置。
    "paths": { 
      "/@/*": ["src/*"],
      "/#/*": ["types/*"]
    }
  },
  // 指定需要编译的文件路径或文件夹路径。
  "include": [
    "tests/**/*.ts",
    "src/**/*.ts",
    "src/**/*.d.ts",
    "src/**/*.tsx",
    "src/**/*.vue",
    "types/**/*.d.ts",
    "types/**/*.ts",
    "build/**/*.ts",
    "build/**/*.d.ts",
    "mock/**/*.ts",
    "vite.config.ts"
  ],
  // 排除需要编译的文件
  "exclude": ["node_modules", "tests/server/**/*.ts", "dist", "**/*.js"]
}
```

- 通过在没有输入文件的情况下调用 tsc，在这种情况下，编译器会从当前目录开始搜索 tsconfig.json 文件，并继续沿父目录链向上。
- 通过在没有输入文件和 --project（或只是 -p）命令行选项的情况下调用 tsc，该选项指定包含 tsconfig.json 文件的目录的路径，或包含配置的有效 .json 文件的路径。

> 在命令行上指定输入文件时，tsconfig.json 文件将被忽略。


**示例**

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "noImplicitAny": true,
    "removeComments": true,
    "preserveConstEnums": true,
    "sourceMap": true
  },
  "files": [
    "core.ts",
    "sys.ts",
    "types.ts",
    "scanner.ts",
    "parser.ts",
    "utilities.ts",
    "binder.ts",
    "checker.ts",
    "emitter.ts",
    "program.ts",
    "commandLineParser.ts",
    "tsc.ts",
    "diagnosticInformationMap.generated.ts"
  ]
}
```
使用 `include` 和 `exclude` 属性

```json
{
  "compilerOptions": {
    "module": "system",
    "noImplicitAny": true,
    "removeComments": true,
    "preserveConstEnums": true,
    "outFile": "../../built/local/tsc.js",
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "**/*.spec.ts"]
}
```

#### [2.3 tsc cli 选项](#)
[tsc cli 选项](https://nodejs.cn/typescript/project-config/compiler-options/) 在本地运行 tsc 将编译由 tsconfig.json 定义的最接近的项目，您可以通过传入一个您想要的文件来编译一组 TypeScript 文件。

```shell
## Run a compile based on a backwards look through the fs for a tsconfig.json
tsc

## Emit JS for just the index.ts with the compiler defaults
tsc index.ts

## Emit JS for any .ts files in the folder src, with the default settings
tsc src/*.ts

## Emit files referenced in with the compiler settings from tsconfig.production.json
tsc --project tsconfig.production.json

## Emit d.ts files for a js file with showing compiler options which are booleans
tsc index.js --declaration --emitDeclarationOnly

## Emit a single .js file from two files via compiler options which take string arguments
tsc app.ts util.ts --target esnext --outfile index.js
```

**noImplicitAny**: 然而，使用 any 通常会破坏使用 TypeScript 的初衷。您的程序类型越多，您获得的验证和工具就越多，这意味着您在编写代码时遇到的错误就越少。打开 noImplicitAny 标志将对任何类型隐式推断为 any 的变量触发错误。

**strictNullChecks**:  默认情况下，像 null 和 undefined 这样的值可以分配给任何其他类型。这可以使编写一些代码更容易，但忘记处理 null 和 undefined 是世界上无数错误的原因 - 有些人认为它是 十亿美元的错误！strictNullChecks 标志使处理 null 和 undefined 更加明确，让我们不必担心是否忘记处理 null 和 undefined。

#### [2.4 tsconfig.json 常用属性说明](#)
以下参数既可以通过tsc命令传递，也可以通过  `tsconfig.json`， 有了这个配置文件，编译时直接调用tsc命令就可以了，不必再传递参数。

**--outFile** ： 如果想将多个 TypeScript 脚本编译成一个 JavaScript 文件，使用--outFile参数。
```shell
 tsc file1.ts file2.ts --outFile app.js
 #上面命令将file1.ts和file2.ts两个脚本编译成一个 JavaScript 文件app.js。
```
**--outDir** : 编译结果默认都保存在当前目录，--outDir参数可以指定保存到其他目录。
```shell
tsc app.ts --outDir dist
#上面命令会在dist子目录下生成app.js。
```
**--target**: 为了保证编译结果能在各种 JavaScript 引擎运行，tsc 默认会将 TypeScript 代码编译成很低版本的 JavaScript，即 3.0 版本（以es3表示）。这通常不是我们想要的结果。这时可以使用--target参数，指定编译后的 JavaScript 版本。建议使用es2015，或者更新版本。
```shell
tsc --target es2015 app.ts
```


### [三. 基本用法](#)
TypeScript 代码最明显的特征，就是为 JavaScript 变量加上了类型声明。

```typescript
let foo: string;
```
上面示例中，变量foo的后面使用冒号，声明了它的类型为string。

类型声明的写法，一律为在标识符后面添加“冒号 + 类型”。函数参数和返回值，也是这样来声明类型。

```typescript
function toString(num: number): string {
  return String(num);
}

function count(x: number, y: number) {
    return x + y;
}
```
上面示例中，函数toString()的参数num的类型是number。参数列表的圆括号后面，声明了返回值的类型是string。更详细的介绍，参见《函数》一章。

注意，变量的值应该与声明的类型一致，如果不一致，TypeScript 就会报错。

```typescript
// 报错
let foo: string = 123;
```
上面示例中，变量foo的类型是字符串，但是赋值为数值123，TypeScript 就报错了。

另外，TypeScript 规定，**变量只有赋值后才能使用，否则就会报错**。


```typescript
let x: number;
console.log(x); // 报错
```
上面示例中，变量x没有赋值就被读取，导致报错。而 JavaScript 允许这种行为，不会报错，没有赋值的变量会返回 undefined。

#### [3.1 字面量类型](#)
TypeScript中的字面量类型是一种非常强大的特性，**它允许你使用具体的值作为类型**。这可以用于限制变量只能取某些特定的值，从而使得代码更加健壮和易于理解。

除了通用类型 string 和 number 之外，我们还可以在类型位置引用特定的字符串和数字。var 和 let 都允许更改变量中保存的内容，而 const 不允许。这反映在 TypeScript 如何为字面创建类型。

```typescript
let a: 75|65 = 65;

console.log(a);
```

**字符串字面量类型**：你可以指定一个或多个字符串作为类型。
```typescript
type EDirection = "North" | "South" | "East" | "West";
let direction: EDirection = "North"; // 正确
direction = "Northwest"; // 错误
```

**数字字面量类型**：类似于字符串字面量，但是是用数字来定义的。
```typescript
type HTTPStatusCode = 200 | 404 | 500;
let statusCode: HTTPStatusCode = 200; // 正确
statusCode = 300; // 错误
```

**布尔字面量类型**：虽然不常见，但也可以使用`true`或`false`作为类型。
```typescript
type WindowState = true | false;
let isOpen: WindowState = true; // 正确
```

**枚举成员类型**：当与枚举一起使用时，可以将枚举的值作为类型的一部分。
```typescript
enum Status {
    Ready,
    Waiting
}

let status: Status.Ready | Status.Waiting = Status.Ready;
```
但是通过将字面组合成联合，你可以表达一个更有用的概念——例如，只接受一组已知值的函数：
```typescript
function compare(a: string, b: string): -1 | 0 | 1 {
  return a === b ? 0 : a > b ? 1 : -1;
}
```
数字字面类型的工作方式相同：
```typescript
function printText(s: string, alignment: "left" | "right" | "center") {
  // ...
}
```
当然，您可以将这些与非字面类型结合使用：
```typescript
interface Options {
  width: number;
}

function configure(x: Options | "auto") {
  // ...
}
configure({ width: 100 });
configure("auto");
configure("automatic");
```

### [四. 类型推断](#)
类型声明并不是必需的，如果没有，TypeScript 会自己推断类型。
```typescript
let foo = 123;
```
上面示例中，变量foo并没有类型声明，TypeScript 就会推断它的类型。由于它被赋值为一个数值，因此 TypeScript 推断它的类型为number。

后面，如果变量foo更改为其他类型的值，跟推断的类型不一致，TypeScript 就会报错。
```typescript
let foo = 123;
foo = "hello"; // 报错
```
上面示例中，变量foo的类型推断为number，后面赋值为字符串，TypeScript 就报错了。

TypeScript 也可以推断函数的返回值。

```typescript
function toString(num: number) {
  return String(num);
}
```
上面示例中，函数toString()没有声明返回值的类型，但是 TypeScript 推断返回的是字符串。正是因为 TypeScript 的类型推断，所以函数返回值的类型通常是省略不写的。

#### [4.1 值与类型](#)
学习 TypeScript 需要分清楚“值”（value）和“类型”（type）。

“类型”是针对“值”的，可以视为是后者的一个元属性。每一个值在 TypeScript 里面都是有类型的。比如，3是一个值，它的类型是number。

TypeScript 代码只涉及类型，不涉及值。所有跟“值”相关的处理，都由 JavaScript 完成。

TypeScript 项目里面，其实存在两种代码，一种是底层的“值代码”，另一种是上层的“类型代码”。前者使用 JavaScript 语法，后者使用 TypeScript 的类型语法。

它们是可以分离的，TypeScript 的编译过程，实际上就是把“类型代码”全部拿掉，只保留“值代码”。

编写 TypeScript 项目时，不要混淆哪些是值代码，哪些是类型代码。