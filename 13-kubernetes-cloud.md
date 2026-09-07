# 13 — Kubernetes y cloud (operación Intermedio)

## Analogía para no perderte

| Kubernetes | Qué mirar en Dynatrace |
| --- | --- |
| Cluster | Entidad cluster, salud de API server (si hay integración) |
| Node | Host / saturación, pressure, discos |
| Namespace | Filtro y MZ |
| Workload (deploy/sts) | Restarts, CPU/mem vs request/limit |
| Pod | Efímero: no lo uses como ID eterno |
| Service / ingress | A menudo el **servicio APM** que ya conoces |

El error Junior: “el pod `xxx-7f8d9` está mal” dos horas después (el pod ya no existe). Habla de **workload + namespace + env**.

## Señales operativas (orden)

1. ¿El **servicio APM** (requests) está mal? Si sí, hay impacto de aplicación.
2. ¿**Restarts**, CrashLoop, OOMKilled? Mira eventos K8s y límites de memoria.
3. ¿**Throttling CPU** (usage vs limit)? Latencia sin “CPU host” alto.
4. ¿Nodo NotReady / disk pressure? Impacto transversal.
5. ¿OneAgent o operator no corre en ese namespace? Entonces no hay magia APM.

## Integraciones cloud

Dynatrace puede tirar de **AWS/Azure/GCP APIs** (vía ActiveGate o config): RDS, ELB, functions, etc.

Intermedio:

- Un 5xx de API Gateway puede ser el síntoma; la causa está en el servicio o en IAM/timeout del gateway
- Métricas cloud **sin** traza son el mundo pre-APM: úsalas como complemento

## Service mesh, sidecars, probes

- Health probes ruidosos inflan throughput y “errores” si no se filtran
- Istio/Envoy: puede haber **dos** capas de latencia (app + proxy). Aprende cuál entidad estás viendo
- Serverless / scale-to-zero: cold start parece “problema Davis” el primer request

## Qué no hagas en este nivel

- Cambiar OneAgent operator, CSI, o privilegios de Dynatrace en el cluster
- Abrir todos los namespaces “para ver mejor”
- Alertar por `CPU > 80%` de cada pod (fatiga garantizada)

Profundidad de plataforma: [19](19-kubernetes-cloud-escala.md).

## Handover típico K8s

```
Workload: payments-api (ns: pagos, env: prod)
Síntoma: p95 2.8s, OOMKilled 12 reinicios 14:10–14:18
Límite mem: 256Mi; heap Java evidencia saturación
Problem: P-…  |  PurePath: alloc/GC
Acción propuesta: subir limit (plataforma) vs fuga (dev)
```
