# 33 — Prompts, práctica por nivel y trampas (cierre IA)

## Plantilla de prompt que no filtra prod

Trabaja con **metadatos**, no con copias:

```
Contexto: Dynatrace Grail, servicio "checkout-api", env qa.
Objetivo: DQL de tasa de HTTP 5xx por http.route últimos 15 min.
Restricciones: no inventes nombres de métrica; usa placeholder
dt.entity.service = SERVICE_ID; incluye limit; no pidas fetch logs de 7 días.
Explica cada pipe en una línea.
```

Luego **pegas SERVICE_ID tú** en el tenant, no en un chat público si el ID se considera interno sensible — en muchas empresas el ID de entidad es interno; usa el canal corporativo.

## Prompt para interpretar un Problem (anonimizado)

```
Davis RCA (parafraseado): latency en dependencia "billing".
Impacto: failure rate 7% en API de checkout.
Pregunta: lista 5 hipótesis ordenadas y qué señal en PurePath/log confirmaría cada una.
No asumas nombres de tablas ni datos de clientes.
```

## Prompt para postmortem (Senior)

```
Redacta timeline y 3 acciones preventivas.
Hechos: 14:02 sintético falló; 14:05 Problem P-xxx (no pegar URL interna si el modelo es público);
causa confirmada: pool de conexiones agotado, no SQL lento.
Tono blameless, español, viñetas.
```

## Trampas (alucinaciones típicas)

| El modelo dice | Tú verificas |
| --- | --- |
| Nombre de métrica “oficial” | Catalog / Data Explorer de **tu** tenant |
| `fetch spans` con campos inventados | Schema explorer |
| “Activa este setting en Settings → …” | Docs de tu versión; no copies rutas de UI viejas |
| RCA definitivo | PurePath × N |
| Query sin `filter` de tiempo extra | El picker + recorte de entidad |
| Regex que extrae tarjetas/emails | **No** la uses |

## Ejercicio mental de Junior

Si el Copilot genera 40 líneas de DQL que no entiendes, **no la ejecutes**. Vuelve a [14](14-dql-grail.md). La IA no es un atajo al nivel Intermedio.

## Ejercicio de Senior

Incluye en el runbook del squad:

```
1. Copilot/LLM propone DQL
2. Par o dueño lee filtros y costo
3. Ejecutar ventana ≤ 15 min
4. Si es dashboard oficial → PR as-code
```

## Criterio de cierre de la academia

Pasas [capacitacion/quiz-ia.md](capacitacion/quiz-ia.md) y [capacitacion/ejercicios-ia.md](capacitacion/ejercicios-ia.md). Sin esto, el nivel **Senior (Master) queda incompleto** en este kit: hoy operar observabilidad incluye gobernar IA, no solo hosts.
