### [异步 Promise 期约](#)
> **介绍**：在早期的 JavaScript 中，只支持定义回调函数
来表明异步操作完成。串联多个异步操作是一个常见的问题，通常需要深度嵌套的回调函数（俗称“回
调地狱”）来解决。而现在JS直接支持异步操作。

-----
- [1. Promise期约](#1-promise期约)
- [2. 期约的实例方法](#2-期约的实例方法)
- [6. 期约连锁和期约合成](#6-期约连锁和期约合成)

-----
### [1. Promise 期约](#)
ECMAScript 6 增加了对 Promises/A+规范的完善支持，即 Promise 类型。一经推出，Promise 就
大受欢迎，成为了主导性的异步编程机制。所有现代浏览器都支持 ES6 期约，很多其他浏览器 API（如
fetch()和 Battery Status API）也以期约为基础。

`Promise` 一旦定义完成就开始执行。编写一个经典的 Promise 程序 Promise 构造函数参数为一个执行函数，执行函数有两个参数。
* `resolve(any v)` 将期约状态设置为 fulfilled  成功 执行完成。
* `reject(any v)` 将期约状态设置为 rejected  失败 有错误。

```javascript
//新建一个抽奖异步进行
//抽奖成功 将Promise 设置为 fulfilled 并且传递抽到的随机数 否则设置为 rejected

let run = new Promise((resolve,reject) =>{
    let rum = Math.random() *10;
    if (rum > 2.0) {
        console.log("抽奖失败，未获得奖品，感谢惠顾！");
        reject(rum);////执行失败
    }else{
        console.log(`恭喜你，中奖了！获得现金：${ Math.ceil( (rum + 1) * 10)}元`);
        resolve(rum);//执行成功
    }
}).then(v => console.log("抽奖成功，抽到数值: " + v))  /* then 和 catch不可以反过来*/
  .catch(v => console.log("未获奖，损失资金:" + v)); /* 反过来了*/
```

#### [1.1 Promise 是一个状态机](#)
期约是一个有状态的对象，他只能处于以下三种状态之一：
1. 待定 pending。 待定是初始状态 期约只能从待定状态转换为 `对线状态` 或者 `拒绝状态`。然后保持。
2. 对线 fulfilled。 
3. 拒绝 rejected。

```css
                -----> fulfilled
                |
state pending ---
                |
                -----> rejected 
```
期约通过执行函数控制期约的状态。

#### [1.2 Promise 类](#)
通过构造一个 **Promise** 的对象创建一个实例，创建了一个异步操作。一旦创建完成操作就开始执行。

Promise的构造函数 需要传递一个参数。这个参数是一个函数 可以使用箭头函数 这个函数有两个参数。 `resolve` 和 `reject`,这两个参数都是函数
执行 resolve 可以将期约状态从 pending 转换为 `fulfilled` 执行 `reject` 可以将期约状态从 `pending` 转换为 `rejected`。 

这两个函数都可以传递一个参数 `resolve(value)`、`reject(reason)`;

一个Promise 只能有一此状态的改变，一旦改变就不可以再修改。

```javascript
let p = new Promise((resolve, reject) => { });

let p1 = new Promise((resolve, reject) => {
    resolve();
    reject();//没有效果
});

let p2 = new Promise((resolve, reject) => {
    resolve('成功了');
    reject(`失败了`);//没有效果
});
```
**Promise.resolve(value)**:返回一个状态为 fulfilled的期约 等于 `new Promise((resolve, reject)=> resolve(value))`。

**Promise.reject(reason)**:返回一个状态为 rejected的期约 等于 `new Promise((resolve, reject)=> reject(reason))`。
```javascript
let result = Promise.resolve(20).then( v=> { console.log(v)}); //20

let failed = Promise.reject('因为网络问题').catch(reason => {console.log(reason)});//因为网络问题
```

#### [1.3 封装一个json 请求](#)

```javascript
const getJSON = function(url) {
    const promise = new Promise(function(resolve, reject){
        const handler = function() {
            if (this.readyState !== 4) {
                return;
            }
            if (this.status === 200) {
                resolve(this.response);
            } else {
                reject(new Error(this.statusText));
            }
        };
        const client = new XMLHttpRequest();
        client.open("GET", url);
        client.onreadystatechange = handler;
        client.responseType = "json";
        client.setRequestHeader("Accept", "application/json");
        client.send();
    });

    return promise;
};

getJSON("/posts.json").then(function(json) {
    console.log('Contents: ' + json);
}, function(error) {
    console.error('出错了', error);
});
```

#### [1.4 错误处理](#)
Promise的设计很大程度上会导致一种完全不同于JavaScript的设计模式。

```javascript
try {
  throw new Error('We got Error!');
} catch(e) {
  console.log(e); // Error: We got Error!
}

try {
  Promise.reject(new Error('We got Error!'));
} catch(e) {
  console.log(e); // Uncaught (in promise) Error: We got Error!
}
```
1. 在Promise中抛出的错误，不会被外部 `try/catch` 捕获 只能通过catch捕获。
2. 不能在Promise中使用 `try/catch`,里面抛出的错误可以使用catch捕获处理。

这种写法会报错无法执行了。
```javascript
let p1 = new Promise((resolve, reject) => {
    try{
        throw new Error('oh 出错了');
    }catch (e) {
        console.log(e);
    }
});
```
正确写法
```javascript
let p1 = new Promise((resolve, reject) => {
    reject(new Error('oh 出错了'));
}).catch(err => console.log(err.message));//oh 出错了
```
### [2. 期约的实例方法](#)
期约实例的方法是连接外部同步代码与内部异步代码之间的桥梁。这些方法可以访问异步操作返回的数据，处理
期约成功和失败的结果，连续对期约求值，或者添加只有期约进入终止状态时才会执行的代码。

#### [2.1 Thenable接口](#)
期约实例的方法是连接外部同步代码与内部异步代码之间的桥梁。这些方法可以访问异步操作返回的数据，处理期约成功和失败的结果，连续对期约求值，或者添加只有期约进入终止状态时才会执行的代码。

1. 实现Thenable接口:
2. 在 ECMAScript暴露的异步结构中，任何对象都有一个then() 方法。 这个方法就被认为实现了Thenable接口。

下面的例子展示了实现这一接口的最简单的类：
```javascript
class MyThenable{
    /* onResolved、onRejected 是两个函数 */
    then(onResolved, onRejected){
        /*...*/
    }
}
```
#### [2.1 Promise.prototype.then()](#)
Promise实例具有then方法，也就是说，then方法是定义在原型对象Promise.prototype上的。它的作用是为Promise实例添加状态改变时的回调函数。
前面说过，then方法的第一个参数是Resolved状态的回调函数，第二个参数（可选）是Rejected状态的回调函数。

then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即then方法后面再调用另一个then方法。

```javascript
then(onResolved, onRejected);
```  

then返回一个Promise实例。then传入的两个参数是两个函数，这个两个函数只会执行一个。如果Promise状态是fulfilled.那么就执行onResolved函数。 如果状态是rejected那就就执行nRejected函数。`

```javascript
let pVal = new Promise((resolve, reject) => {
    let rum = Math.random() *10;
    if (rum > 5.0) {
        reject(rum);////执行失败
    }else{
        resolve(rum);//执行成功
    }
}).then(value => {
    console.log('i get a number small than (<) 5.0 ');
}, reason => {
    console.log('i get a number big than (>) 5.0 ');
})
```


### [2.2 Promise.prototype.catch](#)
catch只接受一个参数: onRejected处理程序。事实上,这个方法就是一个语法糖 
Promise.prototype.catch方法是.then(null, rejection)的别名，用于指定发生错误时的回调函数。

上面代码中，getJSON方法返回一个Promise对象，如果该对象状态变为Resolved，则会调用then方法指定的回调函数；
如果异步操作抛出错误，状态就会变为Rejected，就会调用catch方法指定的回调函数，处理这个错误。另外，then方法
指定的回调函数，如果运行中抛出错误，也会被catch方法捕获。

```javascript
let promise = new Promise(function(resolve, reject) {
    resolve('ok');
    throw new Error('test');
}).then(function(value) { console.log(value) })
    .catch(function(error) { console.log(error.message) });
```


### [2.3 Promise.prototype.finally](#)
finally:该方法用来制定不管Promise对象最后状态如何，都会执行的操作 该方法是 **ES2018** 引入标准的。

finally方法的回调函数不接受任何参数，这意味着没有办法知道，前面的promise状态到底是fulfilled（成功）还是rejected（失败），这表明，finally方法里面的操作，应该
是与状态无关的，不依赖与promise的执行结果。

```javascript
promise
.then(result => {/* ··· */})
.catch(error => {/* ··· */})
.finally(() => {/* ··· */});

//例子
server.listen(port)
  .then(function () {
    // ...
  })
  .finally(server.stop);
```

### [3. 期约连锁与期约合成](#)
多个期约组合在一起可以构成强大的代码逻辑。这种组合可以通过两种方式实现：期约连锁与期约
合成。前者就是一个期约接一个期约地拼接，后者则是将多个期约组合为一个期约。

#### [3.1 Promise.all()](#)
Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。 方法接受一个数组作为参数 如果参数不是 Promise类型将会转换为 Promise类型。
* Promise.all方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。
* 如果作为参数的 Promise 实例，自己定义了catch方法，那么它一旦被rejected，并不会触发Promise.all()的catch方法。

```javascript
const p = Promise.all([p1, p2, p3]);
//p的状态由p1、p2、p3决定，分成两种情况。
/*
（1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的
返回值组成一个数组，传递给p的回调函数。

（2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的
实例的返回值，会传递给p的回调函数。
*/
```

```javascript
// 生成一个Promise对象的数组
const promises = [2, 3, 5, 7, 11, 13].map(function (id) {
  return getJSON('/post/' + id + ".json");
});

Promise.all(promises).then(function (posts) {
  // ...
}).catch(function(reason){
  // ...
});
```
#### [3.1 要成功就全成功](#)
```javascript
let p1 = new Promise((resolve,reject)=>{
    resolve("p1 执行成功");
});

let p2 = new Promise((resolve,reject)=>{
    resolve("p2 执行成功");
});

let p3 = new Promise((resolve,reject)=>{
    resolve("p3 执行成功");
});

let p = Promise.all([p1,p2,p3]).then( result=>{
    console.log(result);
    //[ 'p1 执行成功', 'p2 执行成功', 'p3 执行成功' ]
}).catch(error =>{
    console.log(error);
})
```
#### [3.2 一个失败全失败](#)

```javascript
 p1 = new Promise((resolve,reject)=>{
    let x = 1;
    let x = 1;
    resolve("p1 执行成功");
});

let p2 = new Promise((resolve,reject)=>{
    resolve("p2 执行成功");
});

let p3 = new Promise((resolve,reject)=>{
    resolve("p3 执行成功");
});

let p = Promise.all([p1,p2,p3]).then( result=>{
    console.log(result);
}).catch(error =>{
    console.log("error:");
    console.log(error);
    /*
    error:
    index.html:28 SyntaxError: Identifier 'x' has already been declared
        at new Promise (<anonymous>)
        at index.html:9

    */
});
```
#### [3.3 Promise.race()](#)
Promise.race方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。
* 跑多个任务，只要一个完成就行。

```javascript
const p = Promise.race([p1, p2, p3]);
/*
上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就
跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。
*/
```

#### [3.4 Promise.try()](#)
database.users.get()可能还会抛出同步错误（比如数据库连接错误，具体要看实现方法），这时你就不得不用try...catch去捕获。

简而言之，Promise.try就像promiseObject.then，但是不需要前面的 promise 对象。现在这仍然有点模糊，所以让我们从一个例子开始。

对于那些未支持Promise.try的 Promise 实现形式，只要它们的 Promise 构造函数可以捕获同步错误，就可以很容易地实现Promise.try功能
。ES6 Promise的构造函数就能够捕获同步错误，所以我们可以这样实现Promise.try：


```javascript
'use strict';
export function promiseTry(func) {
    return new Promise(function(resolve, reject) {
        resolve(func());
    });
}
```
如果如此使用try，会有些麻烦
```javascript
try {
  database.users.get({id: userId})
  .then(...)
  .catch(...)
} catch (e) {
  // ...
}
```
上面这样的写法就很笨拙了，这时就可以统一用promise.catch()捕获所有同步和异步的错误。
```javascript
Promise.try(database.users.get({id: userId}))
  .then(...)
  .catch(...)
```

### [4. 期约扩展](#)
ES6 期约实现是很可靠的，但它也有不足之处。比如，很多第三方期约库实现中具备而 ECMAScript
规范却未涉及的两个特性：**期约取消**和**进度追踪**。


-----
时间: 2022/2/3 12:22