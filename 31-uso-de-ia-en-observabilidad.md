# 31 — Uso de IA en observabilidad (cierre de la ruta)

Este módulo se estudia **al final**, cuando ya eres capaz de leer un Problem, un PurePath y un SLO. La IA **no sustituye** esos músculos: los acelera o los atrofia, según cómo la uses.

## Tres IAs distintas (no las mezcles)

| Nombre en la práctica | Qué es | Fortaleza | Riesgo |
| --- | --- | --- | --- |
| **Davis AI** | IA **causal / de correlación** de Dynatrace sobre topología y señales | Agrupa síntomas, propone RCA, abre Problems | RCA incompleto si falta instrumentación o MZ |
| **Davis CoPilot / Copilot en el tenant** | IA **generativa** *dentro* de Dynatrace (NL → DQL, ayuda en UI) | Bajar la barrera de Grail | DQL incorrecto, consultas caras, campos inventados |
| **LLM externo** (ChatGPT, Copilot de IDE, modelos corporativos) | Modelo general | Runbooks, explicar conceptos, redactar postmortem | **Fuga de datos**, alucinaciones, “arreglos” de Settings que no aplican a tu tenant |

Davis (Problems) lo usas desde Intermedio ([09](09-problemas-davis.md)). Este cierre enseña a usar **generativa** con oficio de Senior.

## Regla de oro

**La IA propone. El humano verifica en el tenant. Nunca se pega a un modelo público: tokens, PII, payloads, capturas de sesión, listas de clientes, dumps de logs de prod.**

Si la empresa tiene un **GPT/LLM corporativo** con contrato y retención acordada, úsalo según política. Si no hay política, asume **no**.

## Davis — uso Senior (ya no “creer el banner”)

1. Lee RCA como **hipótesis**
2. Exige 2–3 PurePaths y un log
3. Pregunta qué **no** está en el mapa (dependencia no instrumentada)
4. Si Davis se equivoca a menudo en un patrón: **instrumenta o recorta detección**, no “ignorar Problems”

Eso es IA **de producto**. No es ChatGPT.

## Copilot en Dynatrace (NL → DQL / explicación)

Uso sano:

- “DQL para 5xx de *este* servicio en 15 min” → **lees** la query (filtros, métrica, `limit`)
- Pides que **explique** un notebook que ya tienes
- Generas un **borrador** de dashboard tile

Uso insano:

- Ejecutar a ciegas un `fetch logs` de 14 días
- Aceptar métricas que no existen en tu catalog
- Dejar que arme alerting de prod

Flujo obligatorio: **generar → entender → recortar tiempo → ejecutar en no-prod o ventana corta → guardar en Git/notebook del equipo**.

## LLM externo — para qué sí

- Explicar un concepto (percentiles, error budget) **sin** pegar datos reales
- Borrador de ADR o postmortem **anonimizado** (`servicio A`, no clientes)
- Lista de preguntas para un game day
- Traducir un error **genérico** (`NullPointerException` en `PaymentMapper`) sin stack con datos

## LLM externo — para qué no

- Pegar PurePath, HAR, cookies, `Authorization`
- “Revisa este export de usuarios RUM”
- Pedir exploits, bypass de IAM, o cómo ocultar ingest
- Config de prod copiada tal cual del chat

## Cómo debe usarla cada nivel (resumen)

| Nivel | Davis | Copilot tenant | LLM externo |
| --- | --- | --- | --- |
| Junior | Leer Problem con buddy | No, o solo con buddy y sin ejecutar queries pesadas | Solo teoría, cero datos del tenant |
| Intermedio | Validar RCA | DQL de investigación con `limit` | Texto anonimizado |
| Avanzado | Ajustar detección si Davis falla | Runbooks DQL; review de query | Borradores de diseño |
| Senior | Gobierno de Davis + ruido | Estándar de Copilot; costo de queries | Canal corporativo; política |

Detalle de prompts: [33](33-ia-prompts-y-practica.md). Gobierno y apps LLM: [32](32-gobierno-ia-y-apps-llm.md).
