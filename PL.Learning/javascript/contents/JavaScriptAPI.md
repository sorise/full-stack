### [JavaScript API](#)
**介绍**：

---
- [](#)


---

### [1. Atomic与SharedArrayBuffer](#)
**SharedArrayBuffer**（SAB）：可被多个执行体（主线程/Worker）同时访问的内存缓冲区。它不复制数据，而是多方共享同一块内存。

**Atomics**：在共享内存上提供一组原子读写、算术、位运算、交换、以及“futex 风格”等待/唤醒的 API，确保并发下的内存可见性与操作不。    

原理
- JS 的普通操作并不保证对共享内存的原子性（不可分割）与顺序可见性（别的线程何时能看到你的写入）。
- `Atomics.*` 在 SharedArrayBuffer 上的 `Int32Array/BigInt64Array` 视图上执行，提供内存序保障（类似 CPU 原子指令 + 内存栅栏），避免“读旧值”“丢写”“乱序可见”等并发问题。
