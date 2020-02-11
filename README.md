---
description: >-
  Документация в этом разделе поможет вам быстрее начать создавать
  веб-приложения на Fiber.
---

# 📖 Начало работы

[![](https://img.shields.io/github/release/gofiber/fiber?style=flat-square)](https://github.com/gofiber/fiber/releases) [![](https://img.shields.io/badge/api-documentation-blue?style=flat-square)](https://fiber.wiki) ![](https://img.shields.io/badge/goreport-A%2B-brightgreen?style=flat-square) [![GitHub license](https://img.shields.io/badge/coverage-91%25-brightgreen?style=flat-square)](https://gocover.io/github.com/gofiber/fiber) [![Join the chat at https://gitter.im/gofiber/community](https://img.shields.io/travis/gofiber/fiber/master.svg?label=linux&style=flat-square)](https://travis-ci.org/gofiber/fiber) [![](https://img.shields.io/travis/gofiber/fiber/master.svg?label=windows&style=flat-square)](https://travis-ci.org/gofiber/fiber)

**Fiber** — это **веб фреймворк**, который был вдохновлен [Express](https://github.com/expressjs/express) и основан на [Fasthttp](https://github.com/valyala/fasthttp), самом **быстром** HTTP-движке написанном на [Go](https://golang.org/doc/). Фреймворк был разработан с целью **упростить** процесс **быстрой** разработки **высокопроизводительных** веб-приложений с **нулевым распределением памяти**.

## Установка

Прежде всего, [скачайте](https://golang.org/dl/) и установите Go.

{% hint style="success" %}
Go **1.11** \(с включенными [модулями Go](https://golang.org/doc/go1.11#modules) \) или выше.
{% endhint %}

Установка выполняется с помощью команды [`go get`](https://golang.org/cmd/go/#hdr-Add_dependencies_to_current_module_and_install_them) :

```bash
go get -u github.com/gofiber/fiber
```

## Hello, World!

Ниже приведено простейшее приложение **Fiber**, которое вы можете создать.

```text
touch server.go
```

```go
package main

import "github.com/gofiber/fiber"

func main() {
  // Создание нового экземплара Fiber:
  app := fiber.New()

  // Создание маршрута для корневого адреса, "/":
  app.Get("/", func(c *fiber.Ctx) {
    c.Send("Hello, World!")
    // => "Hello, World!"
  })

  // Старт сервера на "localhost" с портом "8080":
  app.Listen(8080)
}
```

```text
go run server.go
```

После перехода по адресу `http://localhost:8080`, вы должны увидеть надпись `Hello, World!` на странице.

## Базовая маршрутизация

Маршрутизация \(_routing_\) относится к определению того, как приложение отвечает на запрос клиента к конкретной конечной точке, которая является URI \(или путем\) и конкретным методом HTTP-запроса \(GET, PUT, POST и прочие\).

{% hint style="info" %}
Каждый маршрут \(_route_\) может иметь **одну функцию-обработчик** \(_handler_\), которая выполняется при сопоставлении маршрута.
{% endhint %}

Определение маршрута имеет следующие сигнатуры:

```go
app.Method(func(*fiber.Ctx))              // без указания пути
app.Method(path string, func(*fiber.Ctx)) // с использованием пути
```

* `app` — экземпляр **Fiber**.
* `Method` — [метод HTTP-запроса](https://fiber.wiki/application#methods) , с заглавной буквы: `Get` , `Put` , `Post` и прочие.
* `path` — путь на сервере.
* `func(*fiber.Ctx)` — функция обратного вызова \(_callback_\), содержащая [контекст,](https://fiber.wiki/context) выполняемый при сопоставлении маршрута.

### Простой маршрут

```go
// Возвращает "Hello, World!" при запросе на корневой адрес, "/":
app.Get("/", func(c *fiber.Ctx) {
  c.Send("Hello, World!")
})
```

### Маршрут с параметром

```go
// GET http://localhost:8080/hello%20world

app.Get("/:value", func(c *fiber.Ctx) {
  c.Send("Get request with value: " + c.Params("value"))
  // => Get request with value: hello world
})
```

### Маршрут с необязательным параметром

```go
// GET http://localhost:8080/hello%20world

app.Get("/:value?", func(c *fiber.Ctx) {
  if c.Params("value") != "" {
    c.Send("Get request with value: " + c.Params("Value"))
    // => Get request with value: hello world
    return
  }

  c.Send("Get request without value")
})
```

### Маршрут с wildcard

```go
// GET http://localhost:8080/api/user/john

app.Get("/api/*", func(c *fiber.Ctx) {
  c.Send("API path with wildcard: " + c.Params("*"))
  // => API path with wildcard: user/john
})
```

## Статические файлы

Чтобы ваше веб-приложение могло получить доступ к статическим файлам, таким как **изображения**, файлы **CSS** и **JavaScript**, воспользуйтесь методом **Static**.

Сигнатура функции:

```go
app.Static(root string)         // => без префикса
app.Static(prefix, root string) // => с использованием префикса
```

Используйте следующий код для обслуживания файлов в каталоге с именем `./public` :

```go
app := fiber.New()

app.Static("./public") // => Доступны все файлы в ./public

app.Listen(8080)
```

Теперь вы можете получить доступ к файлам, которые находятся в каталоге `./public` :

```bash
http://localhost:8080/hello.html
http://localhost:8080/js/jquery.js
http://localhost:8080/css/style.css
```

