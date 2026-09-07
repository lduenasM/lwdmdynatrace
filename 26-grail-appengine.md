# 26 — Grail, buckets, AppEngine y apps custom (Senior / Master)

## Grail como producto de datos

El Senior trata Grail como un **warehouse con dueño**:

- **Buckets** / retención / permisos de query
- Qué tipo de dato vive dónde (logs de pago vs logs de plataforma)
- Límites de ingest y de consulta
- Clasificación (¿hay datos personales? retención legal vs operativa)

Sin esto, cada squad hace `fetch logs` sobre todo y la plataforma se vuelve un costo opaco.

## AppEngine

Permite **aplicaciones** en el ecosistema Dynatrace (UI + lógica) sobre datos del tenant: portales de un CoE, vistas de negocio, wizards de onboarding.

Senior decide:

- Qué merece una **App** vs un dashboard vs un notebook
- Quién desarrolla, ciclo de vida, autenticación
- Que no se convierta en shadow IT con tokens eternos

## Custom apps vs comprar

Si el 80 % se cubre con Notebooks + Dashboards + Workflows, no construyas una app. Construye cuando hay **un flujo repetible de muchos equipos** (alta de servicio, scorecard de instrumentación).

## Calidad de datos

- Esquemas y campos extraídos en ingest
- Convenios de `service.name`
- Tests: “un deploy de lab genera las entidades esperadas”

## Criterio Senior

Política de buckets/retención escrita y un ejemplo de App **o** una justificación documentada de por qué no hace falta.
