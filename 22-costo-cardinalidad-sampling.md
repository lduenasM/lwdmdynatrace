# 22 — Costo, cardinalidad y sampling (Avanzado)

## La observabilidad tiene factura (y techo técnico)

Ingest de logs, métricas de alta cardinalidad y 100 % de trazas en un Black Friday pueden:

- Costar más que el incidente que querías ver
- Degradar consultas y la UI
- Incentivar “apagar todo” (peor)

Un Avanzado trata el volumen como **requisito de diseño**, no como sorpresa de fin de mes.

## Cardinalidad

Cardinalidad = cuántas series únicas (métrica × dimensiones).

Explosivo:

- `userId`, `email`, `sessionId` como dimensión de métrica
- Path HTTP con UUID (`/orders/550e8400-...`)
- Label de K8s que cambia por build (`commit` en cada replica como dimensión de métrica de infra)

Estable:

- `http.route` plantilla (`/orders/{id}`)
- `env`, `app`, `status_class`, `k8s.cluster.name`

Si un gráfico tiene miles de series, **está mal el modelo**, no “falta zoom”.

## Sampling de trazas

- Head sampling: decisión al inicio (barato, puedes perder el error raro)
- Tail / error-centric: guardar más los failed (útil; aún así hay límites)
- 100 % en no-prod de una app chica; **no** copiar eso a prod masivo

Documenta el % y si los errores se priorizan. “No encuentro la traza del usuario” a veces es sampling, no un bug de Dynatrace.

## Logs

- Niveles por entorno (DEBUG no es de prod global)
- Drop de health checks y access logs ruidosos en ingest
- Extracción de campos en pipeline vs guardar línea cruda eterna
- Retención corta para DEBUG, más larga para audit **si** compliance lo pide (eso es legal, no “por si acaso”)

## Cómo decidir qué no ingerir

Pregunta: **¿esta señal cambia una decisión en incidente o un SLO?** Si no, no entra. El Senior respalda esa lista políticamente ([29](29-coe-enablement-programa.md)).

## Criterio Avanzado

Puedes estimar (orden de magnitud) qué pasaría al añadir una dimensión o un log por request, y propones un **techo** antes de implementar.
