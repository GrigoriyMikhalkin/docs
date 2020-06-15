---
description: کلمه app معمولا به یک برنامه Fiber اشاره می کند.
---

# 🚀 Application

## New

This method creates a new **App** named instance. You can pass optional [settings ](app.md#settings)when creating a new instance

{% code title="Signature" %}
```go
fiber.New(settings ...*Settings) *App
```
{% endcode %}

{% code title="Example" %}
```go
package main

import "github.com/gofiber/fiber"

func main() {
    app := fiber.New()

    // ...

    app.Listen(3000)
}
```
{% endcode %}

## Settings

شما می توانید هنگام فراخوانی `New` تنظیمات برنامه را ارسال کنید.

{% code title="Example" %}
```go
func main() {
    // Pass Settings creating a new instance
    app := fiber.New(&fiber.Settings{
        Prefork:       true,
        CaseSensitive: true,
        StrictRouting: true,
        ServerHeader:  "Fiber",
    })

    // ...

    app.Listen(3000)
}
```
{% endcode %}

یا بعد از ایجاد یک `app` تنظیمات آن را تغییر دهید.

{% code title="Example" %}
```go
func main() {
    app := fiber.New()

    // Or change Settings after creating an instance
    app.Settings.Prefork = true
    app.Settings.CaseSensitive = true
    app.Settings.StrictRouting = true
    app.Settings.ServerHeader = "Fiber"

    // ...

    app.Listen(3000)
}
```
{% endcode %}

**پارامترهای** **تنظیمات**

| ویژگی                     | نوع             | توضیحات                                                                                                                                                                                                                                                                 | پیش‌فرض           |
|:------------------------- |:--------------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |:----------------- |
| Prefork                   | `bool`          | استفاده از گزینه سوکت [`SO_REUSEPORT`](https://lwn.net/Articles/542629/) را فعال می کند. این گزینه باعث می شود تا چندین پردازش Go از یک پورت استفاده کنند. در مورد [socket sharding](https://www.nginx.com/blog/socket-sharding-nginx-release-1-9-1/) بیشتر یاد بگیرید. | `false`           |
| ServerHeader              | `string`        | هدر HTTP `Server` را با مقدار داده شده مقداردهی می کند.                                                                                                                                                                                                                 | `""`              |
| StrictRouting             | `bool`          | در صورت فعال بودن، روتر برای `/foo` و `/foo/` تمایز قائل می شود. در غیر این صورت، روتر برای `/foo` و `/foo/` یکسان عمل می کند.                                                                                                                                          | `false`           |
| CaseSensitive             | `bool`          | در صورت فعال بودن، `/Foo` و `/foo` مسیرهای متفاوتی هستند. در صورت غیرفعال بودن، `/Foo`و `/foo` یکسان تلقی می شوند.                                                                                                                                                      | `false`           |
| Immutable                 | `bool`          | درصورت فعال بودن، تمام مقادیر به صورت تغییرناپذیر توسط context بازگشت داده می شوند. به صورت پیش‌فرض تمام مقادیر تا هنگامی که از هندلر برمی گردید معتبر هستند، این موضوع را ببینید [\#185](https://github.com/gofiber/fiber/issues/185).                               | `false`           |
| BodyLimit                 | `int`           | حداکثر اندازه بدنه ی یک درخواست را تنظیم می کند، اگر اندازه از حد تنظیم شده تجاوز کند، خطای `413 - Request Entity Too Large` ارسال می کند.                                                                                                                              | `4 * 1024 * 1024` |
| CompressedFileSuffix      | `string`        | Adds suffix to the original file name and tries saving the resulting compressed file under the new file name.                                                                                                                                                           | `".fiber.gz"`     |
| Concurrency               | `int`           | حداکثر تعداد کانکشن های همزمان.                                                                                                                                                                                                                                         | `256 * 1024`      |
| DisableKeepalive          | `bool`          | با غیرفعال کردن کانکشن های keep-alive، سرور کانکشن های ورودی را بعد از ارسال اولین پاسخ به کلاینت خواهد بست                                                                                                                                                             | `false`           |
| DisableDefaultDate        | `bool`          | در صورت مقداردهی با true، هدر پیش‌فرض تاریخ حذف می شود.                                                                                                                                                                                                                 | `false`           |
| DisableDefaultContentType | `bool`          | در صورت مقداردهی با true، باعث می شود تا هدر Content-Type از ریسپانس حذف شود.                                                                                                                                                                                           | `false`           |
| DisableStartupMessage     | `bool`          | در صورت مقداردهی با true، پیغام fiber ASCII و "listening" چاپ نمی شود                                                                                                                                                                                                   | `false`           |
| DisableHeaderNormalizing  | `bool`          | By default all header names are normalized: conteNT-tYPE -&gt; Content-Type                                                                                                                                                                                       | `false`           |
| ETag                      | `bool`          | فعال یا غیرفعال کردن ساختن هدر ETag، در صورت فعال بودن هر دو حالت ETag ضعیف و قوی با استفاده از یک متد هش یکسان ساخته می شوند \(CRC-32\). در صورت فعال بودن، ETagهای ضعیف پیش‌فرض هستند.                                                                              | `false`           |
| Templates                 | `Templates`     | Templates is the interface that wraps the Render function. See our [**Template Middleware**]() for supported engines.                                                                                                                                                   | `nil`             |
| ReadTimeout               | `time.Duration` | مقدار زمان مجاز به خواندن کامل درخواست شامل بدنه. مهلت پیش‌فرض نامحدود است.                                                                                                                                                                                             | `nil`             |
| WriteTimeout              | `time.Duration` | حداکثر مدت زمان قبل از پایان زمان نوشتن پاسخ. مهلت پیش‌فرض نامحدود است.                                                                                                                                                                                                 | `nil`             |
| IdleTimeout               | `time.Duration` | حداکثر مدت زمان برای منتظر ماندن تا درخواست بعدی هنگامی که keep-alive فعال شده است. اگر IdleTimeout صفر باشد، از مقدار ReadTimeout استفاده می شود.                                                                                                                      | `nil`             |

## Static

از متد **Static** برای پردازش فایل های استاتیک مثل **images** ،**CSS** و **JavaScript** استفاده کنید.

{% hint style="info" %}
By default, **Static** will serve `index.html` files in response to a request on a directory.
{% endhint %}

{% code title="Signature" %}
```go
app.Static(prefix, root string, config ...Static) // => with prefix
```
{% endcode %}

از کد زیر برای پردازش فایل های یک دایرکتوری به اسم `./public` استفاده کنید

{% code title="Example" %}
```go
app.Static("/", "./public")

// => http://localhost:3000/hello.html
// => http://localhost:3000/js/jquery.js
// => http://localhost:3000/css/style.css
```
{% endcode %}

برای پردازش از چند دایرکتوری، می توانید از **Static** چندین بار استفاده کنید.

{% code title="Example" %}
```go
// Serve files from "./public" directory:
app.Static("/", "./public")

// Serve files from "./files" directory:
app.Static("/", "./files")
```
{% endcode %}

{% hint style="info" %}
از یک reverse proxy cache مثل [**NGINX**](https://www.nginx.com/resources/wiki/start/topics/examples/reverseproxycachingexample/) برای افزایش کارایی پردازش فایل های استاتیک استفاده کنید.
{% endhint %}

از هر پیشوند مسیر مجازی \(_در حالی که این مسیر در سیستم فایل وجود ندارد_\) برای پردازش فایل هایی که توسط متد **Static** انجام می شود، می توانید استفاده کنید. مانند مثال زیر، برای مسیر دایرکتوری استاتیک یک پشوند تعیین کنید:

{% code title="Example" %}
```go
app.Static("/static", "./public")

// => http://localhost:3000/static/hello.html
// => http://localhost:3000/static/js/jquery.js
// => http://localhost:3000/static/css/style.css
```
{% endcode %}

اگر می خواهید کنترل بیشتری روی پردازش فایل های استاتیک داشته باشید، You could use the `fiber.Static` struct to enable specific settings.

{% code title="fiber.Static{}" %}
```go
// Static represents settings for serving static files
type Static struct {
    // Transparently compresses responses if set to true
    // This works differently than the github.com/gofiber/compression middleware
    // The server tries minimizing CPU usage by caching compressed files.
    پسوند "fiber.gz." را به نام اصلی فایل اضافه می کند.
    // اختیاری. مقدار پیش‌فرض false
    Compress bool
    // اگر به true تنظیم شود، درخواست های byte range را فعال می کند.
    // اختیاری. مقدار پیش‌فرض false
    ByteRange bool
    // فعال کردن جستجوگر دایرکتوری.
    // اختیاری. مقدار پیش‌فرض false.
    Browse bool
    // فایل ایندکس برای پردازش دایرکتوری.
    // اختیاری. مقدار پیش‌فرض "index.html".
    Index string
}
```
{% endcode %}

{% code title="Example" %}
```go
app.Static("/", "./public", fiber.Static{
  Compress:   true,
  ByteRange:  true,
  Browse:     true,
  Index:      "john.html"
})
```
{% endcode %}

## HTTP Methods

یک درخواست HTTP را مسیریابی می کند، در حالی که **METHOD** همان [HTTP متد](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) است.

{% code title="Signatures" %}
```go
// Add allows you to specifiy a method as value
app.Add(method, path string, handlers ...func(*Ctx)) *Route

// All will register the route on all methods
app.All(path string, handlers ...func(*Ctx)) []*Route

// HTTP methods
app.Get(path string, handlers ...func(*Ctx)) *Route
app.Put(path string, handlers ...func(*Ctx)) *Route
app.Post(path string, handlers ...func(*Ctx)) *Route
app.Head(path string, handlers ...func(*Ctx)) *Route
app.Patch(path string, handlers ...func(*Ctx)) *Route
app.Trace(path string, handlers ...func(*Ctx)) *Route
app.Delete(path string, handlers ...func(*Ctx)) *Route
app.Connect(path string, handlers ...func(*Ctx)) *Route
app.Options(path string, handlers ...func(*Ctx)) *Route

// Use is mostly used for middleware modules
// These routes will only match the beggining of each path
// i.e. "/john" will match "/john/doe", "/johnnnn"
app.Use(handlers ...func(*Ctx)) *Route
app.Use(prefix string, handlers ...func(*Ctx)) *Route
```
{% endcode %}

{% code title="Example" %}
```go
app.Use("/api", func(c *fiber.Ctx) {
  c.Set("X-Custom-Header", random.String(32))
  c.Next()
})
app.Get("/api/list", func(c *fiber.Ctx) {
  c.Send("I'm a GET request!")
})
app.Post("/api/register", func(c *fiber.Ctx) {
  c.Send("I'm a POST request!")
})
```
{% endcode %}

## Group

You can group routes by creating a `*Group` struct.

**Signature**

```go
app.Group(prefix string, handlers ...func(*Ctx)) *Group
```

**Example**

```go
func main() {
  app := fiber.New()

  api := app.Group("/api", handler)  // /api

  v1 := api.Group("/v1", handler)   // /api/v1
  v1.Get("/list", handler)          // /api/v1/list
  v1.Get("/user", handler)          // /api/v1/user

  v2 := api.Group("/v2", handler) // /api/v2
  v2.Get("/list", handler)          // /api/v2/list
  v2.Get("/user", handler)          // /api/v2/user

  app.Listen(3000)
}
```

## Routes

Routes returns all registered routes

{% code title="Signature" %}
```go
app.Routes() []*Route
```
{% endcode %}

{% code title="Example" %}
```go
app := fiber.New()

handler := func(c *fiber.Ctx) { }

app.Get("/sample", handler)
app.Post("/john", handler)
app.Put("/doe", handler)

for _, r := range app.Routes() {
    fmt.Printf("%s\t%s\n", r.Method, r.Path)
}
// GET      /sample
// POST     /john
// PUT      /doe
```
{% endcode %}

## Listen

Binds and listens for connections on the specified address. This can be a `int` for port or `string` for address.

{% code title="Signature" %}
```go
app.Listen(address interface{}, tls ...*tls.Config) error
```
{% endcode %}

{% code title="Examples" %}
```go
app.Listen(8080)
app.Listen("8080")
app.Listen(":8080")
app.Listen("127.0.0.1:8080")
```
{% endcode %}

To enable **TLS/HTTPS** you can append a [**TLS config**](https://golang.org/pkg/crypto/tls/#Config).

{% code title="Example" %}
```go
cer, err := tls.LoadX509KeyPair("server.crt", "server.key")
if err != nil {
    log.Fatal(err)
}
config := &tls.Config{Certificates: []tls.Certificate{cer}}

app.Listen(443, config)
```
{% endcode %}

## Serve

You can pass your own [`net.Listener`](https://golang.org/pkg/net/#Listener) using the `Serve` method.

{% code title="Signature" %}
```go
app.Serve(ln net.Listener, tls ...*tls.Config) error
```
{% endcode %}

{% hint style="warning" %}
**Serve** does not support the [**Prefork**](app.md#settings) feature.
{% endhint %}

{% code title="Example" %}
```go
if ln, err = net.Listen("tcp4", ":8080"); err != nil {
    log.Fatal(err)
}

app.Serve(ln)
```
{% endcode %}

## Test

Testing your application is done with the **Test** method. Use this method for creating `_test.go` files or when you need to debug your routing logic. The default timeout is `200ms` if you want to disable a timeout completely, pass `-1` as a second argument.

{% code title="Signature" %}
```go
app.Test(req *http.Request, msTimeout ...int) (*http.Response, error)
```
{% endcode %}

{% code title="Example" %}
```go
// Create route with GET method for test:
app.Get("/", func(c *Ctx) {
  fmt.Println(c.BaseURL())              // => http://google.com
  fmt.Println(c.Get("X-Custom-Header")) // => hi

  c.Send("hello, World!")
})

// http.Request
req := httptest.NewRequest("GET", "http://google.com", nil)
req.Header.Set("X-Custom-Header", "hi")

// http.Response
resp, _ := app.Test(req)

// Do something with results:
if resp.StatusCode == 200 {
  body, _ := ioutil.ReadAll(resp.Body)
  fmt.Println(string(body)) // => Hello, World!
}
```
{% endcode %}
