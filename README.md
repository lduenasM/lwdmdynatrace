# Observabilidad con Dynatrace — de Junior a Senior (Master)

Kit de **documentación** y **capacitación**: desde quien **inicia** hasta **Senior**. En este material **Master = Senior**: mismo nivel, mismo criterio de salida. No hay un quinto rango por encima; lo que cierra la ruta es el **uso de IA** (Davis, Copilot y asistentes) con criterio profesional.

> El producto se llama **Dynatrace**.

## Mapa de niveles

| Nivel | Nombre en este kit | Rol equivalente | Eres capaz de… | Documentos |
| --- | --- | --- | --- | --- |
| **1** | **Junior** | L1, dev que empieza | Entrar al tenant, hallar un servicio, leer un PurePath, no romper prod | [00](00-ruta-de-aprendizaje.md)–[08](08-metricas.md) |
| **2** | **Intermedio** | L2, on-call acompañado | Cerrar un incidente con Problems, SLO, RUM, K8s básico y DQL | [09](09-problemas-davis.md)–[15](15-buenas-practicas-seguridad.md) |
| **3** | **Avanzado** | SRE / plataforma de aplicación | Instrumentar, alertar bien, DQL serio, costo de datos, API | [16](16-instrumentacion-oneagent-otel.md)–[22](22-costo-cardinalidad-sampling.md) |
| **4** | **Senior (Master)** | Staff / CoE / arquitecto de observabilidad | Topología del tenant, gobierno, as-code, cultura SRE | [23](23-arquitectura-plataforma.md)–[30](30-casos-estudio-y-criterio-senior.md) |
| **Cierre** | **Uso de IA** | Todos los niveles, con reglas distintas | Davis, Copilot, LLMs externos; cuándo confiar y cuándo no | [31](31-uso-de-ia-en-observabilidad.md)–[33](33-ia-prompts-y-practica.md) |

**Nadie es Senior solo por leer.** Cada nivel tiene criterio de salida, ejercicios y quiz. Rúbrica: [capacitacion/rubrica-certificacion-interna.md](capacitacion/rubrica-certificacion-interna.md).

## Cómo usar este kit

1. Empieza en el nivel que **demuestres**. Si no has abierto un PurePath en un tenant real, eres Junior.
2. Sigue [00-ruta-de-aprendizaje.md](00-ruta-de-aprendizaje.md).
3. Practica en **no-producción**. Producción: observación + buddy hasta Intermedio sólido.
4. El módulo de **IA se estudia al final** (tras Senior), pero las **reglas de no pegar PII/tokens a un LLM** aplican desde el día 1 ([15](15-buenas-practicas-seguridad.md)).
5. La UI cambia (classic vs Apps/Grail). El flujo no: Problem → entidad → traza → log → impacto.

## Índice completo

### Nivel 1 — Junior

| # | Documento |
| --- | --- |
| 00 | [Ruta de aprendizaje](00-ruta-de-aprendizaje.md) |
| 01 | [Fundamentos de observabilidad](01-fundamentos-observabilidad.md) |
| 02 | [Introducción a Dynatrace](02-introduccion-dynatrace.md) |
| 03 | [Arquitectura, OneAgent y ActiveGate](03-arquitectura-oneagent.md) |
| 04 | [Primeros pasos en la UI](04-primeros-pasos-ui.md) |
| 05 | [Hosts, procesos, servicios y Smartscape](05-hosts-procesos-servicios.md) |
| 06 | [Trazas y PurePath](06-trazas-purepath.md) |
| 07 | [Logs](07-logs.md) |
| 08 | [Métricas](08-metricas.md) |

### Nivel 2 — Intermedio

| # | Documento |
| --- | --- |
| 09 | [Problems y Davis AI](09-problemas-davis.md) |
| 10 | [Dashboards](10-dashboards.md) |
| 11 | [Alertas, profiles y SLO (lectura)](11-alertas-slo.md) |
| 12 | [RUM y sintéticos](12-rum-sinteticos.md) |
| 13 | [Kubernetes y cloud (operación)](13-kubernetes-cloud.md) |
| 14 | [DQL y Grail (introducción)](14-dql-grail.md) |
| 15 | [Buenas prácticas y seguridad del dato](15-buenas-practicas-seguridad.md) |

### Nivel 3 — Avanzado

