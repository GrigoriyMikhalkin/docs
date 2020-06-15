---
description: >-
  Ctx 구조체는 HTTP 요청과 응답을 가지고 있는 Context를 나타냅니다. 그것은 요청 쿼리 문자열, 파라미터, 바디, HTTP 헤더 등을 위한 메소드들을 가지고 있습니다.
---

# 🧠 Context

## Accepts

**extensions** 또는 **content** **types** 가 허용 되는지 확인합니다.

{% hint style="info" %}
요청의 [Accept](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept) HTTP 헤더에 기반합니다.
{% endhint %}

{% code title="Signature" %}
```go
c.Accepts(types ...string)                 string
c.AcceptsCharsets(charsets ...string)      string
c.AcceptsEncodings(encodings ...string)    string
c.AcceptsLanguages(langs ...string)        string
```
{% endcode %}

{% code title="Example" %}
```go
// Accept: text/*, application/json

app.Get("/", func(c *fiber.Ctx) {
  c.Accepts("html")             // "html"
  c.Accepts("text/html")        // "text/html"
  c.Accepts("json", "text")     // "json"
  c.Accepts("application/json") // "application/json"
  c.Accepts("image/png")        // ""
  c.Accepts("png")              // ""
})
```
{% endcode %}

Fiber는 다른 accept 헤더들을 위한 유사한 함수들도 제공합니다.

```go
// Accept-Charset: utf-8, iso-8859-1;q=0.2
// Accept-Encoding: gzip, compress;q=0.2
// Accept-Language: en;q=0.8, nl, ru

app.Get("/", func(c *fiber.Ctx) {
  c.AcceptsCharsets("utf-16", "iso-8859-1") 
  // "iso-8859-1"

  c.AcceptsEncodings("compress", "br") 
  // "compress"

  c.AcceptsLanguages("pt", "nl", "ru") 
  // "nl"
})
```

## Append

명시된 **value** 를 HTTP 응답 헤더 영역에 덧붙입니다.

{% hint style="warning" %}
만약 헤더가 이미 설정되지 **않았다면**, 그것은 명시된 값을 가진 헤더를 생성합니다.
{% endhint %}

{% code title="Signature" %}
```go
c.Append(field, values ...string)
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.Append("Link", "http://google.com", "http://localhost")
  // => Link: http://localhost, http://google.com

  c.Append("Link", "Test")
  // => Link: http://localhost, http://google.com, Test
})
```
{% endcode %}

## Attachment

HTTP 응답 [Content-Disposition](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition) 헤더 영역을 `attachment` 로 설정합니다.

{% code title="Signature" %}
```go
c.Attachment(file ...string)
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.Attachment()
  // => Content-Disposition: attachment

  c.Attachment("./upload/images/logo.png")
  // => Content-Disposition: attachment; filename="logo.png"
  // => Content-Type: image/png
})
```
{% endcode %}

## App

