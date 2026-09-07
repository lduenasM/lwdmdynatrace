# 15 — Buenas prácticas y seguridad del dato (Intermedio, obligatorio)

Sin esto no hay promoción a Avanzado: un tenant lleno de PII y tokens es un incidente de seguridad, no “observabilidad madura”.

## Datos que no pertenecen a tickets ni a chats

- Documentos de identidad, cuentas, tarjetas, salud
- Tokens, cookies, `Authorization`, connection strings
- Cuerpos de request/response completos de canales sensibles
- Session replay si la política no lo permite en ese entorno

**Qué sí:** tipo de error, span, URL **sin** querystring sensible, ID interno de correlación (`trace_id`, `orderId` si es permitido).

Si el PurePath muestra un payload: describe el **tipo** de fallo, no el contenido.

## Tokens y credenciales Dynatrace

- Nunca en Git, wikis públicas, chats de LLM **públicos**, ni este repo
- Alcance mínimo, expiración, un token por propósito (ingest ≠ lectura API ≠ Monaco)
- Rotación según política
- Junior/Intermedio: si “necesitas un token”, lo pide plataforma

## Acceso

- SSO, MFA
- Least privilege y **Management Zones**
- Prod: lectura; cambios por pipeline o rol de plataforma
- Cuentas compartidas “dynatrace-equipo” = auditoría rota

## Configuración peligrosa (no la toques)

- Apagar detección global “porque molesta”
- Log ingest `*` de todos los containers en DEBUG
- Captura de argumentos de método con datos de clientes
- RUM sin consentimiento donde la ley lo exige (legal/compliance manda)

## Higiene operativa

- Favoritos y dashboards con **dueño**
- Nombres de entidades/tags estables (`env`, `app`, `team`)
- Runbook junto al dashboard
- No uses prod para “probar DQL pesado”; usa no-prod o ventanas cortas

## Escalamiento ético

Si ves datos personales masivos en logs: **no** los reenvías para “demostrarlo”. Avisas a seguridad/plataforma: *fuente, tipo de dato, volumen aproximado, URL de config si la tienes*.

## Puente al resto de la ruta

Gobierno a escala: [24](24-gobierno-iam-mz-tags.md). Costo que también es riesgo (retenemos de más): [22](22-costo-cardinalidad-sampling.md).
