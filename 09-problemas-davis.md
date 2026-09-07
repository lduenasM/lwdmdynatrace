# 09 — Problems y Davis AI (Intermedio)

## Qué es un Problem (y qué no)

Un **Problem** es un incidente que **Davis** abre al correlacionar eventos y anomalías sobre entidades (servicio, host, app, K8s…) con un **impacto** estimado.

No es:

- Un log ERROR suelto
- Un ticket de Jira (aunque se integre)
- Una alerta de umbral clásico aislada (eso puede *alimentar* a Davis)

En un incidente, **Problems es el primer clic**, no el dashboard bonito.

## Cómo lee Davis (modelo mental)

1. Detecta **eventos**: degradación de response time, failure rate, CPU, saturación, deploy, eventos cloud, sintéticos, etc.
2. Conoce la **topología** (Smartscape): quién depende de quién.
3. Agrupa en un Problem: **causa raíz candidata** + **afectados**.
4. Mantiene el Problem **abierto** mientras hay impacto; lo cierra cuando las señales vuelven al baseline.

Tu trabajo intermedio: **verificar**, no idolatrar. Davis se equivoca con:

- Tráfico nuevo sin historia (primer día de un servicio)
- Batch jobs que parecen “outage”
- Dependencias no instrumentadas (el culpable está “fuera del mapa”)
- MZ incompletas: ves un recorte y la causa está en otra zona

## Anatomía de un Problem (qué copiar al canal)

| Campo | Para qué |
| --- | --- |
| ID (`P-…`) y **URL** | Fuente única |
| Severidad / impacto | User, service, infra |
| Root cause (texto Davis) | Hipótesis, no veredicto |
| Entidades afectadas | Blast radius |
| Timeline | ¿Coincide con deploy/cambio? |
| Duplicate / merge | A veces hay varios Problems del mismo fuego |

Plantilla:

```
Problem: P-xxxxx  <URL>
Impacto: checkout-api failure 7%, RUM conversión checkout caída
Causa Davis: dependencia billing-svc latency
Validación: PurePath span HTTP 504; log TimeoutException
Cambio cercano: deploy billing 14:01
Dueño: equipo billing | siguiente acción: rollback vs scale
```

## Problems vs “alertas clásicas”

En Dynatrace moderno conviven:

- **Davis problems** (correlación)
- **Metric events** / custom events (reglas que tú defines)
- Notificaciones (email, Slack, ServiceNow, Workflows)

Un entorno ruidoso suele ser: demasiados metric events de infra **sin** filtro de impacto. El nivel Avanzado ([17](17-deteccion-anomalias-y-perfiles.md)) enseña a recortar. En Intermedio: **no apagues Davis**; documenta el ruido y escala.

## Flujo de investigación (Intermedio)

1. Abre el Problem → lee RCA y afectados.
2. Abre el **servicio de impacto** (no solo el host de CPU).
3. Confirma **failure rate / response time** en la misma ventana.
4. 2–3 **PurePaths** failed o lentos (no uno solo: evita el outlier).
5. Log correlacionado.
6. ¿RUM o sintético también rojo? Eso prioriza negocio.
7. Escribe hipótesis + siguiente acción (escalar equipo X, rollback, feature flag).

## Merge, duplicados y fatiga

Si hay 15 Problems abiertos, agrupa por **misma causa raíz** y **misma ventana**. Comenta en el canal el **Problem canónico**. Pedir a plataforma que revise alerting profiles es un outcome de Intermedio maduro, no “cerrar Problems a mano” como deporte.

## Integraciones típicas

Problems pueden abrir incidentes en ITSM. El junior pega la URL. El intermedio verifica que el **payload** (entidad, MZ) llega al equipo correcto. Si todo cae en un único grupo “Dynatrace”, el gobierno está mal ([24](24-gobierno-iam-mz-tags.md)).

## Criterio de nivel

**Intermedio:** no sales de un incidente sin URL de Problem + traza + decisión.  
**Aún no Avanzado:** no rediseñas la detección global.

Davis es la IA **causal** del producto. La IA **generativa** (Copilot, ChatGPT) se forma al **cierre** de la ruta, cuando ya validas RCA a mano: [31](31-uso-de-ia-en-observabilidad.md). Senior = Master.
