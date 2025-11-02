## [Node Buffer](#)
**介绍**：在 Node.js 中，Buffer 是处理二进制数据的重要工具。无论是读取文件、处理网络请求，还是处理流数据，Buffer 都扮演着至关重要的角色。

----

- [Buffer 基本概念](#1-buffer-基本概念)


---
### [1. buffer 基本概念](#)
JS原生缺乏二进制处理能力，需处理TCP流、大文件时更高效，于是Buffer诞生了，**Buffer是固定长度的二进制数据容器，独立于V8堆内存，直接操作物理内存**。‌‌

> Buffer 是Node.JS的内置对象

**使用场景**:
- 文件系统读写时不指定编码返回Buffer。‌‌
- 网络传输中处理TCP流数据。
- 图像处理时修改像素数据。‌‌
- 大文本操作时直接修改内存提升性能。‌‌

#### [1.1 Buffer 与字符编码](#)
Buffer 实例一般用于表示编码字符的序列，比如 UTF-8 、 UCS2 、 Base64 或十六进制编码的数据。通过使用显式的[字符编码](https://nodejs.org/docs/latest/api/buffer.html#buffers-and-character-encodings)，就可以在 Buffer 实例与普通的 JavaScript 字符串之间进行相互转换, 如果未指定字符编码，则**默认使用 UTF-8**。

> Node.js 缓冲区接受其收到的编码字符串的**所有大小写变体**，例如，UTF-8 可以指定为'utf8'、'UTF8'或'uTf8'。

[Node.JS支持的字符编码](https://nodejs.org/docs/latest/api/buffer.html#buffers-and-character-encodings)：
- `utf8`(别名：`utf-8`)：多字节编码的 Unicode 字符。
- `utf16le`：多字节编码的 Unicode 字符。与 不同'utf8'，字符串中的每个字符将使用 2 个或 4 个字节进行编码。Node.js 仅支持UTF-16的 小端变体。
- `latin1` 代表ISO-8859-1,[详细介绍](./knowledge/EncodingLatin1.md)。
- `base64`: Base64编码。base64 编码字符串中包含的空格、制表符和换行符等空白字符将被忽略
- `base64url`: 从字符串创建 URL 时，此编码也能正确接受常规的 base64 编码字符串。
- `hex`：将每个字节编码为两个十六进制字符。解码并非完全由偶数个十六进制字符组成的字符串时，可能会发生数据截断。
- `ascii` 仅适用于 7 位ASCII数据。
- `binary` 别名`latin1`。此编码的名称可能极具误导性，因为此处列出的所有编码都用于在字符串和二进制数据之间进行转换。对于字符串和二进制数据之间的转换Buffer，通常情况下，'utf8'才是正确的选择。
- `ucs2`, 别名`utf16le`。UCS-2 曾指代 UTF-16 的一种变体，不支持代码点大于 U+FFFF 的字符。在 Node.js 中，这些代码点始终受支持。
 
#### [1.2 Buffer和类型数组](#)
Buffer实例也是 JavaScript 的`<Uint8Array> `和`<TypedArray>` 实例, 一定要牢记Buffer就是**Uint8Array**。
> [类型数组请看](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)

可以将Buffer缓存区创建的内存空间分配给定性数组。
```javascript
const { Buffer } = require('node:buffer');

const buf = Buffer.from([1, 2, 3, 4]);

console.log(buf.byteLength); //4
console.log(buf); //<Buffer 01 02 03 04>

const uint32array = new Uint32Array(buf);

console.log(uint32array); //Uint32Array(4) [ 1, 2, 3, 4 ]
console.log(uint32array.byteLength); //16
```
解释一下，[为什么 `byteLength` 从 4 变成了 16？](./knowledge/BufferAndTypeArray.md)
```javascript
const { Buffer } = require('node:buffer');

const buf = Buffer.from('hello', 'utf16le');
const uint16array = new Uint16Array(
  buf.buffer,
  buf.byteOffset,
  buf.length / Uint16Array.BYTES_PER_ELEMENT);

console.log(uint16array);

// Prints: Uint16Array(5) [ 104, 101, 108, 108, 111 ]
```

#### [1.3 创建 Buffer 类](#)
在Node.js中，Buffer类是核心模块的一部分，无需手动创建。但你可以使用它来创建Buffer实例并处理二进制数据。下面是一个简单的示例：
```javascript
// 引入Buffer模块
const { Buffer } = require('buffer');

// 创建一个Buffer实例
let buf = Buffer.from('Hello, world!', 'utf8');

// 输出Buffer实例的内容
console.log(buf.toString('utf8')); // 输出: "Hello, world!"

// 将Buffer实例转换为十六进制字符串
console.log(buf.toString('hex')); // 输出: "48656c6c6f2c20776f726c6421"
```
**通过指定大小创建空的 Buffer**:
```javascript
const buf = Buffer.alloc(10); // 创建一个包含10个字节的空Buffer
console.log(buf); // 输出: <Buffer 00 00 00 00 00 00 00 00 00 00>
```
**通过已有的数据创建 Buffer**:
```javascript
const buf = Buffer.from('Hello, Node.js!');
console.log(buf); // 输出: <Buffer 48 65 6c 6c 6f 2c 20 4e 6f 64 65 2e 6a 73 21>
```
**通过指定数组创建 Buffer**:
```javascript
const buf = Buffer.from([72, 101, 108, 108, 111]);
console.log(buf.toString()); // 输出: "Hello"
```

#### [1.3.1 Buffer.alloc()](#)
**Buffer.alloc(size[, fill[, encoding]])** 一种带初始化的创建Buffer的方式。
- size 整数,新长度的期望值Buffer。
- fill `<string> | <Buffer> | <Uint8Array> | <integer>` 用于预填充新值Buffer 。默认值： 0 .
- encoding `<string>` 如果fill是一个字符串，则这是它的编码。 默认值： 'utf8' .

#### [1.3.2 Buffer.allocUnsafe()](#)
**Buffer.allocUnsafe(size)**,一种不带初始化的创建Buffer的方式。
```javascript
const buf2 = Buffer.allocUnsafe(10);
console.log(buf2); // <Buffer 内容不确定>
```
#### [1.3.3 from创建](#)
Buffer.from() 是 Node.js 中创建 Buffer 对象最常用、最安全的方法之一。它是一个重载函数，根据传入参数的不同类型，会以不同的方式创建 Buffer。

**Buffer.from(array)**,一个由 0 到 255 之间的整数组成的数组。超出范围的值会被截断（取低8位）。  
**Buffer.from(arrayBuffer[, byteOffset[, length]])**,从一个 ArrayBuffer 对象创建 Buffer，可以指定共享内存的偏移量和长度。
**Buffer.from(buffer)**,从另一个 Buffer 实例创建一个新的 Buffer —— 内容拷贝。  
**Buffer.from(object[, offsetOrEncoding[, length]])**,尝试从一个 类数组对象（TypedArray, DataView 等） 创建 Buffer。  
**Buffer.from(string[, encoding])**,从一个 字符串 创建 Buffer，使用指定的字符编码。  

```javascript
// 从数组创建 Buffer
const buf3 = Buffer.from([0x62, 0x75, 0x66, 0x66, 0x65, 0x72]);
console.log(buf3); // <Buffer 62 75 66 66 65 72>

const uint8 = new Uint8Array([1, 2, 3]);
const buf = Buffer.from(uint8);
console.log(buf); // <Buffer 01 02 03>

const buffer = new ArrayBuffer(8);
const view = new DataView(buffer);
view.setUint8(0, 10);
view.setUint8(1, 20);

const buf2 = Buffer.from(view, 0, 2); // 偏移0，长度2
console.log(buf2); // <Buffer 0a 14>
```

#### [1.4  Buffer 的常用方法](#)
`toString([encoding], [start], [end])` 将 Buffer 转换为字符串。encoding 这边就会之前介绍的各种编码。
```javascript
const buf = Buffer.from('Hello, World!');

console.log(buf.toString()); // 输出: "Hello, World!"
console.log(buf.toString('utf8')); // 输出: "Hello, World!"
console.log(buf.toString('hex')); // 输出: "48656c6c6f2c20576f726c6421"
console.log(buf.toString('base64')); // 输出: "SGVsbG8sIFdvcmxkIQ=="
console.log(buf.toString('ascii')); // 输出: "Hello, World!"
```
`buf.write(string, [offset], [length], [encoding])`  将字符串写入 Buffer 中，指定偏移量和编码。
```javascript
const buf = Buffer.alloc(20);
buf.write('Hello');
console.log(buf.toString()); // 输出: "Hello"
```
`buf.slice([start], [end])` 创建一个新的 Buffer，它是原 Buffer 的一部分。
```javascript
const buf = Buffer.from('Hello, Node.js!');
const slice = buf.slice(0, 5);
console.log(slice.toString()); // 输出: "Hello"
```
`buf.copy(targetBuffer, [targetStart], [sourceStart], [sourceEnd])` 将数据从一个 Buffer 复制到另一个 Buffer 中。
```javascript
const buf1 = Buffer.from('Hello');
const buf2 = Buffer.alloc(5);
buf1.copy(buf2);

console.log(buf2.toString()); // 输出: "Hello"
```
`buf.equals(otherBuffer)` 判断两个 Buffer 是否相等。
```javascript
const buf1 = Buffer.from('Hello');
const buf2 = Buffer.from('Hello');
console.log(buf1.equals(buf2)); // 输出: true
```

#### [1.5  Buffer 实例方法列表](#)

| 方法 | 描述 | 版本引入 |
|------|------|----------|
| `buf.length` | Buffer 的字节长度 | 0.1.90 |
| `buf.write()` | 向 Buffer 写入字符串 | 0.1.90 |
| `buf.toString()` | 将 Buffer 解码为字符串 | 0.1.90 |
| `buf.toJSON()` | 返回 Buffer 的 JSON 表示 | 0.9.2 |
| `buf.equals()` | 比较两个 Buffer 是否相等 | 0.11.13 |
| `buf.compare()` | 比较 Buffer 并返回排序顺序 | 0.11.13 |
| `buf.copy()` | 复制 Buffer 数据 | 0.1.90 |
| `buf.slice()` | 创建 Buffer 的部分视图 | 0.3.0 |
| `buf.includes()` | 检查 Buffer 是否包含值 | 5.3.0 |
| `buf.indexOf()` | 返回值的首次出现位置 | 1.5.0 |
| `buf.lastIndexOf()` | 返回值的最后出现位置 | 6.0.0 |
| `buf.fill()` | 用指定值填充 Buffer | 0.5.0 |
| `buf.readUIntLE()` | 读取无符号小端整数 | 0.5.5 |
| `buf.readUIntBE()` | 读取无符号大端整数 | 0.5.5 |
| `buf.readIntLE()` | 读取有符号小端整数 | 0.11.15 |
| `buf.readIntBE()` | 读取有符号大端整数 | 0.11.15 |
| `buf.readDoubleBE()` | 读取大端64位双精度浮点数 | 0.11.15 |
| `buf.readDoubleLE()` | 读取小端64位双精度浮点数 | 0.11.15 |
| `buf.readFloatBE()` | 读取大端32位浮点数 | 0.11.15 |
| `buf.readFloatLE()` | 读取小端32位浮点数 | 0.11.15 |
| `buf.writeUIntLE()` | 写入无符号小端整数 | 0.5.5 |
| `buf.writeUIntBE()` | 写入无符号大端整数 | 0.5.5 |
| `buf.writeIntLE()` | 写入有符号小端整数 | 0.11.15 |
| `buf.writeIntBE()` | 写入有符号大端整数 | 0.11.15 |
| `buf.writeDoubleBE()` | 写入大端64位双精度浮点数 | 0.11.15 |
| `buf.writeDoubleLE()` | 写入小端64位双精度浮点数 | 0.11.15 |
| `buf.writeFloatBE()` | 写入大端32位浮点数 | 0.11.15 |
| `buf.writeFloatLE()` | 写入小端32位浮点数 | 0.11.15 |
| `buf.swap16()` | 交换16位字节顺序 | 5.10.0 |
| `buf.swap32()` | 交换32位字节顺序 | 5.10.0 |
| `buf.swap64()` | 交换64位字节顺序 | 6.3.0 |
| `buf.keys()` | 创建 Buffer 键的迭代器 | 1.1.0 |
| `buf.values()` | 创建 Buffer 值的迭代器 | 1.1.0 |
| `buf.entries()` | 创建 [index, byte] 迭代器 | 1.1.0 |

#### [1.6  Buffer 静态方法和属性](#)

| 方法 / 属性 | 描述 | 版本引入 |
|------------|------|----------|
| `Buffer.poolSize` | 预分配的内部 Buffer 实例大小（字节），可修改 | 0.11.3 |
| `Buffer.from()` | 从字符串、数组或缓冲区创建新 Buffer | 5.10.0 |
| `Buffer.alloc()` | 创建指定大小的已初始化 Buffer | 5.10.0 |
| `Buffer.allocUnsafe()` | 创建指定大小的未初始化 Buffer（更快但不安全） | 5.10.0 |
| `Buffer.allocUnsafeSlow()` | 创建未初始化 Buffer（不使用共享内存池） | 5.12.0 |
| `Buffer.isBuffer()` | 检查对象是否为 Buffer | 0.1.101 |
| `Buffer.isEncoding()` | 检查编码是否有效 | 0.9.1 |
| `Buffer.concat()` | 合并 Buffer 数组 | 0.7.11 |
| `Buffer.byteLength()` | 返回字符串的字节长度（考虑编码） | 0.1.90 |
| `Buffer.compare()` | 比较两个 Buffer，返回排序顺序 | 0.11.13 |

---

> **说明**：
> - 这些是 **Buffer 类的静态方法/属性**，直接通过 `Buffer` 调用，而不是实例。
> - 自 Node.js 6 起，`Buffer.from()` 和 `Buffer.alloc()` 被推荐为创建 Buffer 的标准方式，取代了已废弃的 `new Buffer()`。
> - `Buffer.byteLength(str, 'utf8')` 与 `Buffer.from(str).length` 效果相同，但更高效（不实际创建 Buffer）。
> - `Buffer.poolSize` 默认为 `8192`（8KB），Node.js 使用内存池优化小 Buffer 的分配。

#### [1.7 Buffer 的实际应用](#)

```javascript
const fs = require('fs');

// 读取文件到 Buffer
fs.readFile('example.txt', (err, data) => {
    if (err) throw err;
    console.log(data.length); // <Buffer ...>
    console.log(data.toString()); // 文件内容
});

// 写入 Buffer 到文件
const content = Buffer.from('Hello File System');

fs.writeFile('output.txt', content, (err) => {
    if (err) throw err;
    console.log('文件已保存');
});
```