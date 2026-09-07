# 18 — DQL avanzado, Notebooks y runbooks (Avanzado)

## De explorar a industrializar

Intermedio: `fetch` + `filter` + `limit`.  
Avanzado: consultas **parametrizadas**, reutilizables, con tiempo acotado, sin PII, versionadas (export o Git).

## Patrones que debes dominar

- **Recorte primero:** entidad, k8s.namespace, status, loglevel
- **`parse` / `fieldsAdd`:** extraer código de error **sin** el payload completo
- **`summarize`:** conteos por ruta, no por usuario
- **`timeseries` / `makeTimeseries`:** SLI en el tiempo
- **`lookup` / joins:** mapear id de servicio a nombre humano (con cuidado de tamaño)
- **Parámetros de notebook:** `env`, `service`, ventana — un runbook, muchos servicios

Los nombres de campos y métricas **cámbialos según el catalog de tu tenant**. Valida en docs y en el schema explorer.

## Notebook como runbook de incidente

Estructura recomendada (secciones):

1. Pregunta y dueño
2. Parámetros
3. ¿Hay Problem / eventos de deploy?
4. RED del servicio
5. Top rutas lentas o 5xx
6. Muestra de logs **redactada** (`fields` allowlist)
7. Enlace a Smartscape / siguiente equipo

Al terminar el incidente, el notebook se **limpia** (límites, no dumps). No es un archivo forense con datos personales.

## Rendimiento y costo de consulta

- Ventanas largas + `parse` de JSON enorme = factura y timeout
- Prefiere campos ya extraídos en ingest (pipeline) vs parse ad-hoc masivo
- `limit` en exploración; agrega en producción del dashboard

## Calidad

- Título que es una pregunta
- Comentario de **definición del SLI** (qué cuenta como “bueno”)
- Prohibido seleccionar `*` de logs en dashboards compartidos

## Puente

Consultas que viven en **Apps** y buckets: [26](26-grail-appengine.md). Copilot que **escribe DQL**: [31](31-uso-de-ia-en-observabilidad.md) — siempre **ejecutar y leer el plan**, nunca pegar a prod sin leer.