Returns the [\*App](app.md#new) reference so you could easily access all application settings.

{% code title="Signature" %}
```go
c.App() *App
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/bodylimit", func(c *fiber.Ctx) {
  bodylimit := c.App().Settings.BodyLimit
  c.Send(bodylimit)
})
```
{% endcode %}

## BaseURL

Base URL \(**protocol** + **host**\) 을 `string`으로 반환합니다.

{% code title="Signature" %}
```go
c.BaseURL() string
```
{% endcode %}

{% code title="Example" %}
```go
// GET https://example.com/page#chapter-1

app.Get("/", func(c *fiber.Ctx) {
  c.BaseURL() // https://example.com
})
```
{% endcode %}

## Body

**POST** 요청으로 보내진 **raw body** 를 담고 있습니다.

{% code title="Signature" %}
```go
c.Body() string
```
{% endcode %}

{% code title="Example" %}
```go
// curl -X POST http://localhost:8080 -d user=john

app.Post("/", func(c *fiber.Ctx) {
  // Get raw body from POST request:
  c.Body() // user=john
})
```
{% endcode %}

> _Returned value is only valid within the handler. Do not store any references.  
> Make copies or use the_ [_**`Immutable`**_](app.md#settings) _setting instead._ [_Read more..._](./#zero-allocation)

## BodyParser

요청 바디를 구조체로 바인드합니다. `BodyParser` 는 쿼리 파라미터 디코딩과 `Content-Type` 헤더에 기반한 다음의 content type들을 지원합니다:

* `application/json`
* `application/xml`
* `application/x-www-form-urlencoded`
* `multipart/form-data`

{% code title="Signature" %}
```go
c.BodyParser(out interface{}) error
```
{% endcode %}

{% code title="Example" %}
```go
// Field names should start with an uppercase letter
type Person struct {
    Name string `json:"name" xml:"name" form:"name" query:"name"`
    Pass string `json:"pass" xml:"pass" form:"pass" query:"pass"`
}

app.Post("/", func(c *fiber.Ctx) {
        p := new(Person)

        if err := c.BodyParser(p); err != nil {
            log.Fatal(err)
        }

        log.Println(p.Name) // john
        log.Println(p.Pass) // doe
})
// Run tests with the following curl commands

// curl -X POST -H "Content-Type: application/json" --data "{\"name\":\"john\",\"pass\":\"doe\"}" localhost:3000

// curl -X POST -H "Content-Type: application/xml" --data "<login><name>john</name><pass>doe</pass></login>" localhost:3000

// curl -X POST -H "Content-Type: application/x-www-form-urlencoded" --data "name=john&pass=doe" localhost:3000

// curl -X POST -F name=john -F pass=doe http://localhost:3000

// curl -X POST "http://localhost:3000/?name=john&pass=doe"
```
{% endcode %}

## ClearCookie

클라이언트 쿠키를 만료시킵니다 \(_또는 비어있는 모든 쿠키들\)_

{% code title="Signature" %}
```go
c.ClearCookie(key ...string)
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  // Clears all cookies:
  c.ClearCookie()

  // Expire specific cookie by name:
  c.ClearCookie("user")

  // Expire multiple cookies by names:
  c.ClearCookie("token", "session", "track_id", "version")
})
```
{% endcode %}

## Context

Returns context.Context that carries a deadline, a cancellation signal, and other values across API boundaries.

**Signature**

```go
c.Context() context.Context
```

## Cookie

쿠키를 설정합니다

**Signature**

```text
c.Cookie(*Cookie)
```

```go
type Cookie struct {
    Name     string
    Value    string
    Path     string
    Domain   string
    Expires  time.Time
    Secure   bool
    HTTPOnly bool
    SameSite string // lax, strict, none
}
```

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  // Create cookie
  cookie := new(fiber.Cookie)
  cookie.Name = "john"
  cookie.Value = "doe"
  cookie.Expires = time.Now().Add(24 * time.Hour)

  // Set cookie
  c.Cookie(cookie)
})
```
{% endcode %}

## Cookies

키에 맞는 쿠키 값을 가져옵니다.

**Signatures**

```go
c.Cookies(key string) string
```

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  // Get cookie by key:
  c.Cookies("name") // "john"
})
```
{% endcode %}

