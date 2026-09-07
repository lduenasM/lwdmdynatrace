# 06 — Trazas y PurePath

## Qué es un PurePath

Un **PurePath** es la traza de una transacción: desde el entry point (HTTP, mensaje, job) a través de métodos, llamadas salientes y base de datos, con **tiempos** en cada span.

Es la herramienta #1 para “está lento” y “da 500”.

## Cuándo abrir trazas (no métricas)

| Situación | Ve a PurePath |
| --- | --- |
| p95 alto | Ordena por **response time** |
| Failure rate alto | Filtra **failed** / HTTP 5xx |
| “A veces falla” | Busca el **trace id** que te pasó soporte o el log |
| Sospecha de N+1 | Cuenta queries SQL repetidas en el path |

Las métricas del servicio te dicen *que* hay un problema. El PurePath te dice *dónde*.

## Cómo leer un PurePath (método)

1. **Entry:** método HTTP, URL, status code, tiempo total.
2. **Árbol / waterfall:** qué hijo se lleva el tiempo (barra larga).
3. **Llamadas salientes:** HTTP a otros servicios, SQL, Redis, gRPC.
4. **Código / método:** en Java/.NET suele verse el método caliente (si hay code-level).
5. **Errores:** excepción, HTTP 4xx/5xx, timeout.

Pregunta guía: **¿el tiempo está en CPU, espera de red, espera de DB, o cola?**

- CPU / métodos de app → bug o algoritmo.
- SQL lento o muchas queries → índice, N+1, plan.
- HTTP externo lento → dependencia; abre el servicio hijo.
- Tiempo “hueco” → a veces lock, thread pool, o I/O no instrumentado.

## Filtros útiles

- Servicio + **failed**
- Response time **> 2s** (ajusta al SLO)
- Endpoint concreto (`/api/v1/orders`)
- Atributos: `http.status_class`, user-agent (RUM), etc. según lo que esté capturado

No busques “todas las trazas del mes” sin filtro: la UI se vuelve lenta y tú también.

## Correlación con logs

En un setup bueno, el log lleva `trace_id` / `dt.trace_id` / equivalente. Desde el PurePath hay enlace a **logs**.

Si no hay correlación:

- Aun así usa timestamp + servicio + mensaje de excepción
- Escala a plataforma para activar enriquecimiento de logs (OneAgent log enrichment)

## Sampling y “no encuentro mi traza”

En picos, no siempre se guarda el 100 % de las trazas. Si el usuario te da hora exacta y no aparece:

- Amplía 1–2 minutos
- Filtra el endpoint
- Busca por error en logs y luego sube a traza
- Pregunta si hay captura **siempre** en errores (muchos tenants priorizan failed)

## OpenTelemetry vs OneAgent

Si la traza viene de OTel, el aspecto puede ser el de **Distributed tracing** en Grail en lugar del PurePath clásico. El método de lectura es el mismo: **timeline, spans, errores, atributos**.

## Mini plantilla para el ticket

```
Servicio: checkout-api
Ventana: 2026-09-02 14:00–14:20 (hora Lima)
Síntoma: p95 3.1s, 5xx 7%
PurePath ejemplo: <URL>
Span dominante: SQL SELECT ... /payments (2.8s)
Error: TimeoutException / HTTP 504 hacia billing-svc
```

No pegues SQL con datos de clientes. Generaliza el tipo de query.
