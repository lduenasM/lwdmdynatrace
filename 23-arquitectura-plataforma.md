# 23 — Arquitectura de la plataforma de observabilidad (Senior / Master)

## Qué diseña un Senior

No “un dashboard más”. El **sistema** con el que toda la empresa observa: agentes, red, datos, identidad, costo y operación.

## Bloques a dibujar (una página)

1. **Fuentes:** OneAgent, OTel collector, RUM, sintéticos, cloud API, logs pipeline
2. **Camino:** ActiveGates (red, proxy, privados), firewalls, no NAT improvisado
3. **Control plane:** tenant SaaS vs Managed, entornos (prod / no-prod), región y residencia
4. **Datos:** Grail buckets, retención, clasificaciones (logs vs trazas vs RUM)
5. **Identidad:** SSO, grupos, MZ, tokens de automatización
6. **Consumo:** Apps, notebooks, ITSM, data out (si existe y es legal)
7. **Operación:** who-is-on-call de la **propia** plataforma (agente caído, ingest parado)

Si no puedes dibujarlo, aún no eres Senior de observabilidad: eres Avanzado de una app.

## SaaS vs Managed

| | SaaS | Managed |
| --- | --- | --- |
| Operas el cluster Dynatrace | No | Sí (capacidad, upgrades) |
| Residencia | Contrato y región del vendor | Tu datacenter |
| Parches | Vendor | Tu calendario |
| Típico | Mayoría de empresas | Regulatorio / air-gap |

El Senior documenta **por qué** se eligió y el plan de continuidad (qué pasa si el tenant no responde: sintéticos externos, status page, runbook).

## Topología de ActiveGate

- AG de **salida** (SaaS) vs AG de **módulo** (sintéticos privados, Kubernetes, cloud)
- Alta disponibilidad: un AG único es SPOF de visibilidad
- Segmentación de red: prod no habla a internet; el AG sí, con policy

## Multi-entorno

Patrones:

- **Un tenant**, MZ y tags `env:*` (simple; riesgo de ver prod por error de permiso)
- **Dos tenants** (prod / no-prod): más aislamiento, más doble mantenimiento as-code
- Cuentas cloud y clusters **etiquetados** desde el día 0

El Senior elige y escribe el ADR.

## SLO de la propia plataforma

La observabilidad también se cae. Define:

- % de hosts con OneAgent reciente
- Lag de ingest
- Sintéticos de “Dynatrace accesible”
- Tiempo de restaurar un AG

## Criterio Senior

Un diagrama + ADR de 2 páginas que otro Senior de plataforma puede atacar en review y **no se cae** en la primera pregunta de red o IAM.
