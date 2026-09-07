# 19 — Kubernetes y cloud a escala (Avanzado)

## Problemas que aparecen con muchos clusters

- Namespaces homónimos (`default`, `payments`) en varios clusters → **tags de cluster/account obligatorios**
- OneAgent/operator no homogéneo: un cluster “ciego”
- Cardinalidad: métricas por pod × contenedor × label de CI
- Istio/Gateway API: doble latencia; hay que saber **qué entidad** alerta
- Autoscaling: Davis ve “más hosts” que no son incidente

## Estándar de despliegue (lo que diseñas con plataforma)

- Opt-in por namespace o label (`dynatrace.com/inject`)
- Imagen y versión de OneAgent **pinneadas**
- Recursos del agente (CPU/mem) para no evictar
- Exclusiones: nodos de build, jobs efímeros de CI si no aportan
- Monitoreo del **propio** operator (si el agente muere, no hay magia)

## Kubernetes workload vs servicio APM

El Avanzado mantiene una tabla del squad:

| Workload | service.name APM | SLO | MZ |
| --- | --- | --- | --- |
| `checkout-api` | `checkout-api` | 99.5 % 2s | pagos-prod |

Si no existe esa tabla, los dashboards mienten tras el primer rename.

## Cloud (AWS/Azure/GCP)

- Integrar **cuentas** con least privilege (rol, no clave larga en un wiki)
- Elegir servicios cloud que **cierran el journey** (ALB 5xx, RDS CPU, no 200 métricas vanity)
- Lambdas: cold start, OTel o extensión; no esperes PurePath de JVM clásico
- Retención y regiones: datos que no pueden salir de un país ([24](24-gobierno-iam-mz-tags.md))

## Alertado K8s que sí escala

Prioridad:

1. Impacto de servicio (RED)
2. OOMKilled / CrashLoop **del workload de negocio**
3. Presión de nodo que afecta esos workloads
4. No: CPU de cada replica en HPA sano

## Criterio Avanzado

Documentas **cómo se instrumenta un cluster nuevo** en una página y qué métricas cloud están **fuera de alcance** a propósito.
