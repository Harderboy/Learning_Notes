# HTTP 学习笔记

## http 概念

- http 特性
  - 无状态的协议
    - cookie：客户端相关
    - session：服务器相关
  - http 1.1 版本后支持长链接
  - 单向性协议（必须现有请求后有响应）

## http 请求

- 常见 `GET` 方式
  - form 表单请求方式为 get 或 GET：`<form action="/sunck/showhomepage/" method="GET">`
  - 浏览器搜索
  - 超链接：`a` 标签链接一个地址（`<a href="url">超链接</a>`）
  - js 代码：`window.location.href='url'`，点击跳转

- `POST` 方式
  - form 表单请求方式为 post 或 POST

- `GET` 方式与 `POST` 方式对比
  - 简单分析

    ```text
    GET在浏览器回退时是无害的，而POST会再次提交请求。
    GET产生的URL地址可以被Bookmark，而POST不可以。
    GET请求会被浏览器主动cache，而POST不会，除非手动设置。
    GET请求只能进行url编码，而POST支持多种编码方式。
    GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
    GET请求在URL中传送的参数是有长度限制的（不超过2K），而POST么有。
    对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
    GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。两者都不安全
    GET参数通过URL传递，POST放在Request body中

    Django服务器GET/POST请求为什么接受参数方式都一样
      因为他们都是QueryDict对象（django.http.request）
    ```

  - 深入分析: HTTP 报文的角度
    - 参考资料
      - [GET 和 POST 到底有什么区别？](https://www.zhihu.com/question/28586791)
      - [都 2019 年了，还问 GET 和 POST 的区别](https://blog.fundebug.com/2019/02/22/compare-http-method-get-and-post/)
      - [99%的人都理解错了HTTP中GET与POST的区别](https://mp.weixin.qq.com/s?__biz=MzI3NzIzMzg3Mw==&mid=100000054&idx=1&sn=71f6c214f3833d9ca20b9f7dcd9d33e4#rd)

## http 与 https 对比

**参考链接：**  
[HTTP 协议入门](https://www.ruanyifeng.com/blog/2016/08/http.html)  
[HTTP 的特性](https://hit-alibaba.github.io/interview/basic/network/HTTP.html)  
[http 中 url 的长度限制](https://my.oschina.net/qingqingdego/blog/2994983)
