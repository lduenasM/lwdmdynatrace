# 28 — Observabilidad de negocio (Senior / Master)

## Qué es

Ligar **técnico** con **resultado**: conversión, tiempo de un onboarding, fallos de un canal, no solo p95 de un pod.

## Qué no es

- Vanity: “miles de user actions” sin definición
- Espiar usuarios: PII, perfiles, session replay fuera de política
- Duplicar un data warehouse de analítica de producto (puede **complementar**, no reemplazar legal/BI)

## Patrones sanos

- Un **funnel** de 4–6 pasos instrumentados (RUM user actions o eventos de backend)
- Correlación: caída de conversión **y** 5xx en `/pay` en la misma ventana → hipótesis fuerte
- Métricas de negocio como **eventos** o atributos de baja cardinalidad (`channel=web|mobile`), nunca `customerName`

## Dueños

Producto define el funnel. Ingeniería instrumenta. Observabilidad Senior **evita** que RUM se convierta en CRM ilegal.

## Criterio Senior

Un journey de negocio con SLI técnico + indicador de negocio, documentado, con privacidad revisada.
