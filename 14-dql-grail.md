# 14 — DQL y Grail (introducción Intermedio)

## Qué es Grail

**Grail** es el data lakehouse de Dynatrace: logs, métricas, eventos, spans (según licencia y configuración) consultables con **DQL** (Dynatrace Query Language).

Si tu tenant aún es “solo classic”, igual lee esto: es el destino de la UI nueva.

## Dónde se escribe DQL

- **Notebooks**
- **Dashboards** (tiles DQL)
- **Security / Apps** varias
- A veces **Workflows**

Guarda consultas útiles en un notebook del **equipo**, no en 15 copias personales.

## Forma mental de una consulta

```
fetch <fuente>
| filter <recorte agresivo: tiempo ya lo da el picker, más servicio/env>
| ... transformar ...
| summarize / timeseries / fields
```

El pecado capital: `fetch logs` sin filtro sobre 15 días. Cuesta dinero y tiempo.

## Ejemplos mínimos (adapta nombres de campos a tu tenant)

Logs con error en un servicio (campos reales varían; usa el schema explorer):

```dql
fetch logs
| filter matchesPhrase(content, "TimeoutException")
| filter dt.entity.service == "SERVICE-XXXXXXXXXXXXXXXX"
| sort timestamp desc
| limit 50
```

Métrica en el tiempo (patrón típico):

```dql
timeseries avg(dt.service.request.response_time),
  by: { dt.entity.service }
| filter dt.entity.service == "SERVICE-XXXXXXXXXXXXXXXX"
```

**No copies IDs de este documento:** sácalos de la UI (entity selector). Los nombres de métricas built-in evolucionan; el Data Explorer / métrica catalog es la fuente.

## Hábitos Intermedio

1. Recorta **primero** (servicio, k8s.namespace, status)
2. `limit` mientras exploras
3. No proyectes PII (`user.email`, tokens) en notebooks compartidos
4. Comenta en el notebook **qué pregunta responde**
5. Si la consulta tarda: más filtro, menos `parse` pesado, ventana más corta

## De Intermedio a Avanzado

Cuando uses `lookup`, joins, `makeTimeseries`, dashboards parametrizados y runbooks DQL, pasa a [18](18-dql-avanzado-notebooks.md).

## Criterio de salida Intermedio

Eres capaz de **encontrar logs de un incidente** con DQL o el viewer, y de **graficar un SLI simple**. No se espera que seas ingeniero de Grail.
