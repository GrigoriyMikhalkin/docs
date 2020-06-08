---
description: >-
  Un middleware es una función interpuesta en el ciclo de vida de una solicitud HTTP con acceso al contexto o "Context" que se utiliza para realizar una acción específica, por ejemplo registrar cada solicitud o habilitar CORS.
---

# 🧬 Middleware

## Middleware de Fiber

 Los módulos de middleware de Fiber listados aquí son mantenidos por el [equipo de Fiber](https://github.com/orgs/gofiber/people).

| Middleware                                                                                                           | Descripción                                                                                                                                                         | Built-in middleware      |
|:-------------------------------------------------------------------------------------------------------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------- |:------------------------ |
| [**adaptor**](https://github.com/gofiber/adaptor)                                                                    | Conversor entre manejadores net/http y manejadores de petición de FIber. ¡Nuestro especial agradecimiento a @arsmn!                                                 | -                        |
| [**basicauth**](https://github.com/gofiber/basicauth)                                                                | Provee una capa de autenticación HTTP. Llama al siguiente manejador si los credenciales son válidos o devuelve un 401 si no hay credenciales o si son inválidos.    | -                        |
| [**compression**](https://github.com/Fenny/fiber/blob/master/middleware/compress.md)                                 | Compression middleware for Fiber, it supports `deflate`, `gzip` and `brotli` by default.                                                                            | `middleware.Compress()`  |
| [**cors**](https://github.com/gofiber/cors)                                                                          | Activa CORS, del inglés Cross-Origin Resource Sharing, con varias opciones.                                                                                         | -                        |
| [**csrf**](https://github.com/gofiber/csrf)                                                                          | Protección contra explotación de CSRF (Cross-site request forgery).                                                                                                 | -                        |
| [**embed**](https://github.com/gofiber/embed)                                                                        | Middleware de servidor de ficheros. Gracias a Alireza Salary.                                                                                                       | -                        |
| \*\*\*\*[**favicon**](https://github.com/gofiber/fiber/blob/master/middleware/favicon.md)\*\*\*\*    | Ignore favicon from logs or serve from memory if a file path is provided.                                                                                           | `middleware.Favicon()`   |
| \*\*\*\*[**helmet**](https://github.com/gofiber/helmet)\*\*\*\*                                      | Helps secure your apps by setting various HTTP headers.                                                                                                             | -                        |
| [**jwt**](https://github.com/gofiber/jwt)                                                                            | JWT returns a JSON Web Token \(JWT\) auth middleware.                                                                                                             | -                        |
| [**keyauth**](https://github.com/gofiber/keyauth)                                                                    | Key auth middleware provides a key based authentication.                                                                                                            | -                        |
| [**limiter**](https://github.com/gofiber/limiter)                                                                    | Rate-limiting middleware for Fiber. Use to limit repeated requests to public APIs and/or endpoints such as password reset.                                          | -                        |
| \*\*\*\*[**logger**](https://github.com/gofiber/fiber/blob/master/middleware/logger.md)\*\*\*\*      | HTTP request/response logger.                                                                                                                                       | `middleware.Logger()`    |
| [**pprof**](https://github.com/gofiber/pprof)                                                                        | Special thanks to Matthew Lee \(@mthli\)                                                                                                                          | -                        |
| \*\*\*\*[**recover**](https://github.com/gofiber/fiber/blob/master/middleware/recover_id.md)\*\*\*\* | Recover middleware recovers from panics anywhere in the stack chain and handles the control to the centralized[ ErrorHandler](error-handling.md).                   | `middleware.Recover()`   |
| [**rewrite**](https://github.com/gofiber/rewrite)                                                                    | Rewrite middleware rewrites the URL path based on provided rules. It can be helpful for backward compatibility or just creating cleaner and more descriptive links. | -                        |
| \*\*\*\*[**requestid**](https://github.com/Fenny/fiber/blob/master/middleware/request_id.md)\*\*\*\* | Request ID middleware generates a unique id for a request.                                                                                                          | `middleware.RequestID()` |
| [**session**](https://github.com/gofiber/session)                                                                    | This session middleware is build on top of fasthttp/session by @savsgio MIT. Special thanks to @thomasvvugt for helping with this middleware.                       | -                        |
| [**template**](https://github.com/gofiber/template)                                                                  | This package contains 8 template engines that can be used with Fiber `v1.10.x` Go version 1.13 or higher is required.                                               | -                        |
| [**websocket**](https://github.com/gofiber/websocket)                                                                | Based on Fasthttp WebSocket for Fiber with Locals support!                                                                                                          | -                        |

## Middleware de terceros

A continuación algunos módulos de los más populares creados por la comunidad. Por favor, abre un [informe de problema](https://github.com/gofiber/fiber/issues) si quieres ver el tuyo.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Middleware</th>
      <th style="text-align:left">Descripción</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><a href="https://github.com/arsmn/fiber-swagger"><b>fiber-swagger</b></a>
      </td>
      <td style="text-align:left">Genera documentación automática de API RESTFUL con Swagger 2.0.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/arsmn/fiber-casbin"><b>fiber-casbin</b></a>
      </td>
      <td style="text-align:left">Middleware Casbin (autenticación) para Fiber.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/arsmn/fiber-introspect"><b>fiber-introspect</b></a>
      </td>
      <td style="text-align:left">
        <p>Middleware de introspección.</p>
        <p>Provee verificación de acceso por token contra un servidor remoto de introspección (RFC7662).</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/shareed2k/fiber_tracing"><b>fiber_tracing</b></a>
      </td>
      <td style="text-align:left">Traza las peticiones en <a href="https://gofiber.io/">Fiber</a>  con OpenTracing API. Puedes utilizar cualquier trazador que implemente la interfaz OpenTracing.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/shareed2k/fiber_limiter"><b>fiber_limiter</b></a>
      </td>
      <td style="text-align:left">Limita el ratio de peticiones, usando Redis como almacenamiento y dos algoritmos: sliding window y gcra <a href="https://en.wikipedia.org/wiki/Leaky_bucket">leaky bucket</a>.
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/thomasvvugt/fiber-boilerplate"><b>fiber-boilerplate</b></a>
      </td>
      <td style="text-align:left">Una plantilla de proyecto con bastantes añadidos listos para utilizar.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/arsmn/gqlgen"><b>gqlgen</b></a>
      </td>
      <td style="text-align:left">Una biblioteca Go para construir servidores GraphQL sin preocupaciones y con Fasthttp.</td>
    </tr>
  </tbody>
</table>

## Líneas generales

{% hint style="warning" %}
**Documentación sin terminar.**
{% endhint %}

