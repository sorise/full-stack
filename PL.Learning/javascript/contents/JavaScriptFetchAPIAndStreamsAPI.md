## [JavaScript FetchAPI、StreamsAPI](#)
**介绍**：Fetch API 是一种用于发起网络请求的现代接口，它提供了比传统的 XMLHttpRequest 更强大和灵活的方式来与远程资源进行交互。Streams API 提供了一种机制来处理流数据，使得我们可以逐步读取或写入数据。

---

- [1. Fetch API](#1-fetch-api)

---
### [1. Fetch API](#)
Fetch API 是一种用于发起网络请求的现代接口，它提供了比传统的 XMLHttpRequest 更强大和灵活的方式来与远程资源进行交互。
。XMLHttpRequest 可以选择异步，而 Fetch API 则必须是异步。

Fetch API 本身是使用 JavaScript 请求资源的优秀工具，同时这个 API 也能够应用在服务线程
（service worker）中，提供拦截、重定向和修改通过 fetch()生成的请求接口。

Fetch 提供了对 Request 和 Response（以及其他与网络请求有关的）对象的通用定义。这将在未来更多需要它们的地方使用它们，无论是 service worker、Cache API，又或者是其他处理请求和响应的方式，甚至是任何一种需要你自己在程序中生成响应的方式（即使用计算机程序或者个人编程指令）。

它同时还为有关联性的概念，例如 CORS 和 HTTP Origin 标头信息，提供一种新的定义，取代它们原来那种分离的定义。

发送请求或者获取资源，请使用 fetch() 方法。它在很多接口中都被实现了，更具体地说，是在 Window 和 WorkerGlobalScope 接口上。因此在几乎所有环境中都可以用这个方法获取资源。

fetch() 强制接受一个参数，即要获取的资源的路径。它返回一个 Promise，该 Promise 会在服务器使用标头响应后，兑现为该请求的 Response——即使服务器的响应是 HTTP 错误状态。你也可以传一个可选的第二个参数 init（参见 Request）。

**Fetch 接口**
- [fetch()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/fetch) 包含了 fetch() 方法，用于获取资源。
- [Headers](https://developer.mozilla.org/zh-CN/docs/Web/API/Headers) 表示响应/请求的标头信息，允许你查询它们，或者针对不同的结果做不同的操作。
- [Request](https://developer.mozilla.org/zh-CN/docs/Web/API/Request) 相当于一个资源请求。
- [Response](https://developer.mozilla.org/zh-CN/docs/Web/API/Response) 相当于请求的响应

```javascript
document.getElementById('login-form').addEventListener('submit', async function (e) {
    e.preventDefault();

    const form = e.target;
    const username = form.username.value.trim();
    const password = form.password.value.trim();

    // 设置请求头
    const headers = new Headers({
        'Content-Type': 'application/json',
        'Authorization': 'Bearer your_token_here'  // 模拟 Token
    });

    // 构建请求体
    const body = JSON.stringify({ username, password });

    // 创建 Request 对象
    const request = new Request('https://jsonplaceholder.typicode.com/posts', {
        method: 'POST',
        headers: headers,
        body: body,
        mode: 'cors',       // 跨域模式
        cache: 'default'    // 缓存策略
    });

    try {
    // 发送请求
    const response = await fetch(request);

    const resultDiv = document.getElementById('result');

    // 检查响应状态码
    if (!response.ok) {
        throw new Error(`HTTP 错误：${response.status}`);
    }

    // 获取响应头
    console.log('响应头:', [...response.headers]);

    // 解析响应数据
    const data = await response.json();

    resultDiv.innerHTML = `
          <h2>登录成功！收到响应：</h2>
          <pre>${JSON.stringify(data, null, 2)}</pre>
        `;
    } catch (error) {
        console.error('请求失败:', error);
        document.getElementById('result').innerHTML = `<p style="color:red;">错误：${error.message}</p>`;
    }
});

```

#### [1.1 基本用法](#)
fetch()方法是暴露在全局作用域中的，包括主页面执行线程、模块和工作线程。调用这个方法，
浏览器就会向给定 URL 发送请求。