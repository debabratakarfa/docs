---
description: >-
  В этом разделе вы можете посмотреть сравнение производительности Fiber и
  других популярных веб фреймворков, написанных на Go.
---

# 🤖 Бенчмарки

## TechEmpower

🔗 [https://www.techempower.com/benchmarks/](https://www.techempower.com/benchmarks/)

* **CPU** Intel Xeon Gold 5120
* **МЕМ** 32GB
* **GO** go1.13.6 linux/amd64
* **OS** Linux
* **NET** Dedicated Cisco 10-gigabit Ethernet switch

Чтобы просмотреть результаты для всех фреймворков, посетите страницу «[Plaintext All Results](https://www.techempower.com/benchmarks/#section=test&runid=350f0783-cc9b-4259-9831-28987799782a&hw=ph&test=plaintext)». Чтобы просмотреть результаты только фрейворков на Go, посетите «[Plaintext Go Results](https://www.techempower.com/benchmarks/#section=test&runid=350f0783-cc9b-4259-9831-28987799782a&hw=ph&test=plaintext&l=zijocf-1r)».

### Plaintext

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/techempower-plaintext.png)

### Plaintext latency

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/techempower-plaintext-latency.png)

### JSON serialization

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/techempower-json.png)

### Single query

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/techempower-single-query.png)

### Multiple queries

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/techempower-multiple-queries.png)

### Data updates

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/techempower-updates.png)

## Go Web Framework Benchmark

🔗 [https://github.com/smallnest/go-web-framework-benchmark](https://github.com/smallnest/go-web-framework-benchmark)

* **CPU** Intel\(R\) Xeon\(R\) Gold 6140 CPU @ 2.30GHz
* **МЕМ** 4GB
* **GO** go1.13.6 linux/amd64
* **OS** Linux

Первый тест — это имитация времени обработки в _handlers_ **0 мс**, **10 мс**, **100 мс** и **500 мс**.

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/benchmark.png)

Число клиентов параллелизма составляет **5000**.

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/benchmark_latency.png)

Задержка — это время реального времени обработки веб-серверами. _Чем меньше, тем лучше._

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/benchmark_alloc.png)

Allocations — это распределение _heap_ веб-серверами во время выполнения теста. Единица измерения МБ. _Чем меньше, тем лучше._

Если мы включим **http pipeline**, то результат теста будет следующим:

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/benchmark-pipeline.png)

Тест параллелизма за **30 мс** времени обработки, результат теста для **100**, **1000** и **5000** клиентов:

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/concurrency.png)

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/concurrency_latency.png)

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/concurrency_alloc.png)

Если мы включим **http pipeline**, то результат теста будет следующим:

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/concurrency-pipeline.png)

