# 17 — Detección de anomalías, Metric Events y perfiles (Avanzado)

## Objetivo

Que Davis y las notificaciones **representen impacto**, no el latido de cada CPU.

## Capas (repaso con control)

1. **Anomalías automáticas** (Davis sobre métricas de servicio/infra)
2. **Metric events / eventos custom** (tú defines umbral, consulta DQL, o static)
3. **Alerting profiles** (quién se entera, delay, filtros de tag/severidad)
4. **Silences / maintenance** (excepciones temporales)

El Avanzado diseña 2 y propone 3. El Senior ([24](24-gobierno-iam-mz-tags.md)) pone el estándar de tags para que 3 funcione.

## Diseño de un metric event que no da vergüenza

Plantilla:

- **Nombre:** `prod-checkout-5xx-burn` (env + journey + síntoma)
- **Condición:** tasa, no contador crudo; ventana (p. ej. 5 min)
- **Ámbito:** tag `app:checkout` + `env:prod`, no “todos los servicios”
- **Por qué existe:** el automatic Davis no cubre este contrato (o es más estricto que el SLO)
- **Dueño y runbook:** enlace
- **Revisión:** fecha; si no disparó en 90 días, ¿sigue haciendo falta?

## Umbral vs baseline vs SLO burn

| Tipo | Úsalo cuando |
| --- | --- |
| Estático | Contrato duro (sintético 2 fallos seguidos; disco 95 %) |
| Baseline / Davis | Tráfico estacional, el “normal” cambia |
| Burn rate de SLO | Quieres priorizar **presupuesto de error**, no un pico de 1 minuto |

Tres alertas para lo mismo = fatiga. Elige **una** señal primaria por journey.

## Event ingest (custom)

Apps pueden mandar eventos de deploy, feature flag, “inicio de job”. Davis los usa como **contexto**. Un Avanzado conecta CI (`version`, `commit`) para que el Problem muestre el release. Sin PII.

## Qué no hacer

- CPU > 80 % en todos los hosts
- Alertar `error log contains Exception` en DEBUG
- Profiles que envían *any problem* a un único buzón
- Maintenance window eterno “hasta que bajemos el ruido”

## Proceso de cambio

1. Hipótesis en no-prod o con delay alto
2. Semana de observación (¿falsos positivos?)
3. PR as-code si el equipo ya está en Monaco/Terraform ([25](25-observability-as-code.md))
4. Comunicación al canal de on-call

## Criterio Avanzado

Puedes **apagar o no crear** una alerta y defenderlo con impacto de usuario, no con “me molestaba el Slack”.
