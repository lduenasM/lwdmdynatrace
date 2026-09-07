# 11 — Alertas, alerting profiles y SLO (lectura Intermedio)

Este documento es de **lectura y uso**. El **diseño** de SLO y de detección está en [21](21-diseno-sli-slo-error-budget.md) y [17](17-deteccion-anomalias-y-perfiles.md).

## Tres capas que la gente mezcla

| Capa | Pregunta | Ejemplo |
| --- | --- | --- |
| **Detección** | ¿Qué evento existe? | Anomalía de failure rate; metric event `5xx > 2%` |
| **Problem** | ¿Davis lo agrupa y le pone impacto? | P-123 sobre checkout-api |
| **Notificación** | ¿A quién le llega y cuándo? | Alerting profile → Slack `#pagos-prod` |

Puedes tener detección perfecta y notificación rota (o al revés). En un “no nos enteramos”, investiga **las tres**.

## Alerting profiles (idea)

Un **alerting profile** filtra Problems (severidad, evento, tags, delay, días/horas) y se conecta a un **integration** (webhook, email…).

Intermedio debe saber:

- Por qué *tú* no recibes un Problem: profile, tag de entidad, MZ, silencio, integración caída
- Por qué recibes ruido: profile demasiado amplio (`any problem` en prod)

No edites profiles de prod. Documenta el síntoma (“nos llegan 40 CPU hosts”) para Avanzado/Master.

## SLO en Dynatrace (cómo leerlos)

Un SLO típico une:

- **SLI:** métrica o DQL (fracción de requests buenas)
- **Objetivo:** 99,5 %
- **Ventana:** 7 / 30 días rolling, o calendario

Lo que miras en incidente:

- **Status:** OK / warning / breach
- **Error budget remaining**
- **Burn rate:** si el presupuesto se come en horas, es incidente de negocio aunque Davis aún esté “amarillo”

Un SLO verde con usuarios gritando: el SLI está mal elegido (no mide el journey). Eso se escala a diseño ([21](21-diseno-sli-slo-error-budget.md)), no se “arregla” subiendo el porcentaje.

## Umbrales vs anomalías

- **Umbral fijo:** útil cuando el contrato es claro (`disponibilidad probe 99%`)
- **Baseline/Davis:** útil en estacionalidad (tráfico laboral vs noche)

Intermedio: si un job nocturno dispara Problems, no es “Davis tonto”; es **falta de contexto** (maintenance window, filtro de servicio batch, o el job debería ser otro SLO).

## Maintenance windows

Ventanas de mantenimiento evitan ruido en deploys o parches. Mal usadas **ocultan** outages reales. Solo plataforma las define; tú preguntas “¿hay MW ahora?” antes de decir que Dynatrace falló.

## Criterio Intermedio

Sabes **interpretar** SLO y profiles. No eres dueño de la matriz de notificación de la empresa.
