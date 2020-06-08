---
description: L'istanza dell'app convenzionalmente denota l'applicazione Fiber.
---

# 🚀 Applicazione

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

Puoi passare le impostazioni dell'applicazione quando chiami `New`.

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

O modifica le impostazioni dopo aver inizializzato un `app`.

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

**Impostazioni** **campi**

| Proprietà                 | Tipo            | Descrizione                                                                                                                                                                                                                                                          | Predefinito       |
|:------------------------- |:--------------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |:----------------- |
| Prefork                   | `bool`          | Abilita l'uso dell'opzione di socket [`SO_REUSEPORT`](https://lwn.net/Articles/542629/). Questo genererà processi Go multipli in ascolto sulla stessa porta. scopri di più sullo [socket sharding](https://www.nginx.com/blog/socket-sharding-nginx-release-1-9-1/). | `false`           |
| ServerHeader              | `string`        | Abilita l'intestazione HTTP del `Server` con il valore dato.                                                                                                                                                                                                         | `""`              |
| StrictRouting             | `bool`          | Quando abilitato, il router tratta `/foo` e `/foo/` come differenti. Altrimenti, il router tratta `/foo` e `/foo/` come uguali.                                                                                                                                      | `false`           |
| CaseSensitive             | `bool`          | Quando abilitato, `/Foo` e `/foo` come percorsi differenti. Quando disabilitato, `/Foo` e `/foo` sono trattati come uguali.                                                                                                                                          | `false`           |
| Immutabile                | `bool`          | Quando abilitato, tutti i valori restituiti dai metodi di contesto sono immutabili. Di default sono validi finché torni dal gestore, vedi problema [\#185](https://github.com/gofiber/fiber/issues/185).                                                           | `false`           |
| BodyLimit                 | `int`           | Imposta la dimensione massima consentita per un corpo di richiesta, se la dimensione eccede il limite configurato, invia la risposta `413 - Request Entity Too Large`.                                                                                               | `4 * 1024 * 1024` |
| CompressedFileSuffix      | `string`        | Adds suffix to the original file name and tries saving the resulting compressed file under the new file name.                                                                                                                                                        | `".fiber.gz"`     |
| Concorrenza               | `int`           | Numero massimo di connessioni concorrenti.                                                                                                                                                                                                                           | `256 * 1024`      |
| DisableKeepalive          | `bool`          | Disabilita le connessioni mantenute vive, il server chiuderà le connessioni in arrivo dopo aver inviato la prima risposta al client                                                                                                                                  | `false`           |
| DisableDefaultDate        | `bool`          | Quando impostato in true causa all'intestazione della data predefinita di essere esclusa dalla risposta.                                                                                                                                                             | `false`           |
| DisableDefaultContentType | `bool`          | Quando impostato a true, causa all'intestazione Tipo-Contenuto predefinita di essere esclusa dal Responso.                                                                                                                                                           | `false`           |
| DisableStartupMessage     | `bool`          | Quando impostato a true, non stamperà il fiber ASCII ed sarà "in ascolto" sul messaggio                                                                                                                                                                              | `false`           |
| DisableHeaderNormalizing  | `bool`          | By default all header names are normalized: conteNT-tYPE -&gt; Content-Type                                                                                                                                                                                    | `false`           |
| ETag                      | `bool`          | Abilita o disabilita la generazione di intestazione ETag, poiché sia gli etag deboli che quelli forti sono generati usando lo stesso metodo di hashing \(CRC-32\). Gli ETag deboli sono predefiniti quando abilitati.                                              | `false`           |
| Templates                 | `Templates`     | Templates is the interface that wraps the Render function. See our [**Template Middleware**](middleware.md#template) for supported engines.                                                                                                                          | `nil`             |
| ReadTimeout               | `time.Duration` | La quantità di tempo consentita per leggere la richiesta completa incluso il corpo. Timeout predefinito illimitato.                                                                                                                                                  | `nil`             |
| WriteTimeout              | `time.Duration` | La durata massima prima che il timeout scriva il responso. Timeout predefinito illimitato.                                                                                                                                                                           | `nil`             |
| IdleTimeout               | `time.Duration` | La quantità massima di tempo di attessa per la prossima richiesta quando mantieni vivo è abilitato. Se IdleTimeout è a zero, il valore di ReadTimeout viene usato.                                                                                                   | `nil`             |

## Static

Usa il metodo **Statico** per servire file statici come **immagini**, **CSS** e **JavaScript**.

{% hint style="info" %}
By default, **Static** will serve `index.html` files in response to a request on a directory.
{% endhint %}

{% code title="Signature" %}
```go
app.Static(prefix, root string, config ...Static) // => with prefix
```
{% endcode %}

Usa il codice seguente per servire file in una directory denominata `./public`

{% code title="Example" %}
```go
app.Static("/", "./public")

// => http://localhost:3000/hello.html
// => http://localhost:3000/js/jquery.js
// => http://localhost:3000/css/style.css
```
{% endcode %}

Per servire le directory multiple, puoi usare **Static** multiple volte.

{% code title="Example" %}
```go
// Serve files from "./public" directory:
app.Static("/", "./public")

// Serve files from "./files" directory:
app.Static("/", "./files")
```
{% endcode %}

{% hint style="info" %}
Usa una cache proxy inversa come [**NGINX**](https://www.nginx.com/resources/wiki/start/topics/examples/reverseproxycachingexample/) per migliorare la performance delle risorse statiche servite.
{% endhint %}

Puoi usare ogni prefisso virtuale di percorso \(_dove il percorso non esiste realmente nel sistema del file_\) per i file che sono serviti dal metodo **Static**, specifica un percorso del prefisso per la directory statica, come mostrato sotto:

{% code title="Example" %}
```go
app.Static("/static", "./public")

// => http://localhost:3000/static/hello.html
// => http://localhost:3000/static/js/jquery.js
// => http://localhost:3000/static/css/style.css
```
{% endcode %}

Se vuoi avere un po' di controllo in più riguardante le impostazioni per i file statici serviti. You could use the `fiber.Static` struct to enable specific settings.

{% code title="fiber.Static{}" %}
```go
// Static represents settings for serving static files
type Static struct {
    // Transparently compresses responses if set to true
    // This works differently than the github.com/gofiber/compression middleware
    // The server tries minimizing CPU usage by caching compressed files.
    // Aggiunge il suffisso ".fiber.gz" al nome originale del file.
    // Opzionale. Valore predefinito false
    Booleano compresso
    // Abilita le richieste del range di byte se impostato a true.
    // Opzionale. Valore predefinito false
    ByteRange booleano
    // Abilita la navigazione della directory.
    // Opzionale. Valore predefinito false.
    Naviga booleano
    // File indice per servire una directory.
    // Opzionale. Valore predefinito "index.html".
    Stringa indice
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

Instrada una richiesta HTTP, dove **METHOD** è il [metodo HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) della richiesta.

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

Puoi raggruppare i percorsi creando una struttura `*Group`.

**Signature**

```go
app.Group(prefix string, handlers ...func(*Ctx)) *Group
```

**Esempio**

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

## Listen

Associa ed ascolta le connessioni sull'indirizzo specifico. Questo può essere un `int` per la porta o `string` per l'indirizzo.

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

Per abilitare **TLS/HTTPS** puoi mettere in attesa un [**TLS config**](https://golang.org/pkg/crypto/tls/#Config).

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

Il test della tua applicazione è fatto col metodo **Test**. Usa questo metodo per creare i file `_test.go` o quando necessiti di fare il debug la tua logica di routing. Il timeout predefinito è `200ms` se vuoi disabilitare un timeout completamente, passa `-1` come secondo argomento.

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

