# 05 — Hosts, procesos, servicios y Smartscape

## Tres capas que debes separar

| Capa | Qué es | Pregunta típica |
| --- | --- | --- |
| **Host** | Máquina o nodo (VM, bare metal, a veces el node K8s) | ¿Se quedó sin CPU, RAM, disco, red? |
| **Proceso / Process group** | El proceso OS (java, node, IIS) agrupado por tipo | ¿El proceso crashea, restart, GC loco? |
| **Servicio** | El *endpoint lógico* APM (web request, messaging consumer) | ¿Las peticiones de negocio fallan o van lentas? |

Un junior mezcla “el pod está en CrashLoop” con “el servicio tiene 2 % de errores”. Pueden coincidir o no.

## Hosts

En la ficha de un host mira:

- CPU, memoria, disco, red
- Lista de procesos
- Eventos (reinicios, OneAgent, problemas de disco)
- Relación con **Problems**

**Señales de saturación:** CPU sostenida alta, memory reclaim, disco al 100 %, time wait de red, load average disparado en Linux.

**Trampa:** CPU alta en un host compartido no prueba que *tu* servicio sea el culpable. Baja a **proceso** y a **servicio**.

## Process groups

Dynatrace agrupa procesos similares (mismo cluster de Tomcat, mismo deployment). La instrumentación (OneAgent) vive aquí.

Revisa:

- Tecnología detectada (Java, .NET, Node, …)
- Versión / release (si está disponible)
- Restarts frecuentes
- Deep monitoring on/off

Si el process group no tiene deep monitoring, **no esperes PurePaths ricos**.

## Servicios

El **servicio** es donde trabajas la mayor parte del tiempo en APM:

- **Throughput** (requests/min)
- **Failure rate** (errores)
- **Response time** (percentiles)
- **CPU / suciedad de GC** (según tecnología)
- **Dependencias** (llamadas a DB, HTTP, colas)

Tipos que verás:

- **Web request** (API HTTP)
- **Database** (el servicio “lado cliente” de SQL)
- **Messaging** (Kafka, SQS, Rabbit, etc.)
- **Queue listeners / workers**

El nombre del servicio a veces es feo (`www-ssl / my-app / PROCESS`). Aprende el **nombre canónico** con tu equipo.

## Smartscape — cómo leerlo

Abre Smartscape desde un servicio o desde infraestructura.

- **Cajas:** entidades
- **Líneas:** llamadas observadas en la ventana de tiempo

Uso junior:

1. Parte de *tu* servicio.
2. Mira **downstream**: DB, HTTP, cache.
3. Mira **upstream**: quién te llama (API gateway, otro microservicio, RUM).

Si Smartscape está vacío, suele ser ventana de tiempo sin tráfico, MZ, o falta de instrumentación.

## De un incidente de infra a impacto de servicio

Ejemplo: disco al 100 % en un host.

1. Host → procesos que escriben logs/tmp.
2. Servicios en ese host: ¿sube failure rate o latency?
3. Problem de Davis: ¿ya lo agrupó?

Tu comentario en el canal:

> Host `app-prod-03` disco 100 % desde 14:02. Servicio `checkout-api` p95 3.2 s (baseline 400 ms), failure rate 8 %. Problem `P-12345`. Siguiente: PurePath y logs de `checkout-api`.

Eso es un handover de junior **útil**. Un “el disco está lleno” no lo es.

## Kubernetes (preview; detalle en doc 13)

En K8s la analogía suele ser:

- Node ≈ host
- Pod/container ≈ proceso
- Deployment/workload + service mesh/ingress ≈ cómo aparece el **servicio** APM

Los nombres de pod rotan. Filtra por **workload**, **namespace** y tags, no por un pod de hace 3 horas que ya no existe.
