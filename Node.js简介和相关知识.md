# Node.js简介
`Node.js`是一个开源和跨平台的`JavaScript`运行时环境。它基于`V8 JavaScript`引擎运行（`Google Chrome`的核心），实现在浏览器外部运行。运行在单进程中，其标准库提供异步`I/O`原语。为防止`JavaScript`执行过程出现阻塞，`Node.js`中的库是使用非阻塞范式编写的，从而使阻塞行为成为例外而不是规范。`Node.js`执行`I/O`操作不是阻塞线程并浪费CPU等待时间，而是在响应返回时恢复操作。使得`Node.js`可以处理数千个并发连接，不会增加管理线程的负担。在`Node.js`中，可以毫无问题地使用新的`ECMAScript`标准，因为您可以通过更改`Node.js`版本来决定使用哪个`ECMAScript`版本，还可以通过运行带有特定标志的`Node.js`来启用特定的实验功能。`Node.js` 使用[NPM](https://www.npmjs.com/ "NPM")部署插件，使开发效率大大提升。

# 为什么用Node.js
- 开发效率高，有能力构建复杂系统；
- 性能和I/O负载，Node.js很好的解决I/O密集的问题，通过异步实现；
- 连续的内存开销，支持超过12万活跃连接，每个连接消耗大约2k的内存；
- 操作性，实现对内存堆栈的监控系统；
- Node.js社区在壮大，交流学习资源不断增多，开源项目也是非常多的。

# Node.js框架和工具

## Node.js搭建

[install](https://nodejs.dev/learn/how-to-install-nodejs, "install")

## web框架

[Adnoisjs](https://adonisjs.com/docs/4.1/installation "Adnoisjs"), [Express](https://expressjs.com/en/4x/api.html "Express")，[Fastify](https://www.fastify.io/ "Fastify")，[koa](https://koajs.com/ "koa")

## Javascript引擎

[V8](https://v8.dev/docs "V8") 支持`google chrome`浏览器的`Javascript`，提供`Javascript`运行环境，其他浏览器引擎分别为`Firefox` [SpiderMonkey](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/SpiderMonkey "SpiderMonkey"),`Safari`[JavaScriptCore](https://developer.apple.com/documentation/javascriptcore "JavaScriptCore") （`Nitro`），`Edge` 的 [Chakra](https://github.com/Microsoft/ChakraCore "Chakra").
