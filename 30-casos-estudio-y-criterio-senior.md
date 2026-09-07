# 30 — Casos de estudio y criterio Senior (Master)

**Master = Senior.** Esta página es la **lista de evidencia**. Si no puedes hablar de casos reales (anonimizados), no firmes el nivel.

## Caso A — “Davis dijo la DB, era el pool”

- Síntoma: p95 y Davis RCA en queries
- Validación: PurePath = espera de pool, no SQL lento
- Lección Senior: topología sin **saturación de recurso de app** lleva a RCA incompleto; metric event de pool + runbook

## Caso B — “Sintético rojo, usuarios bien”

- Script roto tras cambio de CSS; RUM estable
- Lección: sintéticos son código; dueño; no paginar a 30 personas por un selector

## Caso C — “Log DEBUG en prod”

- Ingest ×10, consultas lentas, costo
- Lección: techos, pipelines, [22](22-costo-cardinalidad-sampling.md)

## Caso D — “El squad no ve prod”

- MZ mal; on-call ciego
- Lección: gobierno [24](24-gobierno-iam-mz-tags.md) es disponibilidad

## Caso E — “Tres tenants, cero Git”

- Drift, alertas zombi
- Lección: as-code [25](25-observability-as-code.md)

## Autoevaluación Senior (todas sí)

- [ ] He dibujado la topología real de **nuestra** empresa (no la del vendor)
- [ ] He dicho **no** a una métrica/log/alerta y se sostuvo
- [ ] Hay al menos un artefacto as-code mío o de mi liderazgo en Git
- [ ] Un incidente o game day cambió detección o gobierno
- [ ] Formé a alguien Junior/Intermedio (no solo “pasé links”)
- [ ] Cursé el cierre de **IA** ([31](31-uso-de-ia-en-observabilidad.md)–[33](33-ia-prompts-y-practica.md))

## Qué no cuenta como Senior

- Certificación pública sin tenant ni gobierno
- Ser admin del tenant y haber creado 40 dashboards ruidosos
- Usar Copilot para DQL sin poder leer el resultado
