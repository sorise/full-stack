### [JavaScript ES6+ 新增类型](#)
>**介绍**：在ES6以后，新增了许多的类型。

---

- [1. BigInt 大整数类型](#1-bigint-大整数类型)

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