## [JavaScript 客户端存储](#)
> **介绍**：JavaScript 客户端存储（Client-side Storage）是指在用户浏览器中保存数据的技术，常用于保存用户偏好、会话信息、缓存数据等，以减少服务器请求、提升性能和用户体验。

---
- [1. cookie](#1-cookie)
- [2. Web Storage API](#2-web-storage-api)
- [3. ]

---
### [1. cookie](#)
HTTP Cookie 是服务器发送到 Web 浏览器的一段数据。然后，Web 浏览器将 HTTP Cookie 存储在用户的计算机上，并在以后的请求中将其发送回同一服务器。

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

#### [1.4 JavaScript Cookie 类](#)
以下 Cookie 类用于设置、获取和删除 Cookie

```javascript
class Cookie {
  static get(name) {
    const cookieName = `${encodeURIComponent(name)}=`;
    const cookie = document.cookie;
    let value = null;

    const startIndex = cookie.indexOf(cookieName);
    if (startIndex > -1) {
      const endIndex = cookie.indexOf(';', startIndex);
      if (endIndex == -1) {
        endIndex = cookie.length;
      }
      value = decodeURIComponent(
        cookie.substring(startIndex + name.length, endIndex)
      );
    }
    return value;
  }

  static set(name, value, expires, path, domain, secure) {
    let cookieText = `${encodeURIComponent(name)}=${encodeURIComponent(value)}`;
    if (expires instanceof Date) {
      cookieText += `; expires=${expires.toGMTString()}`;
    }

    if (path) cookieText += `; path=${path}`;
    if (domain) cookieText += `; domain=${domain}`;
    if (secure) cookieText += `; secure`;

    document.cookie = cookieText;
  }

  static remove(name, path, domain, secure) {
    Cookie.set(name, '', new Date(0), path, domain, secure);
  }
}
```
Cookie 类具有三个静态方法：get()、set() 和 remove()。



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

### [3. ]