---
description: >-
  Questo benchmark mira a confrontare le prestazioni di Fiber e altri framework
  web.
---

# 🤖 Performance

## TechEmpower

🔗 [https://www.techempower.com/benchmarks/](https://www.techempower.com/benchmarks/#section=test&runid=c7152e8f-5b33-4ae7-9e89-630af44bc8de&hw=ph&test=plaintext)

* **CPU** Intel Xeon Gold 5120
* **MEM** 32GB
* **GO** go1.13.6 linux/amd64
* **OS** Linux
* **NET** Cisco 10-gigabit Ethernet switch dedicato.

### Testo in chiaro

**Fibre** ha gestito **6.162.556** risposte al secondo con una latenza media di **2.0** ms.  
**Express** ha gestito **367.069** risposte al secondo con una latenza media di **354.1** ms.

![](.gitbook/assets/plaintext%20%281%29.png)

![Fiber vs Express](.gitbook/assets/plaintext_express.png)

### Aggiornamento dati

**Fiber** ha gestito **11,846** risposte al secondo con una latenza media di **42.8** ms.  
**Express** ha gestito **2,066** risposte al secondo con una latenza media di **390.44** ms.

![](.gitbook/assets/data_updates.png)

![Fiber vs Express](.gitbook/assets/data_updates_express%20%281%29.png)

### Query multiple

**Fiber** ha gestito **19,664** risposte al secondo con una latenza media di **25.7** ms.  
**Express** ha gestito **4,302** risposte al secondo con una latenza media di **117.2** ms.

![](.gitbook/assets/multiple_queries%20%281%29.png)

![Fiber vs Express](.gitbook/assets/multiple_queries_express.png)

### Query singola

**Fiber** ha gestito **368,647** risposte al secondo con una latenza media di **0.7** ms.  
**Express** ha gestito **57,880** risposte al secondo con una latenza media di **4.4** ms.

![](.gitbook/assets/single_query%20%282%29.png)

![Fiber vs Express](.gitbook/assets/single_query_express.png)

### Serializzazione JSON

**Fiber** ha gestito **1,146,667** risposte al secondo con una latenza media di **0.4** ms.  
**Express** ha gestito **244,847** risposte al secondo con una latenza media di **1.1** ms.

![](.gitbook/assets/json%20%281%29.png)

![Fiber vs Express](.gitbook/assets/json_express.png)

## Go web framework benchmark

🔗 [https://github.com/smallnest/go-web-framework-benchmark](https://github.com/smallnest/go-web-framework-benchmark)

* **CPU** Intel\(R\) Xeon\(R\) Gold 6140 CPU @ 2.30GHz
* **MEM** 4GB
* **GO** go1.13.6 linux/amd64
* **OS** Linux

Il primo test è quello di simulare **0 ms**, **10 ms**, **100 ms**, **500 ms** come tempo di elaborazione nei gestori.

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/benchmark.png)

La simultaneità dei clients è di **5000**.

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/benchmark_latency.png)

La latenza è il tempo effettivo di processing del server web. _Più piccolo è meglio è._

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/benchmark_alloc.png)

Gli allocazioni sono le allocazioni heap dei server web quando il test è in esecuzione. L'unità è MB. _Più piccolo è meglio è._

Se abilitiamo l'**http pipelining**, il risultato del test è come segue:

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/benchmark-pipeline.png)

Test della simultaneità in **30 ms** di tempo di elaborazione, il risultato del test per **100**, **1000**, **5000** clients è:

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/concurrency.png)

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/concurrency_latency.png)

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/concurrency_alloc.png)

Se abilitiamo l'**http pipelining**, il risultato del test è come segue:

![](https://raw.githubusercontent.com/gofiber/docs/master/.gitbook/assets/concurrency-pipeline.png)

Grafico delle dipendenze per `v1.9.0`

![](.gitbook/assets/graph.svg)

