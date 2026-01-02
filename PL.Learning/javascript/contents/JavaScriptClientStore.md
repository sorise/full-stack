## [JavaScript 客户端存储](#)
> **介绍**：JavaScript 客户端存储（Client-side Storage）是指在用户浏览器中保存数据的技术，常用于保存用户偏好、会话信息、缓存数据等，以减少服务器请求、提升性能和用户体验。

---
- [1. cookie](#1-cookie)
- [2. Web Storage API](#2-web-storage-api)
- [3. IndexedDB](#3-indexeddb)
- [4. IndexedDB version 参数详解](#4-indexeddb-version-参数详解)

---
### [1. cookie](#)
HTTP Cookie 是服务器发送到 Web 浏览器的一段数据。然后，Web 浏览器将 HTTP Cookie 存储在用户的计算机上，并在以后的请求中将其发送回同一服务器。

服务器响应HTTP
```
HTTP/1.1 200 OK
Content-type:text/html
Date: Thu, 01 Jan 2026 11:07:55 GMT
Referrer-policy: origin-when-cross-origin, strict-origin-when-cross-origin
Server: github.com
Set-Cookie: name=value path=/; secure; HttpOnly; SameSite=Lax
```
Web 浏览器将存储此信息，并在下次请求时通过 Cookie HTTP 标头将其发送回服务器，如下所示
```
GET /index.html HTTP/1.1
Cookie: name=value path=/; secure; HttpOnly; SameSite=Lax
```

最常见的用处之一就是身份验证：

- 登录后，服务器在响应中使用 Set-Cookie HTTP-header 来设置具有唯一“会话标识符（session identifier）”的 cookie。
- 下次当请求被发送到同一个域时，浏览器会使用 Cookie HTTP-header 通过网络发送 cookie。
- 所以服务器知道是谁发起了请求。

在[JavaScript中处理cookie](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Guides/Cookies#%E5%AE%89%E5%85%A8)比较麻烦，因为接口过于简单，**只有Dom的document.cookie属性**。
document.cookie 的值由 `name=value` 对组成，以 `;` 分隔。每一个都是独立的 cookie。

从技术上讲，cookie 的名称和值可以是任何字符。所有的名何值都是URL编码的，因此，为了保持有效的格式，它们应该使用内建的 encodeURIComponent 函数对其进行转义：
```javascript
// 特殊字符（空格），需要编码
let name = "my name";
let value = "John Smith"

// 将 cookie 编码为 my%20name=John%20Smith
document.cookie = encodeURIComponent(name) + '=' + encodeURIComponent(value);
```

#### [1.1 cookie的限制](#)
cookie 存在许多限制条件，来阻止 cookie 滥用并保护浏览器和服务器免受一些负面影响。有两种 cookie 限制条件：cookie 的属性和 cookie 的总大小。

- 每个cookie的大小不能超过 4KB。因此，我们不能在一个cookie中保存大的东西。
- 每个域的 cookie 总数不得超过 20+ 左右，具体限制取决于浏览器。

#### [1.2 cookie的构成](#)
Cookie 包含以下由 Web 浏览器存储的信息：
- **名称** 一个唯一的名称，用于识别 Cookie。Cookie 名称不区分大小写。这意味着 Username 和 username 是同一个 Cookie。
- **值** - Cookie 的字符串值。它必须是 URL 编码的。
- **域** - Cookie 有效的域。
- **路径** - Cookie 应该发送到服务器的域的路径（不含域）。例如，您可以指定 Cookie 只能从 https://tutorial.javascript.ac.cn/dom/ 访问，因此 https://www.javascripttutoiral.net/ 上的页面不会发送 Cookie 信息。
- **过期时间** - 指示 Web 浏览器何时应删除 Cookie（或何时浏览器应停止将 Cookie 发送到服务器）的时间戳。过期时间以`GMT` 格式的日期设置：`Wdy, DD-Mon-YYYY HH:MM:SS GMT`。过期时间允许 Cookie 即使在用户关闭 Web 浏览器后也存储在用户的 Web 浏览器中。
- **安全标志** - 如果指定，浏览器仅在使用SSL安全连接的情况下才会（https，而不是 http）将 Cookie 发送到服务器。

##### 1.2.1 domain
Domain 指定了哪些主机可以接受 Cookie。如果不指定，该属性默认为同一 host 设置 cookie，不包含子域名。如果指定了 Domain，则一般包含子域名。因此，指定 Domain 比省略它的限制要少。但是，当子域需要共享有关用户的信息时，这可能会有所帮助。

例如，如果设置 Domain=mozilla.org，则 Cookie 也包含在子域名中（如 developer.mozilla.org）。

```javascript
// 在 site.com
// 使 cookie 可以被在任何子域 *.site.com 访问：
document.cookie = "user=John; domain=site.com"

// 之后
// 在 forum.site.com
alert(document.cookie); // 有 cookie user=John
```

##### 1.2.2 Path 属性
Path 属性指定了一个 URL 路径，该 URL 路径必须存在于请求的 URL 中，以便发送 Cookie 标头。以字符 `%x2F` `(“/”)` 作为路径分隔符，并且子路径也会被匹配。

url 路径前缀必须是绝对路径。它使得该路径下的页面可以访问该 cookie。默认为当前路径。

如果一个 cookie 带有 `path=/admin` 设置，那么该 cookie 在 `/admin` 和 `/admin/something` 下都是可见的，但是在 `/home` 或 `/adminpage` 下不可见。

通常，我们应该将 path 设置为根目录：`path=/`，以使 cookie 对此网站的所有页面可见。

例如，设置 `Path=/docs`，则以下地址都会匹配：
```
/docs
/docs/
/docs/Web/
/docs/Web/HTTP
```
但是这些请求路径不会匹配以下地址：
```
/
/docsets
/fr/docs
```

##### 1.2.3 定义 Cookie 的生命周期
Cookie 的生命周期可以通过两种方式定义：
- **会话期 Cookie** 会在当前的会话结束之后删除。浏览器定义了“当前会话”结束的时间，一些浏览器重启时会使用会话恢复。这可能导致会话 cookie 无限延长。
- **持久性 Cookie** 在过期时间（Expires）指定的日期或有效期（Max-Age）指定的一段时间后被删除。
```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```
为了让 cookie 在浏览器关闭后仍然存在，我们可以设置 expires 或 max-age 选项中的一个。

- expires=Tue, 19 Jan 2038 03:14:07 GMT

cookie 的过期时间定义了浏览器会自动清除该 cookie 的时间。

日期必须完全采用 GMT 时区的这种格式。我们可以使用 date.toUTCString 来获取它。例如，我们可以将 cookie 设置为 1 天后过期。
```
// 当前时间 +1 天
let date = new Date(Date.now() + 86400e3);
date = date.toUTCString();
document.cookie = "user=John; expires=" + date;
```
**如果我们将 expires 设置为过去的时间，则 cookie 会被删除**。
- max-age=3600

它是 expires 的替代选项，指明了 cookie 的过期时间距离当前时间的秒数。

如果将其设置为 0 或负数，则 cookie 会被删除：
```javascript
// cookie 会在一小时后失效
document.cookie = "user=John; max-age=3600";

// 删除 cookie（让它立即过期）
document.cookie = "user=John; max-age=0";
```

##### 1.2.4 限制访问 Cookie
有两种方法可以确保 Cookie 被安全发送，并且不会被意外的参与者或脚本访问：Secure 属性和 HttpOnly 属性。

标记为 Secure 的 Cookie 只应通过被 HTTPS 协议加密过的请求发送给服务端。它永远不会使用不安全的 HTTP 发送（本地主机除外），这意味着中间人攻击者无法轻松访问它。不安全的站点（在 URL 中带有 http:）无法使用 Secure 属性设置 cookie。但是，Secure 不会阻止对 cookie 中敏感信息的访问。例如，有权访问客户端硬盘（或，如果未设置 HttpOnly 属性，则为 JavaScript）的人可以读取和修改它。
```javascript
// 假设我们现在在 HTTPS 环境下
// 设置 cookie secure（只在 HTTPS 环境下可访问）
document.cookie = "user=John; secure";
```

JavaScript Document.cookie API 无法访问带有 HttpOnly 属性的 cookie；此类 Cookie 仅作用于服务器。例如，持久化服务器端会话的 Cookie 不需要对 JavaScript 可用，而应具有 HttpOnly 属性。此预防措施有助于缓解跨站点脚本（XSS）攻击。

#### [1.3 httpOnly](#)
这个选项和 JavaScript 没有关系，但是我们必须为了完整性也提一下它。

Web 服务器使用 `Set-Cookie` header 来设置 cookie。并且，它可以设置 httpOnly 选项。

这个选项禁止任何 JavaScript 访问 cookie。我们使用 document.cookie 看不到此类 cookie，也无法对此类 cookie 进行操作。

这是一种预防措施，当黑客将自己的 JavaScript 代码注入网页，并等待用户访问该页面时发起攻击，而这个选项可以防止此时的这种攻击。这应该是不可能发生的，黑客应该无法将他们的代码注入我们的网站，但是网站有可能存在 bug，使得黑客能够实现这样的操作。

通常来说，如果发生了这种情况，并且用户访问了带有黑客 JavaScript 代码的页面，黑客代码将执行并通过 document.cookie 获取到包含用户身份验证信息的 cookie。这就很糟糕了。

但是，如果 cookie 设置了 httpOnly，那么 `document.cookie` 则看不到 `cookie`，所以它受到了保护。

#### [1.4 JavaScript Cookie 工具函数](#)
以下 Cookie 类用于设置、获取和删除 Cookie

获取 cookie 最简短的方式是使用 正则表达式。getCookie(name) 函数返回具有给定 name 的 cookie：
```javascript
// 返回具有给定 name 的 cookie，
// 如果没找到，则返回 undefined
function getCookie(name) {
  let matches = document.cookie.match(new RegExp(
    "(?:^|; )" + name.replace(/([\.$?*|{}\(\)\[\]\\\/\+^])/g, '\\$1') + "=([^;]*)"
  ));
  return matches ? decodeURIComponent(matches[1]) : undefined;
}
```
这里的 new RegExp 是动态生成的，以匹配 `; name=<value>`。
> 请注意 cookie 的值是经过编码的，所以 getCookie 使用了内建方法 decodeURIComponent 函数对其进行解码。

setCookie(name, value, options) 将 cookie 的 name 设置为具有默认值 `path=/`（可以修改以添加其他默认值）和给定值 value：

```javascript
function setCookie(name, value, options = {}) {

  options = {
    path: '/',
    // 如果需要，可以在这里添加其他默认值
    ...options
  };

  if (options.expires instanceof Date) {
    options.expires = options.expires.toUTCString();
  }

  let updatedCookie = encodeURIComponent(name) + "=" + encodeURIComponent(value);

  for (let optionKey in options) {
    updatedCookie += "; " + optionKey;
    let optionValue = options[optionKey];
    if (optionValue !== true) {
      updatedCookie += "=" + optionValue;
    }
  }

  document.cookie = updatedCookie;
}

// 使用范例：
setCookie('user', 'John', {secure: true, 'max-age': 3600});
```
deleteCookie(name); 要删除一个 cookie，我们可以给它设置一个负的过期时间来调用它：

```javascript
function deleteCookie(name) {
  setCookie(name, "", {
    'max-age': -1
  })
}
```




### [2. Web Storage API](#)
[Web Storage API](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API) 提供了浏览器可以存储键/值对的机制，其方式比使用 cookie 更直观。

Web Storage 包含如下两种机制：
- sessionStorage 为每一个给定的源（origin）维持一个独立的存储区域，该存储区域在页面会话期间可用（即只要浏览器处于打开状态，包括页面重新加载和恢复）。
    - 仅为会话存储数据，这意味着数据将一直存储到浏览器（或选项卡）关闭。
    - 数据永远不会被传输到服务器。
    - 存储限额大于 cookie（最大 5MB）。
- localStorage 做同样的事情，但即使浏览器关闭并重新打开也仍然存在。
    - 存储的数据没有过期日期，只能通过 JavaScript、清除浏览器缓存或本地存储的数据来清除。
    - 存储限额是两者之间的最大值。

我们已经有了 cookie。为什么还要其他存储对象呢？

- 与 cookie 不同，Web 存储对象不会随每个请求被发送到服务器。因此，我们可以保存更多数据。大多数现代浏览器都允许保存至少 5MB 的数据（或更多），并且具有用于配置数据的设置。
- 还有一点和 cookie 不同，服务器无法通过 HTTP header 操纵存储对象。一切都是在 JavaScript 中完成的。
- 存储绑定到源（域/协议/端口三者）。也就是说，不同协议或子域对应不同的存储对象，它们之间无法访问彼此数据。

#### [2.1 Storage 类型](#)
[Storage](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage) 作为Web Storage API的接口,提供了访问特定域名下的会话存储或本地存储的功能，例如，可以添加、修改或删除存储的数据项。

如果你想要操作一个域名的会话存储，可以使用 Window.sessionStorage；如果想要操作一个域名的本地存储，可以使用 Window.localStorage。

**属性**:
- Storage.length `只读` 返回一个整数，表示存储在 Storage 对象中的数据项数量。

**方法**:
- Storage.key(index) 该方法接受一个数值 n 作为参数，并返回存储中的第 n 个键名。
- Storage.getItem(keyName) 该方法接受一个键名作为参数，返回键名对应的值，如果该键名不存在，则返回 null。
- Storage.setItem(keyName, keyValue) 该方法接受一个键名和值作为参数，将会把键值对添加到存储中，如果键名存在，则更新其对应的值。
- Storage.removeItem() 该方法接受一个键名作为参数，并把该键名从存储中删除。
- Storage.clear(keyName) 调用该方法会清空存储中的所有键名。

#### [2.2 storage 事件](#)
当 localStorage 或 sessionStorage 中的数据更新后，storage 事件就会触发，它具有以下属性：

- key —— 发生更改的数据的 key（如果调用的是 .clear() 方法，则为 null）。
- oldValue —— 旧值（如果是新增数据，则为 null）。
- newValue —— 新值（如果是删除数据，则为 null）。
- url —— 发生数据更新的文档的 url。
- storageArea —— 发生数据更新的 localStorage 或 sessionStorage 对象。


```javascript
// 在其他文档对同一存储进行更新时触发
// 也可以使用 window.addEventListener('storage', event => {
window.onstorage = event => { 
  if (event.key != 'now') return;
  alert(event.key + ':' + event.newValue + " at " + event.url);
};

localStorage.setItem('now', Date.now());
```

#### [2.3 localStorage](#)
localStorage是Storage的实例，localStorage 中的键值对总是以字符串的形式存储。
大多数现代浏览器都允许保存至少 5MB 的数据（或更多），并且具有用于配置数据的设置。

**存储特性**:
- 在同源的所有标签页和窗口之间共享数据。
- 数据不会过期。它在浏览器重启甚至系统重启后仍然存在。

```javascript
//使用方法存储数据
localStorage.setItem('umi', 785);
//使用熟悉存储数据
localStorage.name = "jxkicker"
//使用熟悉设置key
localStorage.age = 24
//使用方法设置
let _name = localStorage.getItem('name');
```
**遍历键**:
```javascript
for(let i = 0; i < localStorage.length; i++) {
  let key = localStorage.key(i);
  alert(`${key}: ${localStorage.getItem(key)}`);
}
```

#### [2.4 sessionStorage](#)
sessionStorage 对象的使用频率比 localStorage 对象低得多,属性和方法是相同的，但是它有更多的限制：

- sessionStorage 的数据只存在于当前浏览器标签页。
  - 具有相同页面的另一个标签页中将会有不同的存储。
  - 但是，它在同一标签页下的 iframe 之间是共享的（假如它们来自相同的源）。
- 数据在页面刷新后仍然保留，但在关闭/重新打开浏览器标签页后不会被保留。


这是因为 sessionStorage 不仅绑定到源，**还绑定在同一浏览器标签页**。因此，sessionStorage 很少被使用。
```javascript
sessionStorage.setItem('test', 1);
```

### [3. IndexedDB](#)
IndexedDB 是一种底层 API，是浏览器中**存储结构化数据**的一个方案，其设计完全是**异步**的，绝大多数操作
都要求添加onerror何onsuccess事件处理程序来确定输出，它使用**对象存储**而不是二维表存储数据，**类似NoSQL数据库**。

IndexedDB 是一个**事务型数据库系统**，类似于基于 SQL 的 RDBMS。然而，不像 RDBMS 使用固定列表，IndexedDB 是一个基于 JavaScript 的面向对象数据库。IndexedDB 允许你存储和检索用键索引的对象。
- 使用 IndexedDB 执行的操作是异步执行的，以免阻塞应用程序。
- 通过支持多种类型的键，来存储几乎可以是任何类型的值。
- 支撑事务的可靠性。
- 支持键值范围查询、索引。

**IndexedDB 鼓励使用的基本模式如下所示**：

- 打开数据库。
- 在数据库中创建一个对象存储（object store）。
- 启动事务，并发送一个请求来执行一些数据库操作，如添加或获取数据等。
- 通过监听正确类型的 DOM 事件以等待操作完成。
- 对结果进行一些操作（可以在 request 对象中找到）

**数据库层级结构**
```
Database (数据库)
├── Object Store (对象存储空间)
│   ├── Index (索引)
│   └── Data Records (数据记录)
└── Object Store (对象存储空间)
```



#### [3.1 打开数据库](#)
要想使用 IndexedDB，首先需要 open（连接）一个数据库。
```javascript
let openRequest = indexedDB.open(name, version);
```
- name —— 字符串，即数据库名称。
- version —— 一个正整数版本，默认为 1（下面解释）。

open 方法的二个参数是数据库的版本号。数据库的版本决定了数据库模式（schema），即数据库的对象存储（object store）以及存储结构。
- 如果数据库不存在，open 操作会创建该数据库，然后触发 onupgradeneeded 事件，你需要在该事件的处理器中创建数据库模式。
- 如果数据库已经存在，但你指定了一个更高的数据库版本，会直接触发 onupgradeneeded 事件，允许你在处理器中更新数据库模式。

调用之后会返回 openRequest 对象，我们需要监听该对象上的事件：
- success：数据库准备就绪，openRequest.result 中有了一个数据库对象“ Database Object”，我们应该将其用于进一步的调用。
- error：打开失败。
- upgradeneeded：数据库已准备就绪，但其版本已过时（见下文）。

```javascript
let openRequest = indexedDB.open("store", 1);
let db_str = null;
openRequest.onupgradeneeded = function(event) {
    // 如果客户端没有数据库则触发
    db_str = event.target.result;
    console.log(db_str);
    // ...执行初始化...
     // 为数据库创建对象存储（objectStore）
    const objectStore = db_str.createObjectStore("name", { keyPath: "myKey" });
};

openRequest.onerror = function(event) {
    console.error("Error", openRequest.error);
};

openRequest.onsuccess = function(event) {
    db_str = openRequest.result;
    // 继续使用 db 对象处理数据库
};
```
IndexedDB 具有内建的“模式（scheme）版本控制”机制，这在服务器端数据库中是不存在的。

我们可以打开版本 2 中的 IndexedDB 数据库，并像这样进行升级：
```javascript
let openRequest = indexedDB.open("store", 2);

openRequest.onupgradeneeded = function(event) {
  // 现有的数据库版本小于 2（或不存在）
  let db = openRequest.result;
  switch(event.oldVersion) { // 现有的 db 版本
    case 0:
      // 版本 0 表示客户端没有数据库
      // 执行初始化
    case 1:
      // 客户端版本为 1
      // 更新
  }
};
```
接下来，当且仅当 onupgradeneeded 处理程序没有错误地执行完成，openRequest.onsuccess 被触发，数据库才算是成功打开了。

**删除数据库**：
```javascript
let deleteRequest = indexedDB.deleteDatabase(name)
// deleteRequest.onsuccess/onerror 追踪（tracks）结果
```

#### [3.2 并行更新问题](#)
提到版本控制，有一个相关的小问题。

举个例子：
- 一个用户在一个浏览器标签页中打开了数据库版本为 1 的我们的网站。
- 接下来我们发布了一个更新，使得代码更新了。
- 接下来同一个用户在另一个浏览器标签中打开了这个网站。  

这时，有一个标签页和版本为 1 的数据库建立了一个连接，而另一个标签页试图在其 upgradeneeded 处理程序中将数据库版本升级到 2。

问题是，这两个网页是同一个站点，同一个源，共享同一个数据库。而数据库不能同时为版本 1 和版本 2。要执行版本 2 的更新，必须关闭对版本 1 的所有连接，包括第一个标签页中的那个。

为了解决这一问题，versionchange 事件会在“过时的”数据库对象上触发。我们需要监听这个事件，关闭对旧版本数据库的连接（还应该建议访问者重新加载页面，以加载最新的代码）。

如果我们不监听 versionchange 事件，也不去关闭旧连接，那么新的连接就不会建立。openRequest 对象会产生 blocked 事件，而不是 success 事件。因此第二个标签页无法正常工作。

下面是能够正确处理并行升级情况的代码。它安装了 onversionchange 处理程序，如果当前数据库连接过时（数据库版本在其他位置被更新）并关闭连接，则会触发该处理程序。

```javascript
let openRequest = indexedDB.open("store", 2);

openRequest.onupgradeneeded = ...;
openRequest.onerror = ...;

openRequest.onsuccess = function() {
  let db = openRequest.result;

  db.onversionchange = function() {
    db.close();
    alert("Database is outdated, please reload the page.")
  };

  // ……数据库已经准备好，请使用它……
};

openRequest.onblocked = function() {
  // 如果我们正确处理了 onversionchange 事件，这个事件就不应该触发

  // 这意味着还有另一个指向同一数据库的连接
  // 并且在 db.onversionchange 被触发后，该连接没有被关闭
};
```
……换句话说，在这我们做两件事：

- 如果当前数据库版本过时，db.onversionchange 监听器会通知我们并行尝试更新。
- openRequest.onblocked 监听器通知我们相反的情况：在其他地方有一个与过时的版本的连接未关闭，因此无法建立新的连接。

我们可以在 db.onversionchange 中更优雅地进行处理，提示访问者在连接关闭之前保存数据等。

或者，另一种方式是不在 db.onversionchange 中关闭数据库，而是使用 onblocked 处理程序（在浏览器新 tab 页中）来提醒用户，告诉他新版本无法加载，直到他们关闭浏览器其他 tab 页。

这种更新冲突很少发生，但我们至少应该有一些对其进行处理的程序，至少在 onblocked 处理程序中进行处理，以防程序默默卡死而影响用户体验。

#### [3.3 对象库（object store）](#)
要在 IndexedDB 中存储某些内容，我们需要一个**对象库**,几乎可以存储任何值，包括复杂的对象。IndexedDB 使用 标准序列化算法 来克隆和存储对象。类似于 JSON.stringify，不过功能更加强大，能够存储更多的数据类型。

**库中的每个值都必须有唯一的键 key**。

键的类型必须为数字、日期、字符串、二进制或数组。它是唯一的标识符，所以我们可以通过键来搜索/删除/更新值。

但我们需要先创建一个对象库。
创建对象库的语法：
```javascript
db.createObjectStore(name[, keyOptions]);
```
请注意，操作是同步的，不需要 await。

- name 是存储区名称，例如 "books" 表示书。
- keyOptions 是具有以下两个属性之一的可选对象：
  - keyPath —— 对象属性的路径，IndexedDB 将以此路径作为键，例如 id。
  - autoIncrement —— 如果为 true，则自动生成新存储的对象的键，键是一个不断递增的数字。

如果我们不提供 keyOptions，那么以后需要在存储对象时，显式地提供一个键。

例如，此对象库使用 id 属性作为键：
```javascript
db.createObjectStore('books', {keyPath: 'id'});

// 创建/升级 数据库而无需版本检查
openRequest.onupgradeneeded = function() {
  let db = openRequest.result;
  if (!db.objectStoreNames.contains('books')) { // 如果没有 “books” 数据
    db.createObjectStore('books', {keyPath: 'id'}); // 创造它
  }
};
```
**删除对象库**:
```javascript
db.deleteObjectStore('books')
```

#### [3.4 事务（transaction）](#)
术语“事务（transaction）”是通用的，许多数据库中都有用到。事务是一组操作，要么全部成功，要么全部失败。
**所有数据操作都必须在 IndexedDB 中的事务内进行**。
```javascript
db.transaction(store[, type]);
```
- store 是事务要访问的库名称，例如 "books"。如果我们要访问多个库，则是库名称的数组。
- type – 事务类型，以下类型之一：
  - readonly —— 只读，默认值。
  - readwrite —— 只能读取和写入数据，而不能 创建/删除/更改 对象库。

如果不指定参数，则对数据库中所有的对象存储有只读权限
```javascript
let transaction = db.transaction();
```
指定一个对象存储。事务期间只加载对象存储的信息。
```javascript
let transaction = db.transaction("user");
```
指定多个对象存储，表示，要访问多个对象存储
```javascript
let transaction = db.transaction(["user", "books"], "readwrite");
```
例子：
```javascript
let openRequest = indexedDB.open("store", 3);
let db = null;
openRequest.onupgradeneeded = function(event) {
    // 如果客户端没有数据库则触发
    db = event.target.result;
    if (!db.objectStoreNames.contains('books')) { // 如果没有 “books” 数据
        db.createObjectStore('books', {keyPath: 'id'}); // 创造它
    }
};

openRequest.onerror = function(event) {
    console.error("Error", openRequest.error);
};

openRequest.onsuccess = function(event) {
    db = openRequest.result;
    // 继续使用 db 对象处理数据库

    let transaction = db.transaction("books", "readwrite"); // (1)

    // 获取对象库进行操作
    let books = transaction.objectStore("books"); // (2)

    let book = {
        id: '123123',
        price: 10,
        created: new Date()
    };

    let request = books.add(book); // (3)

    request.onsuccess = function() { // (4)
        console.log("Book added to the store", request.result);
    };

    request.onerror = function() {
        console.log("Error", request.error);
    }
};
```
从数据库中删除数据
```javascript
const request = db
  .transaction(["customers"], "readwrite")
  .objectStore("customers")
  .delete("444-44-4444");
request.onsuccess = (event) => {
  // 删除成功！
};
```
从数据库中获取数据
```javascript
const transaction = db.transaction(["customers"]);
const objectStore = transaction.objectStore("customers");
const request = objectStore.get("444-44-4444");

request.onerror = (event) => {
  // 错误处理！
};
request.onsuccess = (event) => {
  // 对 request.result 做些操作！
  console.log(`SSN 444-44-4444 对应的名字是 ${request.result.name}`);
};
```
更新数据库中的记录
```javascript
const objectStore = db
  .transaction(["customers"], "readwrite")
  .objectStore("customers");
const request = objectStore.get("444-44-4444");
request.onerror = (event) => {
  // 错误处理！
};
request.onsuccess = (event) => {
  // 获取我们想要更新的旧值
  const data = event.target.result;

  // 更新对象中你想修改的值
  data.age = 42;

  // 把更新过的对象放回数据库。
  const requestUpdate = objectStore.put(data);
  requestUpdate.onerror = (event) => {
    // 对错误进行处理
  };
  requestUpdate.onsuccess = (event) => {
    // 成功，数据已更新！
  };
};
```

#### [3.5 使用游标](#)
使用 get() 要求你知道你想要检索哪一个键。如果你想要遍历对象存储空间中的所有值，那么你可以使用游标。看起来会像下面这样：
```javascript
const objectStore = db.transaction("customers").objectStore("customers");

objectStore.openCursor().onsuccess = (event) => {
  const cursor = event.target.result;
  if (cursor) {
    console.log(`SSN ${cursor.key} 对应的名字是 ${cursor.value.name}`);
    cursor.continue();
  } else {
    console.log("没有更多记录了！");
  }
};
```


### [4. IndexedDB version 参数详解](#)
在 JavaScript 的 IndexedDB API 中，`indexedDB.open(name, version)` 方法用于打开（或创建）一个数据库。其中的 `version` 参数具有非常重要的语义，它**表示你希望打开数据库时使用的版本号**。

#### 4.1 version 的作用

1. **控制数据库结构的演进**  
  IndexedDB 是一个结构化存储系统，它通过“对象仓库”（Object Store）来组织数据。当你需要：
  - 创建新的对象仓库
  - 删除或修改已有对象仓库
  - 添加索引等结构变更  
  这些操作**只能在 `upgradeneeded` 事件中进行**，而该事件**仅在数据库版本升级时触发**。

2. **版本必须是大于 0 的整数**  
  - 第一次创建数据库时，通常传入 `1`。
  - 后续如果要修改数据库结构（比如加新表），就需要传入比当前版本更大的整数（如 `2`, `3`...）。
  - 如果省略 `version` 参数，则默认使用数据库的当前版本（不会触发结构变更）。


#### 4.2 工作流程示例

```javascript
let request = indexedDB.open('MyDB', 2); // 尝试以版本 2 打开数据库

request.onupgradeneeded = function(event) {
  let db = event.target.result;
  let oldVersion = event.oldVersion; // 升级前的版本（可能是 0、1 等）
  let newVersion = event.newVersion; // 即将升级到的版本（这里是 2）

  if (oldVersion < 1) {
    // 第一次创建数据库：创建 users 表
    db.createObjectStore('users', { keyPath: 'id' });
  }
  if (oldVersion < 2) {
    // 从版本 1 升级到 2：添加 orders 表
    db.createObjectStore('orders', { keyPath: 'orderId' });
  }
};

request.onsuccess = function(event) {
  let db = event.target.result;
  console.log('数据库打开成功，当前版本：', db.version);
};
```

#### 4.3 关键点总结

| 情况 | 行为 |
|------|------|
| `version` **大于** 当前数据库版本 | 触发 `upgradeneeded` 事件，允许修改结构 |
| `version` **等于** 当前数据库版本 | 直接打开数据库，不触发 `upgradeneeded` |
| `version` **小于** 当前数据库版本 | 打开失败，触发 `error` 事件（不允许降级） |
| 不传 `version` | 使用当前版本打开，不升级也不降级 |

> 注意：**不能降级数据库版本**。IndexedDB 不支持回滚结构变更。

#### 4.4 实际开发建议
- 始终显式指定 `version`，便于管理数据库演进。
- 在 `upgradeneeded` 中根据 `oldVersion` 做**增量升级**（而不是每次都重建整个结构）。
- 版本号应为**递增的正整数**（1, 2, 3...），不要跳太多（虽然技术上允许）。


#### 参考链接
- [使用 HTTP Cookie - MDN Web Docs](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Guides/Cookies)
- [JavaScript 核心指南 Cookie](https://tutorial.javascript.ac.cn/web-apis/javascript-cookies/)
- [Cookie，document.cookie](https://zh.javascript.info/cookie)
- [IndexedDB](https://zh.javascript.info/indexeddb)
- [IndexedDB 使用指南](https://juejin.cn/post/7588109656042061870?searchId=2026010216113527C4124354B22CB2D272)