# 04 — Primeros pasos en la UI

## Antes de entrar

1. Confirma la **URL del tenant** (prod vs no-prod).
2. Entra con **SSO** corporativo. No compartas sesión.
3. Si no ves aplicaciones que “deberían existir”, asume **Management Zone** o entorno incorrecto antes de asumir que “Dynatrace está caído”.

## Búsqueda global

El atajo más importante. Busca:

- Nombre del microservicio
- Hostname / nombre de pod
- Nombre de dashboard
- ID de Problem (`P-XXXX`)
- A veces un `trace` / `PurePath` id si lo tienes

Si no encuentras un menú, **búscala**. La UI nueva reorganiza items bajo **Apps** y **Launchpad**.

## Mapa mental de la consola

| Área | Para qué |
| --- | --- |
| **Problems** | Incidentes detectados por Davis. Empieza aquí en un incidente. |
| **Applications / Frontend** | RUM: browsers, apps móviles. |
| **Services** | Backend: rates, errors, response time, PurePaths. |
| **Hosts / Infrastructure** | CPU, memoria, disco, procesos. |
| **Kubernetes** | Cluster, namespace, workload, pod. |
| **Logs** | Búsqueda de logs (classic o Grail). |
| **Dashboards** | Vistas del equipo. |
| **Explore / Notebooks / DQL** | Consultas ad-hoc (Grail). |
| **Synthetic** | Monitores robot. |
| **Settings** | No toques en prod. En no-prod, solo con buddy. |

Los nombres exactos dependen de la versión. El **flujo** es estable: Problem → entidad afectada → métricas/trazas/logs.

## Filtros de tiempo

Errores de junior más comunes:

- Dejar “últimos 2 horas” cuando el incidente fue de madrugada.
- Comparar “hoy” contra un deploy de la semana pasada sin ampliar rango.
- Olvidar la **zona horaria** (UTC vs hora local).

Hábitos:

- En un incidente, fija **desde 15 min antes del primer síntoma** hasta “ahora” (o hasta el cierre).
- Usa **comparar con** (día anterior / semana anterior) si la UI lo ofrece, para ver si “lento” es el baseline.

## Management Zones

Una Management Zone recorta **qué entidades ves** (y a veces qué puedes cambiar).

Checklist si “falta todo”:

1. Selector de zona (arriba): ¿estás en la zona de tu tribu/app?
2. ¿Pediste acceso al grupo correcto?
3. ¿El nombre del servicio en Dynatrace coincide con el de Kubernetes/YAML? (a menudo hay sufijos `-prod`, process groups, etc.)

## Favoritos y dashboards del equipo

El primer día, pide:

- Dashboard de **salud de la aplicación**
- Dashboard de **SLO** si existe
- Lista de **servicios críticos** (nombres canónicos)
- Canal de incidentes y plantilla de handover

Guarda en favoritos **solo** lo que uses. No clones 40 dashboards “por si acaso”.

## Cómo no perderte: el método de 4 clics

1. **Problems** (¿hay algo abierto?).
2. Si no: **Services** → tu servicio → pestaña de **Failure rate / Response time**.
3. **View PurePaths** / Distributed traces → ordenar por duración.
4. Desde el span: **logs correlacionados** o host/proceso.

Ese circuito cubre el 80 % del trabajo L1.

## Etiquetas y propiedades

Hosts y servicios tienen **tags** (manuales, reglas, o de cloud: `environment`, `app`, `team`). Filtrar por tag es más fiable que memorizar hostnames.

Si tu empresa etiqueta `env:prod` y `tribe:pagos`, úsalo siempre en búsquedas y al pedir acceso.

## Capturas para tickets

Incluye:

- URL completa
- Rango de tiempo visible
- Nombre de la entidad
- Qué estabas filtrando

Evita datos personales (emails, cuentas, payloads). Si el PurePath muestra un documento de identidad, **no** lo pegues en Jira público: describe el span y el error genérico.
