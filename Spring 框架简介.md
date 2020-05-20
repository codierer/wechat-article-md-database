# 1 Spring 简介

由于早期`J2EE`复杂的规范，`Spring`于2003年应运而生。`Spring`作为`J2EE`的补充，不包含`J2EE`规定的内容，相反`Spring`精心选择了`J2EE`中的规范并整合到一起，使得开发者能方便快捷规范的实现企业级应用开发。目前`Spring`最新版本为`Version 5.3.0`。`Spring 5.0`开始至少需要`Java EE 7`级别的集成，这样可以确保`Spring`与`Tomcat 8,9`，`WebSphere 9`和`JBoss EAP 7`完全兼容。

`Spring`目前整合的部分[JSR](https://jcp.org/en/home/index "JSR")有：
- Servlet API（[JSR 340](https://www.jcp.org/en/jsr/detail?id=340 "JSR 340")）
- WebSocket API（[JSR 356](https://www.jcp.org/en/jsr/detail?id=356 "JSR 356")）
- 并发程序（[JSR 236](https://www.jcp.org/en/jsr/detail?id=236 "JSR 236")）
- JSON绑定API（[JSR 367](https://www.jcp.org/en/jsr/detail?id=367 "JSR 367")）
- Bean验证（[JSR 303](https://www.jcp.org/en/jsr/detail?id=303 "JSR 303")）
- JPA（[JSR 338](https://www.jcp.org/en/jsr/detail?id=338 "JSR 338")）
- JMS（[JSR 914](https://www.jcp.org/en/jsr/detail?id=914 "JSR 914")）
- 依赖注入（[JSR 330](https://www.jcp.org/en/jsr/detail?id=330 "JSR 330")）
- 通用注释（[JSR 250](https://www.jcp.org/en/jsr/detail?id=250 "JSR 250")）

# 2 Spring模块划分
![Spring模块框架](https://imgkr.cn-bj.ufileos.com/e8f1e47c-72b1-40f4-852e-caa933b155ce.png)
- 核心容器
`spring-core`, `spring-beans`, `spring-context`, `spring-context-support`, `spring-expression`

- AOP和设备支持
`spring-aop`, `spring-aspects`, `spring-instrument` 

- 数据访问与集成
`spring-jdbc`, `spring-orm`, `spring-oxm`, `spring-jms`, `spring-tx`

- Web
`spring-websocket`, `spring-webmvc`, `spring-web`, `portlet`, `spring-webflux`

- Test 
`spring-test`

- 消息(Messaging)
`spring-messaging`

![模块依赖关系](https://imgkr.cn-bj.ufileos.com/f7905ffb-73f0-4e8f-a38f-dd9758bb0c52.png)
