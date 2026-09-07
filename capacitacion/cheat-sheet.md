# Cheat sheet — incidente

```
1. Problems → URL P-xxxx
2. Servicio de IMPACTO (no solo CPU host)
3. Ventana: 15 min antes del síntoma → ahora | zona horaria
4. RED: rate, errors, duration (p95)
5. PurePath × 2–3 (failed o lentos) → span dominante
6. Log correlacionado (sin PII)
7. RUM o sintético: ¿usuario real o robot?
8. ¿Deploy/evento a la misma hora?
9. Handover: impacto, hipótesis, dueño, siguiente acción
```

**No:** Settings prod, tokens, payloads, LLM público.

**DQL lab:** `fetch` → `filter` entidad → `limit` → ventana corta.

**IA:** Davis = hipótesis; Copilot = borrador de query que **lees**; LLM = texto anonimizado.

Nombres de menú: búsqueda global del tenant.
