---
description: Μια τεκμηρίωση API, ώστε να μπορείτε να ξεκινήσετε τη δημιουργία εφαρμογών με το Fiber.
---

# 📖 Εισαγωγή

[![](https://img.shields.io/github/release/gofiber/fiber?style=flat-square)](https://github.com/gofiber/fiber/releases) [![](https://img.shields.io/badge/go.dev-007d9c?logo=go&logoColor=white&style=flat-square
)](https://pkg.go.dev/github.com/gofiber/fiber?tab=doc) [![](https://goreportcard.com/badge/github.com/gofiber/fiber?style=flat-square
)](https://goreportcard.com/report/github.com/gofiber/fiber) [![](https://img.shields.io/badge/coverage-91%25-brightgreen?style=flat-square
)](https://gocover.io/github.com/gofiber/fiber) [![](https://img.shields.io/github/workflow/status/gofiber/fiber/Test?label=tests&style=flat-square
)](https://github.com/gofiber/fiber/actions?query=workflow%3ATest) [![](https://img.shields.io/github/workflow/status/gofiber/fiber/Gosec?label=gosec&style=flat-square
)](https://github.com/gofiber/fiber/actions?query=workflow%3AGosec)

Το **Fiber** είναι ένα διαδικτυακό πλαίσιο εμπνευσμένο από την [Express](https://github.com/expressjs/express)  πάνω από το [Fasthttp](https://github.com/valala/fasthttp), τον  **ταχύτερο**  κινητήρα HTTP για τ [Go](https://golang.org/doc/). Σχεδιασμένο για να  **διευκολύνει** τα πράγματα για **γρήγορη** ανάπτυξη με **μηδενική**  κατανομή μνήμης.

## Installation

Πρώτα απ 'όλα, [ κατεβάστε ](https://golang.org/dl/) και εγκαταστήστε το Go. ` 1.11 ` ή υψηλότερη απαιτείται.

Η εγκατάσταση γίνεται έτσι [ ` go get ` ](https://golang.org/cmd/go/#hdr-Add_dependencies_to_current_module_and_install_them):

```bash
go get -u github.com/gofiber/fiber
```

## Zero Allocation

{% hint style="warning" %}
Values returned from [**fiber.Ctx**](context.md) are **not** immutable by default
{% endhint %}

Because fiber is optimized for **high performance**, values returned from [**fiber.Ctx**](context.md) are **not** immutable by default and **will** be re-used across requests. As a rule of thumb, you **must** only use context values within the handler, and you **must not** keep any references. As soon as you return from the handler, any values you have obtained from the context will be re-used in future requests and will change below your feet. Here is an example:

```go
func handler(c *fiber.Ctx) {
    result := c.Param("foo") // result is only valid within this method
}
```

If you need to persist such values outside the handler, make copies of their **underlying buffer** using the [copy](https://golang.org/pkg/builtin/#copy) builtin. Here is an example for persisting a string:

```go
func handler(c *fiber.Ctx) {
    result := c.Param("foo") // result is only valid within this method
    newBuffer := make([]byte, len(result))
    copy(newBuffer, result)
    newResult := string(newBuffer) // newResult is immutable and valid forever
}
```

Alternatively, you can also use the[ **Immutable setting**](application.md#settings). It will make all values returned from the context immutable, allowing you to persist them anywhere. Of course, this comes at the cost of performance.

For more information, please check ****[**\#426**](https://github.com/gofiber/fiber/issues/426) and ****[**\#185**](https://github.com/gofiber/fiber/issues/185).

## Hello, World!

Embedded below is essentially simplest **Fiber** app, which you can create.

```go
package main

import "github.com/gofiber/fiber"

func main() {
  app := fiber.New()

  app.Get("/", func(c *fiber.Ctx) {
    c.Send("Hello, World!")
  })

  app.Listen(3000)
}
```

```text
go run server.go
```

Browse to `http://localhost:3000` and you should see `Hello, World!` on the page.

## Basic routing

Routing refers to determining how an application responds to a client request to a particular endpoint, which is a URI \(or path\) and a specific HTTP request method \(GET, PUT, POST and so on\).

{% hint style="info" %}
Each route can have **multiple handler functions**, that are executed when the route is matched.
{% endhint %}

Route definition takes the following structures:

```go
// Function signature
app.Method(path string, ...func(*fiber.Ctx))
```

* Το ` app ` είναι μια παρουσία του ** Fiber **.
* ` Method ` είναι μια [ μέθοδος αιτήματος HTTP ](https://fiber.wiki/application#methods), με κεφαλαία γράμματα: `Get`, `Put`, `Post` κ.λπ.
* Το ` path ` είναι μια εικονική διαδρομή στο διακομιστή.
* Το ` func (* fiber.Ctx) ` είναι μια συνάρτηση επανάκλησης που περιέχει το [ Context ](https://fiber.wiki/context) που εκτελείται κατά την αντιστοίχιση της διαδρομής.

**Simple route**

```go
// Respond with "Hello, World!" on root path, "/"
app.Get("/", func(c *fiber.Ctx) {
  c.Send("Hello, World!")
})
```

**Παράμετροι**

```go
// GET http://localhost:8080/hello%20world

app.Get("/:value", func(c *fiber.Ctx) {
  c.Send("Get request with value: " + c.Params("value"))
  // => Get request with value: hello world
})
```

**Optional parameter**

```go
// GET http://localhost:3000/john

app.Get("/:name?", func(c *fiber.Ctx) {
  if c.Params("name") != "" {
    c.Send("Hello " + c.Params("name"))
    // => Hello john
  } else {
    c.Send("Where is john?")
  }
})
```

**Wildcards**

```go
// GET http://localhost:3000/api/user/john

app.Get("/api/*", func(c *fiber.Ctx) {
  c.Send("API path: " + c.Params("*"))
  // => API path: user/john
})
```

## Static files

To serve static files such as **images**, **CSS** and **JavaScript** files, replace your function handler with a file or directory string.

Function signature:

```go
app.Static(prefix, root string)
```

Use the following code to serve files in a directory named `./public`:

```go
app := fiber.New()

app.Static("/", "./public") 

app.Listen(8080)
```

Now, you can load the files that are in the `./public` directory:

```bash
http://localhost:8080/hello.html
http://localhost:8080/js/jquery.js
http://localhost:8080/css/style.css
```
