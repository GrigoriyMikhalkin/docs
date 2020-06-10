---
description: >-
  中间件是一个作用于 HTTP 请求周期链中的一个函数，它可以访问用于执行特定操作的上下文. 例如，记录每个请求或启用 CORS。
---

# 🧬 中间件

## Fiber 中间件

 以下是 [Fiber 团队](https://github.com/orgs/gofiber/people)维护的中间件模块。

| 中间件                                                                                                                  | 说明                                                                                                                                                                  | 内置实现                     |
|:-------------------------------------------------------------------------------------------------------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------- |:------------------------ |
| [**adaptor**](https://github.com/gofiber/adaptor)                                                                    | 支持从 net/http 处理器到 Fiber 处理器之间的双向转换，此处鸣谢 @arsmn                                                                                                                      | -                        |
| [**basicauth**](https://github.com/gofiber/basicauth)                                                                | 基本验证中间件提供一个 HTTP 基本验证。 如果提供的是有效的验证消息，那么自动调用 next 进入下一步；否则返回 401 Unauthorized 表示找不到验证信息或验证信息无效。                                                                      | -                        |
| [**compression**](https://github.com/Fenny/fiber/blob/master/middleware/compress.md)                                 | Fiber 的压缩中间件，默认支持 `deflate`、`gzip` 和 `brotli` 这几种压缩方式。                                                                                                              | `middleware.Compress()`  |
| [**cors**](https://github.com/gofiber/cors)                                                                          | 支持 cross-origin 资源访问\(CORS\)，提供了一系列参数用于自定义。                                                                                                                       | -                        |
| [**csrf**](https://github.com/gofiber/csrf)                                                                          | 提供 CSRF 防护。                                                                                                                                                         | -                        |
| [**embed**](https://github.com/gofiber/embed)                                                                        | FileServer 中间件，特别鸣谢 Alireza Salary                                                                                                                                  | -                        |
| \*\*\*\*[**favicon**](https://github.com/gofiber/fiber/blob/master/middleware/favicon.md)\*\*\*\*    | Ignore favicon from logs or serve from memory if a file path is provided.                                                                                           | `middleware.Favicon()`   |
| \*\*\*\*[**helmet**](https://github.com/gofiber/helmet)\*\*\*\*                                      | 通过设置一系列 HTTP 首部来保护您的应用。                                                                                                                                             | -                        |
| [**jwt**](https://github.com/gofiber/jwt)                                                                            | 返回 JSON Web Token \(JWT\) 验证中间件                                                                                                                                   | -                        |
| [**keyauth**](https://github.com/gofiber/keyauth)                                                                    | 提供基于键的验证方式。                                                                                                                                                         | -                        |
| [**limiter**](https://github.com/gofiber/limiter)                                                                    | Rate-limiting middleware for Fiber. 用来限制重复请求到公共API 和/或 端点，例如密码重置。                                                                                                   | -                        |
| \*\*\*\*[**logger**](https://github.com/gofiber/fiber/blob/master/middleware/logger.md)\*\*\*\*      | HTTP request/response logger.                                                                                                                                       | `middleware.Logger()`    |
| [**pprof**](https://github.com/gofiber/pprof)                                                                        | Special thanks to Matthew Lee \(@mthli\)                                                                                                                          | -                        |
| \*\*\*\*[**recover**](https://github.com/gofiber/fiber/blob/master/middleware/recover_id.md)\*\*\*\* | Recover middleware recovers from panics anywhere in the stack chain and handles the control to the centralized[ ErrorHandler](error-handling.md).                   | `middleware.Recover()`   |
| [**rewrite**](https://github.com/gofiber/rewrite)                                                                    | Rewrite middleware rewrites the URL path based on provided rules. It can be helpful for backward compatibility or just creating cleaner and more descriptive links. | -                        |
| \*\*\*\*[**requestid**](https://github.com/Fenny/fiber/blob/master/middleware/request_id.md)\*\*\*\* | Request ID middleware generates a unique id for a request.                                                                                                          | `middleware.RequestID()` |
| [**session**](https://github.com/gofiber/session)                                                                    | This session middleware is build on top of fasthttp/session by @savsgio MIT. Special thanks to @thomasvvugt for helping with this middleware.                       | -                        |
| [**template**](https://github.com/gofiber/template)                                                                  | This package contains 8 template engines that can be used with Fiber `v1.10.x` Go version 1.13 or higher is required.                                               | -                        |
| [**websocket**](https://github.com/gofiber/websocket)                                                                | Based on Fasthttp WebSocket for Fiber with Locals support!                                                                                                          | -                        |

## Third-Party Middleware

These are some additional popular middleware modules created by the community. Please open an [issue ](https://github.com/gofiber/fiber/issues)if you want to see yours.

<table>
  <thead>
    <tr>
      <th style="text-align:left">中间件</th>
      <th style="text-align:left">说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><a href="https://github.com/arsmn/fiber-swagger"><b>fiber-swagger</b></a>
      </td>
      <td style="text-align:left">Automatically generate RESTful API documentation with Swagger 2.0.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/arsmn/fiber-casbin"><b>fiber-casbin</b></a>
      </td>
      <td style="text-align:left">Casbin middleware for Fiber</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/arsmn/fiber-introspect"><b>fiber-introspect</b></a>
      </td>
      <td style="text-align:left">
        <p>Introspection middleware for Fiber</p>
        <p>Provides verifying an access token against a remote Introspection endpoint
          (RFC7662)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/shareed2k/fiber_tracing"><b>fiber_tracing</b></a>
      </td>
      <td style="text-align:left">Middleware that trace requests on <a href="https://gofiber.io/">Fiber framework</a> with
        OpenTracing API. You can use every tracer that implement OpenTracing interface</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/shareed2k/fiber_limiter"><b>fiber_limiter</b></a>
      </td>
      <td style="text-align:left">fiber_limiter using <a href="https://github.com/go-redis/redis">redis</a> as
        store for rate limit with two algorithms for choosing sliding window, gcra
        <a
        href="https://en.wikipedia.org/wiki/Leaky_bucket">leaky bucket</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/thomasvvugt/fiber-boilerplate"><b>fiber-boilerplate</b></a>
      </td>
      <td style="text-align:left">A boilerplate for the Fiber web framework</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/arsmn/gqlgen"><b>gqlgen</b></a>
      </td>
      <td style="text-align:left">A Go library for building GraphQL servers without any fuss with Fasthttp
        support</td>
    </tr>
  </tbody>
</table>

## Guidelines

{% hint style="warning" %}
**Unfinished documentation**
{% endhint %}

