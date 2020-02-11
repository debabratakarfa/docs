---
description: Экземпляр приложения условно обозначает приложение Fiber.
---

# 🚀 Применение

## New

Метод создает новый экземпляр с именем **Fiber**.

```go
app := fiber.New()
```

## Static

Получите доступ к статическим файлам, таким как **изображения**, файлы **CSS** и **JavaScript**, воспользовавшись методом **Static**.

{% hint style="info" %}
По умолчанию этот метод отправляет файлы `index.html` в ответ на запрос к каталогу.
{% endhint %}

#### Сигнатура

```go
app.Static(root string)         // => без префикса
app.Static(prefix, root string) // => с использованием префикса
```

#### Примеры

Используйте следующий код для получения доступа ко всем файлам в каталоге с именем `./public`:

```go
app.Static("./public")

// => http://localhost:3000/hello.html
// => http://localhost:3000/js/jquery.js
// => http://localhost:3000/css/style.css
```

Для доступа к нескольким каталогам, вы можете использовать метод **Static** сколько угодно раз.

```go
// Доступ к файлам в директории "./public":
app.Static("./public")

// Доступ к файлам в директории "./files":
app.Static("./files")
```

{% hint style="info" %}
Используйте кеш обратного прокси-сервера, например [NGINX,](https://www.nginx.com/resources/wiki/start/topics/examples/reverseproxycachingexample/) для повышения производительности обработки статических файлов.
{% endhint %}

Чтобы создать префикс виртуального пути \(_где путь фактически не существует в файловой системе_\) для файлов обслуживаемых методом **Static**, укажите префикс для каталога, как показано ниже:

```go
app.Static("/static", "./public")

// => http://localhost:3000/static/hello.html
// => http://localhost:3000/static/js/jquery.js
// => http://localhost:3000/static/css/style.css
```

## Methods

Маршрутизация HTTP-запроса, где **METHOD** — это [HTTP-метод](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) запроса.

#### Сигнатура

```go
app.METHOD(handler func(*Ctx))              // соответствует любому пути
app.METHOD(path string, handler func(*Ctx)) // только заданный путь
```

#### Пример

```go
// Единичный метод
app.Connect(...)
app.Delete(...)
app.Get(...)
app.Head(...)
app.Options(...)
app.Patch(...)
app.Post(...)
app.Put(...)
app.Trace(...)

// Соответствует всем методам и полному пути
app.All(...)

// Сопоставляет все методы и URL-адреса, начинающиеся с указанного пути
app.Use(...)
```

## Listen

Связывает и прослушивает все соединения по указанному адресу. Это может быть `int` для порта или `string` для адреса.

#### Сигнатура

```go
app.Listen(address interface{}, tls ...string)
```

#### Пример

```go
app.Listen(8080)
app.Listen("8080")
app.Listen(":8080")
app.Listen("127.0.0.1:8080")
```

Чтобы включить **TLS/HTTPS**, вы можете добавить свой **сертификат** и путь к **ключу**.

```go
app.Listen(443, "server.crt", "server.key")
```

## Настройки

### Engine

Вы можете изменить любые настройки [Fasthttp](https://github.com/valyala/fasthttp/blob/master/server.go#L150) по умолчанию через экземпляр **Fiber**. Эти настройки должны быть установлены **до** метода [Listen](application.md#listen).

{% hint style="danger" %}
Изменяйте эти настройки, только если вы **точно** знаете, что делаете.
{% endhint %}

```go
app.Engine.Concurrency = 256 * 1024
app.Engine.DisableKeepAlive = false
app.Engine.ReadBufferSize = 4096
app.Engine.WriteBufferSize = 4096
app.Engine.ReadTimeout = 0
app.Engine.WriteTimeout = 0
app.Engine.IdleTimeout = 0
app.Engine.MaxConnsPerIP = 0
app.Engine.MaxRequestsPerConn = 0
app.Engine.TCPKeepalive = false
app.Engine.TCPKeepalivePeriod = 0
app.Engine.MaxRequestBodySize = 4 * 1024 * 1024
app.Engine.ReduceMemoryUsage = false
app.Engine.GetOnly = false
app.Engine.DisableHeaderNamesNormalizing = false
app.Engine.SleepWhenConcurrencyLimitsExceeded = 0
app.Engine.NoDefaultContentType = false
app.Engine.KeepHijackedConns = false
```

### Prefork

Параметр Prefork позволяет использовать параметр сокета [**SO\_REUSEPORT**](https://lwn.net/Articles/542629/), который доступен в более новых версиях многих операционных систем, включая **DragonFly BSD** и **Linux** \(версия ядра **3.9** и выше\). Это приведет к появлению нескольких процессов Go, прослушивающих один и тот же порт.

У **NGINX** есть отличная статья о [Socket Sharding](https://www.nginx.com/blog/socket-sharding-nginx-release-1-9-1/), эти фотографии взяты из нее.

![Schema, when Prefork disabled \(by default\)](https://cdn.wp.nginx.com/wp-content/uploads/2015/05/Slack-for-iOS-Upload-1-e1432652484191.png)

![Schema, when Prefork enabled](https://cdn.wp.nginx.com/wp-content/uploads/2015/05/Slack-for-iOS-Upload-e1432652376641.png)

Вы можете включить функцию Prefork, добавив флаг `-prefork`  при запуске приложения:

```bash
./server -prefork
```

Или установить для параметра `Prefork`  значение `true` :

```go
app.Prefork = true // Prefork включен

app.Get("/", func(c *fiber.Ctx) {
  msg := fmt.Sprintf("Worker #%v", os.Getpid())
  c.Send(msg)
  // => Worker #16858
  // => Worker #16877
  // => Worker #16895
})
```

### Server

**Fiber** по умолчанию не отправляет [заголовок сервера](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Server), но вы можете включить его.

```go
app.Server = "Windows 95" // => Server: Windows 95
```

### Banner

При запуске приложения **Fiber** консоль отображает баннер, содержащий версию пакета и порт прослушивания. _Этот баннер включен по умолчанию._

![](.gitbook/assets/screenshot-2020-02-08-at-13.18.27.png)

Чтобы отключить его отображение, установите для параметра `Banner`  значение `false` :

```go
app.Banner = false // Скрыть баннер
```

## Тестирование

Тестирование вашего приложения выполняется с помощью метода **Test**.

{% hint style="info" %}
Метод используется для `_test.go` файлов и отладки приложений.
{% endhint %}

#### Сигнатура

```go
app.Test(req *http.Request) (*http.Response, error)
```

#### Пример

```go
// Создайте корневой маршрут, принимающий GET запросы для теста:
app.Get("/", func(c *Ctx) {
  fmt.Println(c.BaseURL())              // => http://google.com
  fmt.Println(c.Get("X-Custom-Header")) // => hi

  c.Send("hello, World!")
})

// http.Request
req, _ := http.NewRequest("GET", "http://google.com", nil)
req.Header.Set("X-Custom-Header", "hi")

// http.Response
resp, _ := app.Test(req)

// Проверка и вывод:
if resp.StatusCode == 200 {
  body, _ := ioutil.ReadAll(resp.Body)
  fmt.Println(string(body)) // => Hello, World!
}
```

