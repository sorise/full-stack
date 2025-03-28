## [TypeScript 入门必知](#)
**介绍**: TypeScript是JavaScript的超集，具有可选的类型并可以编译为纯JavaScript。从技术上讲TypeScript就是具有静态类型的 JavaScript 。

----

- 

----
### [一、](#)
TypeScript（简称 TS）是微软公司开发的一种基于 JavaScript （简称 JS）语言的编程语言。

命令行的TypeScript编译器可以使用Node.js包来安装:

**安装**:
```shell
npm install -g typescript
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

let user = "Jane User";

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

#### [1.1 接口](#)
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

#### [1.2 类](#)
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








