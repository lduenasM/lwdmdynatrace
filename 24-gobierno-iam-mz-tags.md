# 24 — Gobierno: IAM, Management Zones, tags, multi-env (Senior / Master)

## Gobierno = quién ve qué, quién cambia qué, cómo se nombra

Sin esto, Junior ve de más (fuga) o de menos (ciego). Los alerting profiles no enrutan. Los SLO no tienen dueño.

## Management Zones

Diseño Senior:

- Recorte por **producto/tribu + env**, no por hostname efímero
- Prod separado en permisos aunque la MZ se llame parecido
- MZ de “plataforma observabilidad” para operadores del tenant
- Evitar 200 MZ que nadie entiende: catálogo versionado

Prueba de aceptación: un nuevo ingeniero del squad **solo** ve su app en no-prod al día 1, y prod con el rol correcto.

## IAM / grupos

- SSO; nada de usuarios locales eternos
- Roles: viewer app, on-call, settings-app, admin-tenant (pocos)
- Break-glass documentado y auditado
- Cuentas de servicio para Monaco/CI con scope mínimo

## Taxonomía de tags (contrato)

Obligatorios sugeridos:

| Tag | Ejemplo | Uso |
| --- | --- | --- |
| `app` | `checkout` | MZ, dashboards |
| `team` | `pagos` | routing de Problems |
| `env` | `prod` `qa` | filtros |
| `criticality` | `tier1` | profiles |

Reglas automáticas (cloud labels, K8s labels) **por encima** de tags manuales en hosts que rotan. El Senior publica el diccionario y rechaza PRs de infra sin labels.

## Naming de servicios y process groups

Un rename rompe historia de SLO. Cambios = versión y comunicación. Detection rules son código ([25](25-observability-as-code.md)).

## Datos y residencia

RUM y logs pueden ser personales. El Senior alinea con legal:

- Qué entornos tienen session replay
- Qué campos se enmascaran en capturas
- Retención por bucket

## Criterio Senior

Existe un **documento vivo** (no un slide) de MZ + tags + roles, con dueño, y un proceso de alta de una app nueva que un Avanzado puede ejecutar.
