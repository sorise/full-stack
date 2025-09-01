### [JavaScript ES6+ 新增类型](#)
>**介绍**：在ES6以后，新增了许多的类型。

---

- [1. BigInt 大整数类型](#1-bigint-大整数类型)
- [2. WeakRef 弱引用](#2-weakref-弱引用)
- [3. FinalizationRegistry](#3-finalizationregistry)

----
### [1. BigInt 大整数类型](#)
BigInt 值表示数值，这些数值太大，无法用 number 原始类型 表示。

>JS 中的Number类型只能安全地表示-9007199254740991 (-(2^53-1)) 和9007199254740991(2^53-1)之间的整数，任何超出此范围的整数值都可能失去精度。

BigInt 值，有时也简称为 BigInt，是 bigint 原始类型，**通过在整数字面量的末尾附加 n 创建**，或者**通过调用 BigInt() 函数**（不使用 new 运算符）并向其提供整数或字符串值来创建。

```javascript
const previouslyMaxSafeInteger = 9007199254740991n;

const alsoHuge = BigInt(9007199254740991);
// 9007199254740991n

const hugeString = BigInt("9007199254740991");
// 9007199254740991n

const hugeHex = BigInt("0x1fffffffffffff");
// 9007199254740991n

const hugeOctal = BigInt("0o377777777777777777");
// 9007199254740991n

const hugeBin = BigInt(
    "0b11111111111111111111111111111111111111111111111111111",
);
```

**BigInt 值不能与内置 Math 对象中的方法一起使用，并且不能在运算中与 Number 值混合；它们必须强制转换为相同的类型**。

#### [1.1 类型信息](#)
当针对 typeof 进行测试时，BigInt 值（bigint 原始类型）将返回 "bigint"。
```javascript
typeof 1n === "bigint"; // true
typeof BigInt("1") === "bigint"; // true
```
BigInt 值也可以包装在 Object 中
```javascript
typeof Object(1n) === "object"; // true
```

#### [1.2 运算符](#)
大多数运算符都支持 BigInt，但是大多数运算符不允许操作数类型混合 - 两个操作数必须都是 BigInt 或都不是

* 算术运算符: `+`,`-`, `*`, `/`, `%`, `**`
* 位运算符: `>>`, `<<`, `&`, `|`, `^`, `~`
* 一元否定 (`-`)
* 增量/减量: `++`, `--`

**返回布尔值的运算符允许将数字和 BigInt 作为操作数混合使用**
* 关系运算符和相等运算符: `>, <, >=, <=, ==, !=, ===, !==`
* 逻辑运算符仅依赖于操作数的 **真值**

**有几个运算符根本不支持 BigInt**
* 一元加 (+) 由于在 asm.js 中存在冲突的使用而无法支持，因此已将其排除在外 以避免破坏 asm.js。
* **无符号右移 (>>>) 是唯一不支持的位运算符，因为每个 BigInt 值都是有符号的**。

**特殊情况**
* 涉及字符串和 BigInt 的加法 (+) 返回字符串。
* 除法 (`/`) 将小数部分截断为零，因为 BigInt 无法表示小数。

```javascript
const previousMaxSafe = BigInt(Number.MAX_SAFE_INTEGER); // 9007199254740991n
const maxPlusOne = previousMaxSafe + 1n; // 9007199254740992n
const theFuture = previousMaxSafe + 2n; // 9007199254740993n, this works now!
const multi = previousMaxSafe * 2n; // 18014398509481982n
const subtr = multi - 10n; // 18014398509481972n
const mod = multi % 10n; // 2n

const bigN = 2n ** 54n; // 18014398509481984n
bigN * -1n; // -18014398509481984n
const expected = 4n / 2n; // 2n
const truncated = 5n / 2n; // 2n, not 2.5n
```

#### [1.3 比较](#)
BigInt 值与 Number 值不严格相等，但是松散相等的。
```javascript
0n === 0; // false
0n == 0; // true
```
Number 值和 BigInt 值可以像往常一样进行比较
```javascript
1n < 2; // true
2n > 1; // true
2 > 2; // false
2n > 2; // false
2n >= 2; // true
```
由于在 Number 值和 BigInt 值之间强制转换会导致精度丢失，因此建议执行以下操作

* 仅当合理预期大于 2^53 的值时，才使用 BigInt 值。
* 不要在 BigInt 值和 Number 值之间强制转换。

#### [1.4 条件语句](#)
当 BigInt 值转换为 Boolean 时，它遵循与数字相同的转换规则：

* 通过 Boolean 函数；
* 当与 逻辑运算符 `||`、`&&` 和 `!` 一起使用时；或者
* 在条件测试中（例如 if 语句）中。

```javascript
if (0n) {
  console.log("Hello from the if!");
} else {
  console.log("Hello from the else!");
}
// "Hello from the else!"

0n || 12n; // 12n
0n && 12n; // 0n
Boolean(0n); // false
Boolean(12n); // true
!12n; // false
!0n; // true
```

#### [1.5 静态方法](#)
BigInt.asIntN(size, x) 静态方法将 BigInt 值截断到给定的最低有效位数，并将其值作为有符号整数返回。
* size: 应为 0 到 25^3 - 1之间的整数（含）。，表示位数。
* x: 要转换的 BigInt 值。

BigInt.asUintN(size, x) 静态方法将 BigInt 值截断为给定数量的最低有效位，并将该值作为无符号整数返回。
* size: 应为 0 到 25^3 - 1之间的整数（含）。，表示位数。
* x: 要转换的 BigInt 值。

```javascript
console.log(BigInt.asUintN(8, 10n)); // 输出: 10n
console.log(BigInt.asUintN(8, -1n)); // 输出: 255n (因为 -1 模 256 = 255)

console.log(BigInt.asIntN(8, 10n)); // 输出: 10n
console.log(BigInt.asIntN(8, 255n)); // 输出: -1n (因为 255 补码表示为 -1 在 8 位上)
```
**解释**：
```javascript
const max = 2n ** (64n - 1n) - 1n;

BigInt.asIntN(64, max); // 9223372036854775807n

BigInt.asIntN(64, max + 1n); // -9223372036854775808n
// negative because the 64th bit of 2^63 is 1
```


### [2. WeakRef 弱引用](#)
[WeakRef](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakRef) 对象允许你保留对另一个对象的弱引用，但不会阻止垃圾回收（GC）清理被弱引用的对象。

> WeakRef 对象包含对对象的弱引用，这个弱引用被称为该 WeakRef 对象的 target 或者是 referent。对象的弱引用是指该引用不会阻止 GC 回收这个对象。而与此相反的，一个普通的引用（或者说强引用）会将与之对应的对象保存在内存中。只有当该对象没有任何的强引用时，JavaScript 引擎 GC 才会销毁该对象并且回收该对象所占的内存空间。如果上述情况发生了，那么你就无法通过任何的弱引用来获取该对象。


`WeakRef.prototype.deref()`  deref方法返回WeakRef 实例的目标对象，如果目标对象已被垃圾收集，则返回undefined 。

```javascript
const tick = () => {
  // Get the element from the weak reference, if it still exists
  const element = this.ref.deref();
  if (element) {
    element.textContent = ++this.count;
  } else {
    // The element doesn't exist anymore
    console.log("The element is gone.");
    this.stop();
    this.ref = null;
  }
};
```
例子演示了在一个 DOM 元素中启动一个计数器，当这个元素不存在时停止：
```javascript
class Counter {
  constructor(element) {
    // Remember a weak reference to the DOM element
    this.ref = new WeakRef(element);
    this.start();
  }

  start() {
    if (this.timer) {
      return;
    }

    this.count = 0;

    const tick = () => {
      // Get the element from the weak reference, if it still exists
      const element = this.ref.deref();
      if (element) {
        element.textContent = ++this.count;
      } else {
        // The element doesn't exist anymore
        console.log("The element is gone.");
        this.stop();
        this.ref = null;
      }
    };

    tick();
    this.timer = setInterval(tick, 1000);
  }

  stop() {
    if (this.timer) {
      clearInterval(this.timer);
      this.timer = 0;
    }
  }
}

const counter = new Counter(document.getElementById("counter"));
counter.start();
setTimeout(() => {
  document.getElementById("counter").remove();
}, 5000);
```


### [3. FinalizationRegistry](#)
FinalizationRegistry 是 ECMAScript 2021（ES12）引入的新特性。

一句话来概括就是提供了一种在对象被垃圾回收时执行清理操作（回调函数）的机制，有两个 API，来看一段使用的示例。

```javascript
const finalRegistry = new FinalizationRegistry((value) => {
  console.log(
    "对象被垃圾回收时触发，value 为 register 方法的第二个参数，不传的话为 undefined",
    value
  );
});

let a = { name: "a" };
let b = { age: 18 };
let c = { hobby: "game" };

finalRegistry.register(a); //注册对象
finalRegistry.register(b, "this is b"); // 第二个参数会被当成回调函数的参数
finalRegistry.register(c, "this is c", c); // 第三个参数用于取消监听

a = null;
// 打印 undefined
b = null;
// 打印 this is b

// 取消注册回调函数
finalRegistry.unregister(c);

// 不会打印
c = null;
```

#### [3.1 register](#)
FinalizationRegistry 实例的 register() 方法会将一个值注册到此 FinalizationRegistry 中，以便在该值被垃圾回收时，可能会调用该 registry 的回调函数。

```javascript
register(target, heldValue)
register(target, heldValue, unregisterToken)
```

接受三个参数
- `target`：必传，监听的对象
- `heldValue`：必传，触发回调函数的时候会将此值传递过去（但是其实也可以不传，默认就是 undefined，不知道为什么 MDN 上标记为必传 🤷）
- `unregisterToken`：如果想使用 unregister 方法去取消监听回收事件的话，就传递第三个参数，FinalizationRegistry 会保持对他的弱引用，通常使用目标值本身作为注销令牌

#### [3.2 unregister](#)
接受一个参数，取消监听，FinalizationRegistry 实例的 unregister() 方法会从此 FinalizationRegistry 中注销一个目标值。

```javascript
unregister(unregisterToken)
```

- unregisterToken： 就是 register 的第三个参数即可