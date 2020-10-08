"serverless"不是没有server, 开发者只关心应用功能的实现, 而不是怎么搭建础设施

# How Serverless Works

 Functions as a Service (FaaS) 

1. **The developer writes a function.** This function often serves a specific purpose within the application code.
2. **The developer defines an event.** The event is what triggers the cloud service provider to execute the function. A common type of event is an HTTP request.
3. **The event is triggered.** If the defined event is an HTTP request, a user triggers the event with a click or similar action.
4. **The function is executed.** The cloud service provider checks if an instance of the function is already running. If not, it starts a new one for the function.
5. **The result is sent to the client.** The user sees the result of the executed function inside the application.

# 实践

作为开发者要做的就是寻找一个服务提供商, 然后使用相应的服务即可, 看文档调api.

这里以腾讯云为例, 主要目前(2020.5)有一定的永久白嫖额度.

腾讯云提供了: 云函数, 数据库, 云存储, 静态网站托管, 用户管理这几个基础功能, 基本是提供了一个网站的完善基础设施. 下面简短介绍下核心功能.

这些功能都需要登录使用, 微信登录或自定义对接腾讯云的用户系统.

## 云函数

实现后端各种细致的功能逻辑, 本质是传一个npm包上去执行. 计费方式是"函数占用内存乘以执行时间"的量.

## 数据库

类似MongoDB的操作, 把crud数据库这件事放到前端来做了, 类似GraphQL的感觉.

---

这么看简单的项目都不需要后端了, 前端全包了, 真就"大前端"了. 复杂的大项目需要时间检验, 此处无法断言, 但看好.

该说前端价值变高了还是工作更多了, 又或者不是叫前端而是叫"web+node+serverless全栈"?





