# 08 — Métricas

## Métricas built-in vs custom

- **Built-in:** OneAgent y cloud las generan (CPU, response time del servicio, pods, etc.). Empieza siempre aquí.
- **Custom:** la app emite (Micrometer, StatsD, OTel metrics, ingest API). Hace falta saber el **nombre** y las **dimensiones**.

No crees métricas custom en el primer mes. Aprende a leer las que ya existen.

## Data Explorer (classic) vs DQL (Grail)

- **Data Explorer:** selector de métrica, split by dimensión, visualización. Ideal para explorar.
- **DQL:** `timeseries` / `fetch metric` en Notebooks. Ideal cuando Grail es el estándar del tenant.

El hábito es el mismo: **métrica + filtro de entidad + split + percentil o rate**.

## Métricas de servicio que importan

Para un servicio HTTP:

- Request count / throughput
- Failure rate
- Response time (median, p90, p95, p99 — alinea con el SLO)
- A veces: CPU time por request, wait time

Para infra:

- CPU %, memoria, disco, red
- En Java: GC time, heap, threads
- En K8s: CPU/memory vs request/limit, restarts

## Percentiles, no solo el promedio

El **promedio** esconde a los usuarios lentos. Un p95 de 3 s con promedio de 200 ms significa que el 5 % sufre. Los SLO suelen hablar de percentiles o de “fracción de requests buenas”.

## Tasas vs contadores

- Un **contador** que solo sube no se grafica crudo: se usa **rate** (por segundo/minuto).
- Failure **rate** es errores / total, no el número absoluto (10 errores en 10 requests es peor que 10 en 1 millón).

## Baseline y anomalías

Dynatrace aprende **baselines** (estacionalidad). Un pico de tráfico de Black Friday puede ser “normal” para Davis o no, según historia. Tú igual miras **deploy markers** (si hay integración con CI/CD o eventos).

Si hay un release a las 14:01 y el p95 se rompe a las 14:03, dilo. No es prueba legal, es la hipótesis #1.

## Custom charts en un dashboard

Un gráfico útil tiene:

- Título que dice el SLI (`checkout-api p95`)
- Unidad
- Filtro de Management Zone / tags
- Comparación opcional (semana previa)

Un gráfico inútil: 30 líneas sin leyenda, CPU de 200 hosts en un solo panel.

## Cuando la métrica “no cuadra” con el usuario

RUM mide **navegador** (incluye red del usuario, terceros, frontend). El servicio mide **servidor**. Pueden divergir:

- Front lento, API rápida → mira RUM / sintéticos / CDN
- API lenta, RUM “ok” → pocos usuarios en ese flujo o muestreo

No discutas con el usuario: muestra **ambas** señales.
