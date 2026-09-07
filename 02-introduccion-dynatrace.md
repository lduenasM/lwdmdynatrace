# 02 — Introducción a Dynatrace

## Qué es

**Dynatrace** es una plataforma de observabilidad y APM (Application Performance Monitoring). Instrumenta hosts y aplicaciones (sobre todo con **OneAgent**), construye un mapa de dependencias (**Smartscape**), sigue peticiones (**PurePath**) y usa **Davis AI** para abrir **Problems** con causa raíz probable, en lugar de mil alertas sueltas.

También cubre:

- Infraestructura (hosts, procesos, discos, red)
- Aplicaciones (servicios, colas, bases de datos)
- Kubernetes / cloud
- RUM y monitoreo sintético
- Logs y, en tenants modernos, el data lakehouse **Grail** con lenguaje **DQL**
- Seguridad de aplicaciones (módulo adicional; no es el foco de este kit junior)

## Qué problema de negocio ataca

| Antes (típico) | Con una plataforma tipo Dynatrace |
| --- | --- |
| “Reinicia el servidor” | Ves qué servicio y qué llamada fallan |
| Cada equipo tiene su Grafana distinto | Un mapa común de entidades |
| Alertas de CPU a las 3 a.m. sin impacto | Problems ligados a impacto en servicio/usuario |
| War room de 2 horas para encontrar el culpable | PurePath + Davis acortan el tiempo de investigación |

## SaaS vs Managed (Managed / on-prem)

- **SaaS:** Dynatrace opera el cluster; tú entras a un **tenant** (URL tipo `https://<ambiente>.apps.dynatrace.com` o el dominio clásico `*.live.dynatrace.com`). Menos operación del backend.
- **Managed:** el cluster corre en infraestructura de la empresa. Hay más control y más trabajo de plataforma.

Como junior te importa: **qué URL es la de tu empresa**, **SSO**, y **en qué tenant** está no-producción vs producción. Nunca mezcles capturas de prod en tickets públicos.

## Conceptos de producto que oirás todo el día

- **Environment / tenant:** tu “cuenta” de datos.
- **OneAgent:** agente en el host (o equivalente en K8s) que descubre e instrumenta.
- **ActiveGate:** proxy/puente para tráfico hacia Dynatrace, cloud, o módulos (p. ej. sintéticos privados).
- **Entity:** host, proceso, servicio, aplicación, Kubernetes workload, etc., con un ID.
- **Problem:** incidente detectado por Davis (no es lo mismo que “un log de error”).
- **Management Zone:** recorte de visibilidad (por app, por equipo). Es normal que un junior no vea todo el tenant.
- **Davis:** motor de IA que correlaciona eventos y propone RCA.

## APM vs observabilidad “de métricas”

Herramientas solo de métricas te dicen *cuánto*. Dynatrace, como APM, intenta decirte *quién llamó a quién* y *en qué método se fue el tiempo*. Por eso OneAgent inyecta instrumentación en runtimes (Java, .NET, Node, Go en muchos casos, etc.).

## OpenTelemetry

Dynatrace puede **ingerir OpenTelemetry** (trazas, métricas, a veces logs). En algunas apps nuevas el estándar es OTel; OneAgent sigue siendo el camino más automático en stacks soportados.

Como junior: si te dicen “esta app es OTel”, igual buscas el **servicio** y el **trace id**. El menú puede ser el mismo o pasar por Apps de Grail.

## Qué se espera de un junior (y qué no)

**Sí:**

- Entrar con SSO, respetar Management Zones
- Buscar un host/servicio/aplicación
- Leer un dashboard del equipo
- Abrir un Problem y copiar el enlace en el incidente
- Seguir un PurePath y anotar el span lento
- Buscar logs con filtro de tiempo y servicio

**No (sin autorización):**

- Instalar o actualizar OneAgent en producción
- Cambiar umbrales globales o apagar detección de anomalías
- Borrar dashboards compartidos
- Subir tokens de API a un repo o al chat
- Exportar datos personales de usuarios (RUM) fuera de canales oficiales

## Dónde está la verdad oficial

La UI cambia. La fuente de verdad de producto es la documentación de Dynatrace:

- Documentación: [https://docs.dynatrace.com](https://docs.dynatrace.com)

Este kit enseña **hábitos** y **flujo de trabajo**. Si un botón cambió de nombre, usa la búsqueda del tenant y confirma en docs.