| # | Documento |
| --- | --- |
| 16 | [Instrumentación: OneAgent profundo y OpenTelemetry](16-instrumentacion-oneagent-otel.md) |
| 17 | [Detección de anomalías, Metric Events y perfiles](17-deteccion-anomalias-y-perfiles.md) |
| 18 | [DQL avanzado, Notebooks y runbooks](18-dql-avanzado-notebooks.md) |
| 19 | [Kubernetes y cloud a escala](19-kubernetes-cloud-escala.md) |
| 20 | [API, Workflows y automatización](20-api-workflows-automatizacion.md) |
| 21 | [Diseño de SLI/SLO y error budget](21-diseno-sli-slo-error-budget.md) |
| 22 | [Costo, cardinalidad y sampling](22-costo-cardinalidad-sampling.md) |

### Nivel 4 — Senior (Master)

| # | Documento |
| --- | --- |
| 23 | [Arquitectura de la plataforma de observabilidad](23-arquitectura-plataforma.md) |
| 24 | [Gobierno: IAM, Management Zones, tags, multi-env](24-gobierno-iam-mz-tags.md) |
| 25 | [Observability as code (Monaco, Terraform)](25-observability-as-code.md) |
| 26 | [Grail, buckets, AppEngine y apps custom](26-grail-appengine.md) |
| 27 | [SRE: comando de incidente, game days, postmortem](27-sre-incidentes-game-days.md) |
| 28 | [Observabilidad de negocio](28-observabilidad-de-negocio.md) |
| 29 | [CoE, enablement y métricas del programa](29-coe-enablement-programa.md) |
| 30 | [Casos de estudio y criterio Senior](30-casos-estudio-y-criterio-senior.md) |

### Cierre — Uso de IA

| # | Documento |
| --- | --- |
| 31 | [IA en observabilidad: mapa, Davis, Copilot, LLMs](31-uso-de-ia-en-observabilidad.md) |
| 32 | [Gobierno de IA y observabilidad de aplicaciones LLM](32-gobierno-ia-y-apps-llm.md) |
| 33 | [Prompts, práctica por nivel y trampas](33-ia-prompts-y-practica.md) |

### Capacitación

| Recurso | Uso |
| --- | --- |
| [Plan por niveles](capacitacion/plan-por-niveles.md) | Calendario Junior→Senior + IA |
| [Plan 5 días Junior](capacitacion/plan-5-dias-junior.md) | Bootcamp inicial |
| [Guía del facilitador](capacitacion/guia-facilitador.md) | Quien imparte el curso |
| [Ejercicios Junior](capacitacion/ejercicios-junior.md) · [Intermedio](capacitacion/ejercicios-intermedio.md) · [Avanzado](capacitacion/ejercicios-avanzado.md) · [Senior](capacitacion/ejercicios-senior.md) · [IA](capacitacion/ejercicios-ia.md) | Práctica |
| [Quiz Junior](capacitacion/quiz-junior.md) · [Intermedio](capacitacion/quiz-intermedio.md) · [Avanzado](capacitacion/quiz-avanzado.md) · [Senior](capacitacion/quiz-senior.md) · [IA](capacitacion/quiz-ia.md) | ≥ 80 % |
| [Checklist de onboarding](capacitacion/checklist-onboarding.md) | Accesos y buddy |
| [Rúbrica interna](capacitacion/rubrica-certificacion-interna.md) | Firma de nivel (Senior = Master) |
| [Glosario](capacitacion/glosario.md) · [Cheat sheet](capacitacion/cheat-sheet.md) | Consulta rápida |

## Permisos por nivel

| Acción | Junior | Intermedio | Avanzado | Senior |
| --- | --- | --- | --- | --- |
| Ver MZ de su app en no-prod | Sí | Sí | Sí | Sí |
| Ver prod (solo lectura) | Con buddy | Sí | Sí | Sí |
| Dashboard personal | Sí | Sí | Sí | Sí |
| Dashboard compartido | No | Con revisión | Sí | Sí |
| Cambiar alerting / SLO de negocio | No | No | Propuesta + review | Aprueba gobierno |
| OneAgent en prod | No | No | Con plataforma | Diseña el estándar |
| Tokens API / Monaco en prod | No | No-prod acotado | Pipeline | Dueño del patrón |
| Pegar datos de prod a un LLM público | **Nunca** | **Nunca** | **Nunca** | Define el canal aprobado |

## Fuente de verdad del producto

[https://docs.dynatrace.com](https://docs.dynatrace.com). Este kit enseña **criterio y flujos**, no capturas eternas de botones.