> _Returned value is only valid within the handler. Do not store any references.  
> Make copies or use the_ [_**`Immutable`**_](app.md#settings) _setting instead._ [_Read more..._](./#zero-allocation)

## Download

경로의 파일을 `attachment` 처럼 전송합니다.

일반적으로, 브라우저는 사용자가 다운로드를 하게 합니다. By default, the [Content-Disposition](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition) header `filename=` parameter is the file path \(_this typically appears in the browser dialog_\).

이 기본값을 **filename** 파라미터와 함께 오버라이드합니다.

{% code title="Signature" %}
```go
c.Download(path, filename ...string) error
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  if err := c.Download("./files/report-12345.pdf"); err != nil {
    c.Next(err) // Pass err to fiber
  }
  // => Download report-12345.pdf

  if err := c.Download("./files/report-12345.pdf", "report.pdf"); err != nil {
    c.Next(err) // Pass err to fiber
  }
  // => Download report.pdf
})
```
{% endcode %}

## Fasthttp

여러분은 여전히 **Fasthttp** 메소드들과 속성들에 **접속**하고 사용할 수 있습니다.

**Signature**

{% hint style="info" %}
더 많은 정보는 [Fasthttp Documentation](https://pkg.go.dev/github.com/valyala/fasthttp?tab=doc) 을 읽어주세요.
{% endhint %}

**Example**

```go
app.Get("/", func(c *fiber.Ctx) {
  c.Fasthttp.Request.Header.Method()
  // => []byte("GET")

  c.Fasthttp.Response.Write([]byte("Hello, World!"))
  // => "Hello, World!"
})
```

## Error

이것은 패닉 또는 [`Next(err)`](https://github.com/gofiber/docs/tree/8d965e1e05fb67f965934586c78335ef29f52128/context/README.md#error) 메소드를 통해 던져진 에러 정보를 포함합니다.

{% code title="Signature" %}
```go
c.Error() error
```
{% endcode %}

{% code title="Example" %}
```go
func main() {
  app := fiber.New()
  app.Post("/api/register", func (c *fiber.Ctx) {
    if err := c.JSON(&User); err != nil {
      c.Next(err)
    }
  })
  app.Get("/api/user", func (c *fiber.Ctx) {
    if err := c.JSON(&User); err != nil {
      c.Next(err)
    }
  })
  app.Put("/api/update", func (c *fiber.Ctx) {
    if err := c.JSON(&User); err != nil {
      c.Next(err)
    }
  })
  app.Use("/api", func(c *fiber.Ctx) {
    c.Set("Content-Type", "application/json")
    c.Status(500).Send(c.Error())
  })
  app.Listen(1337)
}
```
{% endcode %}

## Format

[Accept](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept) HTTP 헤더의 content-negotiation을 수행합니다. 그것은 적절한 포맷을 선택하기 위해 [Accepts](ctx.md#accepts) 를 사용합니다.

{% hint style="info" %}
만약 헤더가 명시되지 **않거나** 적절한 포맷이 **없으면**, **text/plain** 이 사용됩니다.
{% endhint %}

{% code title="Signature" %}
```go
c.Format(body interface{})
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  // Accept: text/plain
  c.Format("Hello, World!")
  // => Hello, World!

  // Accept: text/html
  c.Format("Hello, World!")
  // => <p>Hello, World!</p>

  // Accept: application/json
  c.Format("Hello, World!")
  // => "Hello, World!"
})
```
{% endcode %}

## FormFile

MultipartForm 파일들은 이름을 통해 가져올 수 있으며, 주어진 키의 **첫 번째** 파일이 반환됩니다.

{% code title="Signature" %}
```go
c.FormFile(name string) (*multipart.FileHeader, error)
```
{% endcode %}

{% code title="Example" %}
```go
app.Post("/", func(c *fiber.Ctx) {
  // Get first file from form field "document":
  file, err := c.FormFile("document")

  // Check for errors:
  if err == nil {
    // Save file to root directory:
    c.SaveFile(file, fmt.Sprintf("./%s", file.Filename))
  }
})
```
{% endcode %}

## FormValue

Form 값들은 이름을 통해 가져올 수 있으며, 주어진 키의 **첫 번째** 파일이 반환됩니다.

{% code title="Signature" %}
```go
c.FormValue(name string) string
```
{% endcode %}

{% code title="Example" %}
```go
app.Post("/", func(c *fiber.Ctx) {
  // Get first value from form field "name":
  c.FormValue("name")
  // => "john" or "" if not exist
})
```
{% endcode %}

> _Returned value is only valid within the handler. Do not store any references.  
> Make copies or use the_ [_**`Immutable`**_](app.md#settings) _setting instead._ [_Read more..._](./#zero-allocation)

## Fresh

[https://expressjs.com/en/4x/api.html\#req.fresh](https://expressjs.com/en/4x/api.html#req.fresh)

{% hint style="info" %}
아직 구현되지 않았습니다, pull request는 환영입니다!
{% endhint %}

## Get

필드에 명시된 HTTP 요청 헤더를 반환합니다.

{% hint style="success" %}
비교는 **대소문자를 구분하지 않습니다**.
{% endhint %}

{% code title="Signature" %}
```go
c.Get(field string) string
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.Get("Content-Type") // "text/plain"
  c.Get("CoNtEnT-TypE") // "text/plain"
  c.Get("something")    // ""
})
```
{% endcode %}

> _Returned value is only valid within the handler. Do not store any references.  
> Make copies or use the_ [_**`Immutable`**_](app.md#settings) _setting instead._ [_Read more..._](./#zero-allocation)

## Hostname

[Host](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Host) HTTP 헤더에서 가져온 호스트 네임을 포함합니다.

{% code title="Signature" %}
```go
c.Hostname() string
```
{% endcode %}

{% code title="Example" %}
```go
// GET http://google.com/search

app.Get("/", func(c *fiber.Ctx) {
  c.Hostname() // "google.com"
})
```
{% endcode %}

> _Returned value is only valid within the handler. Do not store any references.  
> Make copies or use the_ [_**`Immutable`**_](app.md#settings) _setting instead._ [_Read more..._](./#zero-allocation)

## IP

요청의 원격 IP 주소를 반환합니다.

{% code title="Signature" %}
```go
c.IP() string
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.IP() // "127.0.0.1"
})
```
{% endcode %}

## IPs

[X-Forwarded-For](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-For) 요청 헤더에 명시된 IP 주소들의 배열을 반환합니다.

{% code title="Signature" %}
```go
c.IPs() []string
```
{% endcode %}

{% code title="Example" %}
```go
// X-Forwarded-For: proxy1, 127.0.0.1, proxy3

app.Get("/", func(c *fiber.Ctx) {
  c.IPs() // ["proxy1", "127.0.0.1", "proxy3"]
})
```
{% endcode %}

## Is

들어오는 요청의 [Content-Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) HTTP 헤더 영역이 타입 파라미터로 명시된 [MIME type](https://developer.mozilla.org/ru/docs/Web/HTTP/Basics_of_HTTP/MIME_types) 과 일치하는지를 통해, **content type** 일치하는지 여부를 반환합니다.

{% hint style="info" %}
만약 요청이 바디를 가지고 있지 **않다면**, **false**를 반환합니다.
{% endhint %}

{% code title="Signature" %}
```go
c.Is(t string) bool
```
{% endcode %}

{% code title="Example" %}
```go
// Content-Type: text/html; charset=utf-8

app.Get("/", func(c *fiber.Ctx) {
  c.Is("html")  // true
  c.Is(".html") // true
  c.Is("json")  // false
})
```
{% endcode %}

## JSON

[Jsoniter](https://github.com/json-iterator/go)를 사용해 **인터페이스** 또는 **문자열**을 JSON으로 바꿉니다.

{% hint style="info" %}
JSON은 콘텐츠 헤더도 **application/json**으로 설정합니다.
{% endhint %}

{% code title="Signature" %}
```go
c.JSON(v interface{}) error
```
{% endcode %}

{% code title="Example" %}
```go
type SomeStruct struct {
  Name string
  Age  uint8
}

app.Get("/json", func(c *fiber.Ctx) {
  // Create data struct:
  data := SomeStruct{
    Name: "Grame",
    Age:  20,
  }

  if err := c.JSON(data); err != nil {
    c.Status(500).Send(err)
    return
  }
  // => Content-Type: application/json
  // => "{"Name": "Grame", "Age": 20}"

  if err := c.JSON(fiber.Map{
    "name": "Grame",
    "age": 20,
  }); err != nil {
    c.Status(500).Send(err)
    return
  }
  // => Content-Type: application/json
  // => "{"name": "Grame", "age": 20}"
})
```
{% endcode %}

## JSONP

JSON 응답을 JSONP 지원과 함께 보냅니다. 이 메소드는 [JSON](ctx.md#json)과 동일하지만, JSONP 콜백 지원이 선택 가능합니다. 기본적으로, 콜백 이름은 단순하게 callback입니다.

메소드 안에 **지정된 문자열**을 넣어서 오버라이드 하세요.

{% code title="Signature" %}
```go
c.JSONP(v interface{}, callback ...string) error
```
{% endcode %}

{% code title="Example" %}
```go
type SomeStruct struct {
  name string
  age  uint8
}

app.Get("/", func(c *fiber.Ctx) {
  // Create data struct:
  data := SomeStruct{
    name: "Grame",
    age:  20,
  }

  c.JSONP(data)
  // => callback({"name": "Grame", "age": 20})

  c.JSONP(data, "customFunc")
  // => customFunc({"name": "Grame", "age": 20})
})
```
{% endcode %}

## Links

응답의 [Link](https://developer.mozilla.org/ru/docs/Web/HTTP/Headers/Link) HTTP 헤더 필드를 덧붙이기 위해 속성으로 따라오는 링크를 연결합니다.

{% code title="Signature" %}
```go
c.Links(link ...string)
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.Link(
    "http://api.example.com/users?page=2", "next",
    "http://api.example.com/users?page=5", "last",
  )
  // Link: <http://api.example.com/users?page=2>; rel="next",
  //       <http://api.example.com/users?page=5>; rel="last"
})
```
{% endcode %}

## Locals

요청에 국한된 문자열 변수를 저장하여서 그 요청에 맞는 라우트에서만 사용될 수 있게 해주는 메소드입니다.

{% hint style="success" %}
이것은 만약 여러분이 **특정** 데이터를 다음 미들웨어에 전달하고 싶을 때 유용합니다.
{% endhint %}

{% code title="Signature" %}
```go
c.Locals(key string, value ...interface{}) interface{}
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.Locals("user", "admin")
  c.Next()
})

app.Get("/admin", func(c *fiber.Ctx) {
  if c.Locals("user") == "admin" {
    c.Status(200).Send("Welcome, admin!")
  } else {
    c.SendStatus(403)
    // => 403 Forbidden
  }
})
```
{% endcode %}

## Location

응답의 [Location](https://developer.mozilla.org/ru/docs/Web/HTTP/Headers/Location) HTTP 헤더를 특정 경로 파라미터로 설정합니다.

{% code title="Signature" %}
```go
c.Location(path string)
```
{% endcode %}

{% code title="Example" %}
```go
app.Post("/", func(c *fiber.Ctx) {
  c.Location("http://example.com")
  c.Location("/foo/bar")
})
```
{% endcode %}

## Method

요청의 HTTP 메소드에 해당하는 문자열을 갖고 있습니다: `GET`, `POST`, `PUT` 등.   
선택적으로, 여러분은 문자열을 넣어 메소드를 오버라이드 할 수 있습니다.

{% code title="Signature" %}
```go
c.Method(override ...string) string
```
{% endcode %}

{% code title="Example" %}
```go
app.Post("/", func(c *fiber.Ctx) {
  c.Method() // "POST"
})
```
{% endcode %}

## MultipartForm

Multipart form 엔트리에 접근하기 위해, 여러분은 `MultipartForm()`으로 바이너리를 가져올 수 있습니다. 이것은 `map[string][]string` 반환하며, 키가 주어지면 값은 문자열 슬라이스 입니다.

{% code title="Signature" %}
```go
c.MultipartForm() (*multipart.Form, error)
```
{% endcode %}

{% code title="Example" %}
```go
app.Post("/", func(c *fiber.Ctx) {
  // Parse the multipart form:
  if form, err := c.MultipartForm(); err == nil {
    // => *multipart.Form

    if token := form.Value["token"]; len(token) > 0 {
      // Get key value:
      fmt.Println(token[0])
    }

    // Get all files from "documents" key:
    files := form.File["documents"]
    // => []*multipart.FileHeader

    // Loop through files:
    for _, file := range files {
      fmt.Println(file.Filename, file.Size, file.Header["Content-Type"][0])
      // => "tutorial.pdf" 360641 "application/pdf"

      // Save the files to disk:
      c.SaveFile(file, fmt.Sprintf("./%s", file.Filename))
    }
  }
})
```
{% endcode %}

## Next

**Next**가 호출되면, 이것은 현재 라우트에 맞는 스택에서 다음 매소드를 실행시킵니다. 여러분은 에러 처리를 커스텀하기 위해 메소드 안에 에러 구조체를 넣을 수 있습니다.

{% code title="Signature" %}
```go
c.Next(err ...error)
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  fmt.Println("1st route!")
  c.Next()
})

app.Get("*", func(c *fiber.Ctx) {
  fmt.Println("2nd route!")
  c.Next(fmt.Errorf("Some error"))
})

app.Get("/", func(c *fiber.Ctx) {
  fmt.Println(c.Error()) // => "Some error"
  fmt.Println("3rd route!")
  c.Send("Hello, World!")
})
```
{% endcode %}

## OriginalURL

원본 요청 URL을 가집니다.

{% code title="Signature" %}
```go
c.OriginalURL() string
```
{% endcode %}

{% code title="Example" %}
```go
// GET http://example.com/search?q=something

app.Get("/", func(c *fiber.Ctx) {
  c.OriginalURL() // "/search?q=something"
})
```
{% endcode %}

> _Returned value is only valid within the handler. Do not store any references.  
> Make copies or use the_ [_**`Immutable`**_](app.md#settings) _setting instead._ [_Read more..._](./#zero-allocation)

## Params

라우트 파라미터를 가져오기 위해 사용되는 메소드입니다.

{% hint style="info" %}
만약 파라미터가 존재하지 **않다면**, 기본 값은 빈 문자열 \(`""`\) 입니다.
{% endhint %}

{% code title="Signature" %}
```go
c.Params(param string) string
```
{% endcode %}

{% code title="Example" %}
```go
// GET http://example.com/user/fenny

app.Get("/user/:name", func(c *fiber.Ctx) {
  c.Params("name") // "fenny"
})
```
{% endcode %}

> _Returned value is only valid within the handler. Do not store any references.  
> Make copies or use the_ [_**`Immutable`**_](app.md#settings) _setting instead._ [_Read more..._](./#zero-allocation)\_\_

## Path

요청 URL의 경로 부분을 가집니다. 선택적으로, 여러분은 문자열을 넣어서 경로를 오버라이드 할 수 있습니다.

{% code title="Signature" %}
```go
c.Path(override ...string) string
```
{% endcode %}

{% code title="Example" %}
```go
// GET http://example.com/users?sort=desc

app.Get("/users", func(c *fiber.Ctx) {
  c.Path() // "/users"
})
```
{% endcode %}

## Protocol

요청 프로토콜 문자열을 가집니다: `http` 또는 **TLS** 요청을 위한 `https`.

{% code title="Signature" %}
```go
c.Protocol() string
```
{% endcode %}

{% code title="Example" %}
```go
// GET http://example.com

app.Get("/", func(c *fiber.Ctx) {
  c.Protocol() // "http"
})
```
{% endcode %}

## Query

이 속성은 라우트 안의 각 쿼리 문자열 파라미터를 위한 속성을 담고 있는 객체 입니다.

{% hint style="info" %}
만약 쿼리 문자열이 **없다면**, 그것은 **빈 문자열**을 반환합니다.
{% endhint %}

{% code title="Signature" %}
```go
c.Query(parameter string) string
```
{% endcode %}

{% code title="Example" %}
```go
// GET http://example.com/shoes?order=desc&brand=nike

app.Get("/", func(c *fiber.Ctx) {
  c.Query("order") // "desc"
  c.Query("brand") // "nike"
})
```
{% endcode %}

> _Returned value is only valid within the handler. Do not store any references.  
> Make copies or use the_ [_**`Immutable`**_](app.md#settings) _setting instead._ [_Read more..._](./#zero-allocation)

## Range

반환 될 타입과 범위의 슬라이스를 갖고 있는 구조체 입니다.

{% code title="Signature" %}
```go
c.Range(int size)
```
{% endcode %}

{% code title="Example" %}
```go
// Range: bytes=500-700, 700-900
app.Get("/", func(c *fiber.Ctx) {
  b := c.Range(1000)
  if b.Type == "bytes" {
      for r := range r.Ranges {
      fmt.Println(r)
      // [500, 700]
    }
  }
})
```
{% endcode %}

## Redirect

명시된 경로와, HTTP 상태 코드에 맞는 양의 정수인 명시된 상태에 해당하는 URL로 리다이렉트 합니다.

{% hint style="info" %}
만약 명시되지 **않았다면**, 상태는 기본 값으로 **302 Found** 입니다.
{% endhint %}

{% code title="Signature" %}
```go
c.Redirect(path string, status ...int)
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.Redirect("/foo/bar")
  c.Redirect("../login")
  c.Redirect("http://example.com")
  c.Redirect("http://example.com", 301)
})
```
{% endcode %}

## Render

데이터와 함께 템플릿을 렌더링하고 `text/html` 응답을 보냅니다. 기본적으로 `Render`는 [**Go Template engine**](https://golang.org/pkg/html/template/)을 사용합니다. If you want to use another engine, please take a look at our [**Template middleware**]().

{% code title="Signature" %}
```go
c.Render(file string, data interface{}) error
```
{% endcode %}

## Route

일치하는 [Route](https://pkg.go.dev/github.com/gofiber/fiber?tab=doc#Route) 구조체를 가집니다.

{% code title="Signature" %}
```go
c.Route() *Route
```
{% endcode %}

{% code title="Example" %}
```go
// http://localhost:8080/hello

app.Get("/hello", func(c *fiber.Ctx) {
  r := c.Route()
  fmt.Println(r.Method, r.Path, r.Params, r.Regexp, r.Handler)
})

app.Post("/:api?", func(c *fiber.Ctx) {
  c.Route()
  // => {GET /hello [] nil 0x7b49e0}
})
```
{% endcode %}

## SaveFile

**어떤** multipart 파일을 디스크에 저장할 때 사용되는 메소드 입니다.

{% code title="Signature" %}
```go
c.SaveFile(fh *multipart.FileHeader, path string)
```
{% endcode %}

{% code title="Example" %}
```go
app.Post("/", func(c *fiber.Ctx) {
  // Parse the multipart form:
  if form, err := c.MultipartForm(); err == nil {
    // => *multipart.Form

    // Get all files from "documents" key:
    files := form.File["documents"]
    // => []*multipart.FileHeader

    // Loop through files:
    for _, file := range files {
      fmt.Println(file.Filename, file.Size, file.Header["Content-Type"][0])
      // => "tutorial.pdf" 360641 "application/pdf"

      // Save the files to disk:
      c.SaveFile(file, fmt.Sprintf("./%s", file.Filename))
    }
  }
})
```
{% endcode %}

## Secure

**TLS** 연결이 성립되면, `true`인 boolean 속성입니다.

{% code title="Signature" %}
```go
c.Secure() bool
```
{% endcode %}

{% code title="Example" %}
```go
// Secure() method is equivalent to:
c.Protocol() == "https"
```
{% endcode %}

## Send

HTTP 응답 바디를 설정합니다. **Send** 바디는 어떤 타입이든 가능합니다.

{% hint style="warning" %}
Send는 [Write](https://fiber.wiki/context#write)메소드 처럼 덧붙이지 **않습니다**.
{% endhint %}

{% code title="Signature" %}
```go
c.Send(body ...interface{})
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.Send("Hello, World!")         // => "Hello, World!"
  c.Send([]byte("Hello, World!")) // => "Hello, World!"
  c.Send(123)                     // => 123
})
```
{% endcode %}

Fiber also provides `SendBytes` ,`SendString` and `SendStream` methods for raw inputs.

{% hint style="success" %}
만약 여러분이 타입 확정이 **필요하지 않다면**, **더 빠른** 성능을 위해 이것을 사용하는 것을 추천합니다.
{% endhint %}

{% code title="Signature" %}
```go
c.SendBytes(b []byte)
c.SendString(s string)
c.SendStream(r io.Reader, s ...int)
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.SendByte([]byte("Hello, World!"))
  // => "Hello, World!"

  c.SendString("Hello, World!")
  // => "Hello, World!"

  c.SendStream(bytes.NewReader([]byte("Hello, World!")))
  // => "Hello, World!"
})
```
{% endcode %}

## SendFile

주어진 경로의 파일을 전송합니다. 응답 HTTP 헤더 필드의 [Content-Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)을 **파일 이름** 확장자를 기반으로 설정합니다.

{% hint style="warning" %}
메소드는 **gzipping**을 기본으로 사용하고, 사용하지 않으려면 **true**로 설정하세요.
{% endhint %}

{% code title="Signature" %}
```go
c.SendFile(path string, compress ...bool) error
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/not-found", func(c *fiber.Ctx) {
  if err := c.SendFile("./public/404.html"); err != nil {
    c.Next(err) // pass err to ErrorHandler
  }

  // Enable compression
  if err := c.SendFile("./static/index.html", true); err != nil {
    c.Next(err) // pass err to ErrorHandler
  }
})
```
{% endcode %}

## SendStatus

만약 응답 바디가 **비어있다면**, 상태 코드와 알맞는 상태 메시지를 바디에 설정합니다.

{% hint style="success" %}
여러분은 [여기](https://github.com/gofiber/fiber/blob/dffab20bcdf4f3597d2c74633a7705a517d2c8c2/utils.go#L183-L244)에서 모든 상태 코드와 메시지들을 찾을 수 있습니다.
{% endhint %}

{% code title="Signature" %}
```go
c.SendStatus(status int)
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/not-found", func(c *fiber.Ctx) {
  c.SendStatus(415)
  // => 415 "Unsupported Media Type"

  c.Send("Hello, World!")
  c.SendStatus(415)
  // => 415 "Hello, World!"
})
```
{% endcode %}

## Set

응답의 HTTP 헤더 필드를 명시된 `key`, `value`로 설정합니다.

{% code title="Signature" %}
```go
c.Set(field, value string)
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.Set("Content-Type", "text/plain")
  // => "Content-type: text/plain"
})
```
{% endcode %}

## Stale

[https://expressjs.com/en/4x/api.html\#req.fresh](https://expressjs.com/en/4x/api.html#req.fresh)

{% hint style="info" %}
아직 구현되지 않았습니다, pull request는 환영입니다!
{% endhint %}

## Status

응답의 HTTP 상태를 설정합니다.

{% hint style="info" %}
메소드는 **chainable**입니다.
{% endhint %}

{% code title="Signature" %}
```go
c.Status(status int)
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.Status(200)
  c.Status(400).Send("Bad Request")
  c.Status(404).SendFile("./public/gopher.png")
})
```
{% endcode %}

## Subdomains

요청의 도메인 이름안에 있는 서브도메인들의 배열입니다.

기본 값으로 `2`인 어플리케이션 속성의 서브도메인 오프셋은 서브도메인 세그먼트의 시작점을 결정하는데 사용됩니다.

{% code title="Signature" %}
```go
c.Subdomains(offset ...int) []string
```
{% endcode %}

{% code title="Example" %}
```go
// Host: "tobi.ferrets.example.com"

app.Get("/", func(c *fiber.Ctx) {
  c.Subdomains()  // ["ferrets", "tobi"]
  c.Subdomains(1) // ["tobi"]
})
```
{% endcode %}

## 타입

[Content-Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) HTTP 헤더를 [여기](https://github.com/nginx/nginx/blob/master/conf/mime.types)에 있고 파일 **확장자**에 명시된 MIME 타입으로 설정합니다.

{% code title="Signature" %}
```go
c.Type(t string) string
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.Type(".html") // => "text/html"
  c.Type("html")  // => "text/html"
  c.Type("json")  // => "application/json"
  c.Type("png")   // => "image/png"
})
```
{% endcode %}

## Vary

주어진 헤더 필드를 [Vary](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Vary) 응답 헤더에 추가합니다. 이것은 만약 이미 목록에 있지 않으면 헤더를 덧붙이고, 그렇지 않으면 목록의 현재 위치에 그대로 둡니다.

{% hint style="info" %}
여러 개의 필드들이 **허용됩니다**.
{% endhint %}

{% code title="Signature" %}
```go
c.Vary(field ...string)
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.Vary("Origin")     // => Vary: Origin
  c.Vary("User-Agent") // => Vary: Origin, User-Agent

  // No duplicates
  c.Vary("Origin") // => Vary: Origin, User-Agent

  c.Vary("Accept-Encoding", "Accept")
  // => Vary: Origin, User-Agent, Accept-Encoding, Accept
})
```
{% endcode %}

## Write

**어떤** 입력 값이든지 HTTP 파디 응답에 덧붙입니다.

{% code title="Signature" %}
```go
c.Write(body ...interface{})
```
{% endcode %}

{% code title="Example" %}
```go
app.Get("/", func(c *fiber.Ctx) {
  c.Write("Hello, ")         // => "Hello, "
  c.Write([]byte("World! ")) // => "Hello, World! "
  c.Write(123)               // => "Hello, World! 123"
})
```
{% endcode %}

## XHR

불리언 속성으로, 만약 요청의 [X-Requested-With](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) 헤더 필드가 [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)이면 `true`이고, 이는 요청이 클라이언트 라이브러리 \([jQuery](https://api.jquery.com/jQuery.ajax/)같은\)에 의해 생성되었음을 나타냅니다.

{% code title="Signature" %}
```go
c.XHR() bool
```
{% endcode %}

{% code title="Example" %}
```go
// X-Requested-With: XMLHttpRequest

app.Get("/", func(c *fiber.Ctx) {
  c.XHR() // true
})
```
{% endcode %}
