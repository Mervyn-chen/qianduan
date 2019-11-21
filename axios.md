#### axios的理解

**1. Axios是什么**

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。(一看是基于promise是不是就对它的API有了大概的了解？哈哈哈)

**2. Axios的特点**

- 从浏览器中创建 [XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- 从 node.js 创建 [http](http://nodejs.org/api/http.html) 请求
- 支持 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) API
- 拦截请求和响应 （就是有interceptor）
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 [XSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery)

![image-20190904023245891](/Users/caiyifan/Library/Application Support/typora-user-images/image-20190904023245891.png)

https://juejin.im/post/5cb5d9bde51d456e62545abc

https://juejin.im/post/5b55c118f265da0f6f1aa354

