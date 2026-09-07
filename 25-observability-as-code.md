# 25 — Observability as code (Monaco, Terraform) (Senior / Master)

## Por qué as-code

La UI no escala: 80 dashboards “a mano”, alerting distinto por squad, irreproducible. **Configuración en Git** = review, historial, ambientes.

## Herramientas típicas

- **Dynatrace Monaco (Monitoring as Code):** configs del tenant (dashboards, SLO, MZ, settings…)
- **Terraform provider Dynatrace:** infra-as-code alineado al resto de la nube
- A veces ambos: Terraform para “cimientos”, Monaco para configs densas — **decide un patrón** y no dupliques el mismo objeto

Los recursos exactos cambian con la API de Settings 2.0. La fuente es la documentación actual del vendor y el schema de tu tenant.

## Principios

1. **No-prod primero** (tenant o MZ de lab)
2. Un objeto, **una** fuente de verdad (no editar en UI y en Git)
3. PRs con impacto: “esto pagina a pagos-prod”
4. Secretos fuera del repo
5. Plan/apply en CI con rol de servicio
6. Drift: si alguien cambia en UI, o se pisa o se detecta; política explícita

## Qué sí va a Git

- MZ, tagging rules, alerting profiles (o su esqueleto)
- SLO de tier-1
- Dashboards **oficiales** de squad
- Metric events pactados
- Detection rules estables

## Qué puede quedar en UI (con dueño)

- Notebooks de investigación efímeros
- Dashboards personales
- Experimentos con caducidad

## Onboarding de un squad

Plantilla: repo `obs-as-code` o carpeta por `app/` con README de “cómo aplicar”. El Senior no es el cuello de botella de cada widget: habilita **guardrails**.

## Criterio Senior

Al menos **un cambio real** (aunque sea no-prod) por PR mergeado: no un zip de configs en un correo.
