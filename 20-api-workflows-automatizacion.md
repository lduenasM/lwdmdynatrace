# 20 — API, Workflows y automatización (Avanzado)

## Para qué automatizar

- Abrir ticket ITSM con URL del Problem y MZ correcta
- Anotar deploys como eventos
- Auto-remediation **muy acotada** (scale, restart) con dueño y kill switch
- Extraer SLI a un almacén interno (si legal lo permite)

No automatizar: “cerrar todos los Problems”, “apagar Davis”, “borrar logs”.

## Tokens y seguridad

- Un propósito, un token o OAuth; scopes mínimos
- Secretos en el vault de CI, no en notebooks
- Entornos: token de **no-prod** para aprender
- Rotación y auditoría: quién llamó qué

Si un ejercicio pide API, usa sandbox. Prod solo por pipeline revisado.

## Superficies típicas

- **API v2** de entidades, métricas, problems, settings (según licencia)
- **Platform / Grail** query API para DQL
- **Workflows / AppEngine automation:** Problem → notificación enriquecida → canal
- **Event ingest** desde GitHub Actions / Azure DevOps

La referencia de endpoints cambia: [docs.dynatrace.com](https://docs.dynatrace.com). Este kit no sustituye el catálogo de tu versión.

## Workflow de incidente (patrón sano)

```
Problem abierto
  → filtrar por tag team + severidad
  → extraer entidades y RCA (texto)
  → postear a Slack/Teams con enlace (sin payloads)
  → crear incidente ITSM si burn SLO o impacto user
  → (opcional) runbook: “¿es el sintético o RUM?”
```

Remediation automática solo si:

- Es **idempotente**
- Tiene **límite de veces**
- Queda **trazada** (quién/qué)
- Se puede **desactivar** en 1 clic

## Anti-patrones

- Bot que pagina a 50 personas a las 3 a.m. por un host de lab
- Scripts locales con token `ReadConfig` + `Write` en el laptop del junior
- Loops que consultan Grail cada 10 s sin agregación

## Criterio Avanzado

Una automatización en **no-prod** revisada por un par, con secretos sanos y un párrafo de “qué pasa si falla el workflow”.
