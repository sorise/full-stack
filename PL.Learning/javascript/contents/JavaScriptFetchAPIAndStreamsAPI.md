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

#### [1.1 基本用法](#)
fetch()方法是暴露在全局作用域中的，包括主页面执行线程、模块和工作线程。调用这个方法，
浏览器就会向给定 URL 发送请求。