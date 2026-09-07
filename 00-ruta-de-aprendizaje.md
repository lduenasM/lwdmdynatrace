# Ruta de aprendizaje: Junior → Senior (Master) → IA

**Master = Senior.** En certificaciones internas, organigramas y esta academia son el mismo techo de competencia técnica y de gobierno. El bloque de **IA** se cursa **al final**: ya tienes criterio de datos, incidentes y plataforma; si no, la IA solo acelera el error.

## Principio

Cada nivel exige **horas en un tenant real** (preferible no-prod). Sin tenant, te quedas en Junior teórico.

| Nivel | Lectura + práctica | Calendario típico |
| --- | --- | --- |
| 1 Junior | ~40–60 h | 2–3 semanas (o bootcamp 5 días + sombra) |
| 2 Intermedio | ~50–70 h | 4–6 semanas on-call acompañado |
| 3 Avanzado | ~80–120 h | 2–4 meses sobre un dominio |
| 4 Senior (Master) | ~120 h + evidencia | 6–12 meses liderando decisiones |
| Cierre IA | ~12–20 h | 1–2 semanas después de Senior (o en paralelo ligero si ya eres Avanzado sólido) |

## Nivel 1 — Junior

Docs [01](01-fundamentos-observabilidad.md)–[08](08-metricas.md) · [ejercicios-junior](capacitacion/ejercicios-junior.md) · [quiz-junior](capacitacion/quiz-junior.md) · [bootcamp](capacitacion/plan-5-dias-junior.md)

Salida: localizas servicio, error rate, PurePath; escalas con URL y ventana; no tocas Settings de prod. **No** usas ChatGPT/Copilot con dumps de logs de prod.

## Nivel 2 — Intermedio

Docs [09](09-problemas-davis.md)–[15](15-buenas-practicas-seguridad.md) · [ejercicios-intermedio](capacitacion/ejercicios-intermedio.md) · [quiz-intermedio](capacitacion/quiz-intermedio.md)

Salida: Problem en 5 viñetas; validas Davis con traza y log; lees SLO; RUM/sintético hasta backend; DQL mínimo; cero PII en tickets.

## Nivel 3 — Avanzado

Docs [16](16-instrumentacion-oneagent-otel.md)–[22](22-costo-cardinalidad-sampling.md) · [ejercicios-avanzado](capacitacion/ejercicios-avanzado.md) · [quiz-avanzado](capacitacion/quiz-avanzado.md)

Salida: OneAgent vs OTel argumentado; metric events con impacto; notebook-runbook; cardinalidad/sampling; API en no-prod; propuesta de SLO.

## Nivel 4 — Senior (Master)

Docs [23](23-arquitectura-plataforma.md)–[30](30-casos-estudio-y-criterio-senior.md) · [ejercicios-senior](capacitacion/ejercicios-senior.md) · [quiz-senior](capacitacion/quiz-senior.md) · [rúbrica](capacitacion/rubrica-certificacion-interna.md)

Salida: topología agentes/AG/Grail/IAM; política MZ/tags; un cambio as-code; game day o postmortem con evidencia; plan de enablement; decisión costo vs señal.

## Cierre — Uso de IA (obligatorio para declarar Senior)

Docs [31](31-uso-de-ia-en-observabilidad.md)–[33](33-ia-prompts-y-practica.md) · [ejercicios-ia](capacitacion/ejercicios-ia.md) · [quiz-ia](capacitacion/quiz-ia.md)

Salida: distingues Davis causal vs Copilot vs LLM externo; un canal aprobado para prompts; un runbook de “IA propone, humano verifica”; no hay PII ni tokens en modelos públicos; conoces el esbozo de observar **apps que usan LLM**.

## Orden compacto

```
Semanas 1–3      Nivel 1 Junior
Semanas 4–8      Nivel 2 Intermedio
Meses 3–6        Nivel 3 Avanzado
Meses 6–12       Nivel 4 Senior (Master)
Últimas 1–2 sem  Módulo IA (cierre)
```

Detalle: [capacitacion/plan-por-niveles.md](capacitacion/plan-por-niveles.md).

## Qué no salta etapas

- Senior sin haber **validado Davis con una traza** es teatro.
- IA al inicio sin [15](15-buenas-practicas-seguridad.md) filtra secretos a un proveedor externo.
- Avanzado sin **cardinalidad** genera facturas y tenants inútiles.
