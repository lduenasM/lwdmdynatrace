# 07 — Logs en Dynatrace

## Rol de los logs

Los logs responden: **qué excepción**, **qué mensaje de negocio**, **qué id de entidad**. Las métricas no sustituyen un stacktrace. Las trazas no siempre capturan el mensaje de dominio.

## Dónde buscar (dos mundos)

1. **Log viewer clásico / Log monitoring:** filtros por host, proceso, archivo, texto.
2. **Grail + DQL:** `fetch logs` y pipelines (ver [14-dql-grail.md](14-dql-grail.md)).

Pregunta a tu buddy cuál es el camino oficial del tenant.

## Buena búsqueda (siempre)

1. **Tiempo** acotado al incidente.
2. **Entidad:** servicio, host, Kubernetes namespace/workload, o log source.
3. **Severidad:** ERROR / FATAL primero; luego WARN.
4. **Texto o campo JSON:** `timeout`, `NullPointer`, código de error interno.
5. Si existe: **trace id**.

Evita buscar una palabra de 3 letras en “last 7 days” sobre todos los hosts.

## Correlación OneAgent

OneAgent puede **enriquecer** logs con:

- `dt.entity.service`, host, process
- `trace_id` / `span_id` cuando el log sale del hilo instrumentado

Eso permite ir de Problem → servicio → log y de log → PurePath.

Si los logs “no tienen contexto”, no es culpa tuya: es configuración de ingest y del logger de la app (`Mdc`, `Activity`, OpenTelemetry context).

## Volumen, costo y ruido

Logs son **caros**. Malas prácticas:

- `DEBUG` en producción para todos los requests
- Loguear bodies con PII (documentos, tarjetas, tokens)
- El mismo error 10 000 veces por minuto sin sampling

Si ves un “log storm”, menciónalo: puede tumbar ingest y ocultar la señal. Escala; no intentes “arreglar el logger” en prod tú solo.

## Logs vs Problems

Un millón de líneas ERROR no equivalen a un Problem. Davis agrupa **impacto en entidades**. Tú usas logs para **confirmar** la hipótesis del PurePath.

Orden recomendado: Problem/métricas → traza → log, no al revés (salvo que solo tengas un `errorId` del usuario).

## Retención

La retención la define plataforma (días o más en Grail). Si el incidente es de hace 3 meses, puede que **ya no esté**. Dilas en el ticket: “fuera de retención”.

## Checklist junior de logs

- [ ] Ventana de tiempo correcta y zona horaria
- [ ] Filtro de servicio/host/namespace
- [ ] Un ejemplo de línea ERROR completa (sin PII)
- [ ] Trace id si aparece
- [ ] ¿El error es de *nuestra* app o de health checks / bots?
