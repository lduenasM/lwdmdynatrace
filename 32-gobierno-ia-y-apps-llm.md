# 32 — Gobierno de IA y observabilidad de aplicaciones LLM (Senior)

## Gobierno del uso de IA **para** operar Dynatrace

El CoE / Senior publica una página corta:

1. **Herramientas aprobadas** (Copilot del tenant; LLM corporativo; IDE con políticas)
2. **Prohibido** en herramientas no aprobadas (prod dumps)
3. **Clasificación:** público / interno / restringido / secreto — solo las dos primeras a un LLM, y restringido **nunca** a público
4. **DQL generado:** misma review que código (cuatro ojos si va a dashboard oficial)
5. **Costo:** queries Grail disparadas por un bot = presupuesto de observabilidad
6. **Auditoría:** si el Copilot guarda historial, dónde vive y cuánto

Sin este párrafo, “habilitar Copilot a toda la org” es un incidente de datos en cámara lenta.

## Observabilidad **de** sistemas que ya usan IA (apps LLM)

Muchas aplicaciones llaman a modelos (asistentes, clasificación, RAG). Un Senior no las trata como “caja negra HTTP”.

Señales útiles (concepto; el empaquetado en Dynatrace/OTel evoluciona):

| Señal | Para qué |
| --- | --- |
| Latencia del provider (p95) | SLO del journey que incluye el modelo |
| Tasa de error / timeout / 429 | Saturación y cuotas |
| Tokens in/out (si el contrato permite métricas, **no** el texto) | Costo y anomalías |
| Trazas del orquestador (RAG: retrieve → generate) | Dónde se fue el tiempo |
| Fallos de tools/function calling | Bugs de integración |

**No** envíes prompts ni respuestas completas a logs de prod “para debug” si contienen datos de clientes. Redacta, hashea, o usa entornos de test.

OpenTelemetry y vendors publican semconv de gen-AI; alinea **un** estándar con plataforma, no un JSON distinto por squad.

## Riesgos específicos

- Cardinalidad: dimensión `prompt_hash` por cada request → explosión ([22](22-costo-cardinalidad-sampling.md))
- RUM + chatbot: session replay puede grabar lo que el usuario escribió al modelo
- Davis: un timeout masivo al proveedor de modelo **sí** es incidente; un “el modelo se alucinó” **no** siempre se ve en APM — producto debe definir SLI de calidad aparte (offline eval), no fingir que Dynatrace mide verdad semántica

## Criterio Senior

Hay política de **IA para operar** y un patrón de **telemetría para apps LLM** (qué se mide, qué jamás se loguea).
