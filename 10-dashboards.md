# 10 — Dashboards (Intermedio)

## Para qué sirve un dashboard

Un dashboard **alinea la mirada del equipo** en 30 segundos: ¿hay incidente?, ¿qué SLI?, ¿qué release?

No sustituye:

- Problems (correlación)
- PurePath (causa)
- Un notebook DQL de investigación profunda

## Classic vs Dashboards de Grail/Apps

Pueden coexistir **Dashboards clásicos** y **Dashboards** sobre Grail. Usa el estándar de tu tenant. No dupliques el mismo SLI en tres herramientas.

## Tipos que debe tener un equipo

| Tipo | Público | Contenido |
| --- | --- | --- |
| **Salud del servicio** | On-call | RED del servicio crítico + dependencias + Problems |
| **SLO** | Lead + producto | Cumplimiento, error budget, burn rate |
| **Journey / RUM** | Negocio técnico | Funnel login→pago, JS errors, apdex |
| **Plataforma** | K8s/cloud | Nodos, saturación, ingest OneAgent |
| **Personal / laboratorio** | Tú | Experimentos; no es la fuente de verdad |

## Diseño que no miente

- Un panel = **una pregunta** (`¿el checkout cumple p95 < 2s?`)
- Filtro por **tag/MZ/entorno** (nunca “todo el tenant” en un gráfico de 40 series)
- Misma **ventana** y zona horaria que el incidente
- Percentiles o tasas, no solo promedio
- Enlace a la entidad (drilldown), no PNG huérfano
- Anota **deploys** si hay eventos/markers

## Antipatrones

- 20 widgets de CPU de hosts distintos “por si acaso”
- Colores semáforo sin definición de rojo
- Dashboard de un individuo como proceso oficial
- Métricas con cardinalidad explosiva (split por `userId`)
- Copiar un dashboard de internet con métricas que tu tenant no tiene

## Permisos

- Junior: dashboard **personal**
- Intermedio: puede **proponer** uno de equipo; un senior/plataforma lo publica
- Nunca borres el dashboard del squad en un “cleanup” de viernes

## Calidad de un dashboard de on-call (checklist)

- [ ] Cabecera: app, env, dueño, enlace a runbook
- [ ] Problems o lista de eventos
- [ ] SLI del journey más crítico
- [ ] Dependencia más frágil (DB, pago, IdP)
- [ ] Saturación (CPU/mem/hilos/GC) **del proceso del servicio**, no de un host ajeno
- [ ] Dónde ir después (Services, K8s workload)

## Relación con nivel Avanzado

Cuando el dashboard se vuelva un **notebook parametrizado** o una **app** AppEngine, pasas a [18](18-dql-avanzado-notebooks.md) y [26](26-grail-appengine.md).
