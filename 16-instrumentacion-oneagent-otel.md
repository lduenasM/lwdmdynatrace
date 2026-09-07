# 16 — Instrumentación: OneAgent profundo y OpenTelemetry (Avanzado)

## Decisión que un Avanzado debe poder argumentar

| Enfoque | Cuándo | Costo de ownership |
| --- | --- | --- |
| **OneAgent** (auto-inject) | Runtimes soportados, velocidad, code-level, poco cambio de app | Operar agente/operator; versiones; reinicios |
| **OpenTelemetry** | Policloud, vendor-neutral, lenguajes/edge no cubiertos, estándar de equipo | Código, SDK, collectors, semconv, sampling |
| **Híbrido** | Lo más habitual: OneAgent en JVM/.NET + OTel en jobs/lambdas/front | Disciplina de **un** modelo de servicio y de trace id |

No hay ganador universal. El criterio: **cobertura del journey**, **cardinalidad**, **operación**, **lock-in aceptable**.

## OneAgent — palancas que no son de Junior

- **Process group detection:** reglas para no fusionar dos apps en un servicio
- **Custom service detection / service naming:** endpoints que importan al negocio
- **Deep monitoring** por proceso; tecnologías no soportadas → OTel o custom
- **Captura de método / request attributes:** útil y **peligroso** (PII). Mínimo privilegio, allowlist de parámetros
- **Log enrichment:** correlacionar `trace_id` con el logger
- **OneAgent en K8s:** operator, CSI, webhook de inyección, namespaces opt-in vs opt-out
- **Versiones y reinicios:** ventanas; no “upgrade global viernes 18:00”

Si dos microservicios aparecen como uno solo, el mapa miente. Arreglar **detección** es trabajo Avanzado.

## OpenTelemetry — contrato mínimo

1. **Trazas** con context propagation (W3C Trace Context) de punta a punta
2. **Nombres de span** estables (no URLs con IDs)
3. **Recursos:** `service.name`, `deployment.environment`, `service.version`
4. **Export:** collector (o ingest Dynatrace OTLP) — no cada pod hablando a internet a su aire
5. **Sampling** head/tail documentado (ver [22](22-costo-cardinalidad-sampling.md))

Dynatrace ingiere OTLP; los nombres de entidad pueden no coincidir 1:1 con OneAgent. El Avanzado documenta **cómo se llama el servicio en ambos mundos**.

## Front, workers y batch

- RUM ≠ traza de un cron. Instrumenta workers con entry “messaging” o “scheduled”
- Un batch de 2 h no debe verse como outage de API: **separar servicios** o tagging

## Definition of Done de instrumentación (servicio nuevo)

- [ ] Aparece en Smartscape con dependencias reales
- [ ] Failure rate y p95 tienen sentido (no mezclan health checks)
- [ ] Al menos un atributo de negocio permitido (p. ej. `http.route`, no `user.email`)
- [ ] Logs con trace id en no-prod
- [ ] Dueño y MZ
- [ ] Estimación de volumen de spans/logs

## Criterio Avanzado

Puedes escribir un ADR de una página: *por qué este servicio es OneAgent, OTel o ambos*, y qué **no** se capturará.
