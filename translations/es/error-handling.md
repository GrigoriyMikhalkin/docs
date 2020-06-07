---
description: >-
  Fiber supports centralized error handling by passing an error into the Next method from middleware and handlers.
---

# 🐛 Manejo de errores

Centralized error handler allows us to log errors to external services from a unified location and send a customized HTTP response to the client.

{% hint style="success" %}
You can pass a standard [**`error`**](https://golang.org/pkg/builtin/#error) or [**`fiber.*Error`**](https://godoc.org/github.com/gofiber/fiber#Error) into [**`ctx.Next(err)`**](ctx.md#next)**\`\`**
{% endhint %}

You can also use [fiber.NewError\(\)](https://sourcegraph.com/-/godoc/refs?def=NewError&pkg=github.com%2Fgofiber%2Ffiber&repo=github.com%2Fgofiber%2Ffiber) without a message, in that case status text is used as an error message. For example, `404` will show `Not Found` as the body response.

{% tabs %}
{% tab title="Example" %}
```go
app.Use(func(c *fiber.Ctx) {
    // Handler logic

    // If credentials are invalid, we call the ErrorHandler
    if !valid {
        // c.Next(errors.New("Invalid Credentials")
        // 500 Invalid Credentials

        // c.Next(fiber.Error(401))
        // 401 Unauthorized

        c.Next(fiber.Error(401, "Invalid Credentials))
        // 401 Invalid Credentials
        return
    }

    // Continue to the next handler
    c.Next()
})
```
{% endtab %}
{% endtabs %}

## Default Error Handler

Fiber provides a error handler by default. For a standard error, response is sent as **500 Internal Server Error**. If error is [fiber\*Error](https://godoc.org/github.com/gofiber/fiber#Error), response is sent with the provided status code and message.

{% code title="Example" %}
```go
app := fiber.New()

// This is the default error handler
app.Settings.ErrorHandler = func(ctx *Ctx, err error) {
    code := StatusInternalServerError
    if e, ok := err.(*Error); ok {
        code = e.Code
    }
    ctx.Status(code).SendString(err.Error())
}
```
{% endcode %}

## Custom Error Handler

Custom error handler can be set via `app.Settings.ErrorHandler`

For most cases the default error handler should be sufficient. However, a custom error handler can come in handy if you want to capture different type of errors and take action accordingly e.g. send notification email or log error to a centralized system. You can also send customized response to the client e.g. error page or just a JSON response.

The following example shows how to display error pages for different type of errors.

{% code title="Example" %}
```go
app := fiber.New()

// We override the default error handler
app.Settings.ErrorHandler = func(ctx *Ctx, err error) {
    code := StatusInternalServerError
    if e, ok := err.(*Error); ok {
        code = e.Code
    }
    page := fmt.Sprintf("%d.html", code)
    ctx.Status(code).SendFile(page)
}
```
{% endcode %}

> Special thanks to the [Echo](https://echo.labstack.com/) framework

