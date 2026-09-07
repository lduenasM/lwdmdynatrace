# 03 — Arquitectura, OneAgent y ActiveGate

## Vista de 30 000 pies

```
[Aplicación / host / pod]
        |  OneAgent (o ingest OTel / API)
        v
[ActiveGate]  (a menudo; no siempre en el dibujo mental)
        |
        v
[Tenant Dynatrace]
        |-- Smartscape (topología)
        |-- PurePath (trazas)
        |-- Métricas / Logs / RUM
        v
[Davis] --> Problems, notificaciones
[Grail] --> DQL, Notebooks, Dashboards nuevos
```

No memorices cada flecha. Memoriza: **el dato nace cerca de la app**, **se centraliza en el tenant**, **Davis lo convierte en Problems**.

## OneAgent

**OneAgent** es el componente que se instala en el sistema operativo (o se inyecta en Kubernetes) y:

1. Descubre procesos y tecnologías
2. Instrumenta automáticamente muchos runtimes (sin tocar código, en stacks soportados)
3. Recoge métricas de host, proceso y servicio
4. Captura trazas (PurePath)
5. Puede recoger logs según configuración
6. Puede inyectar RUM en aplicaciones web (según setup)

### Deep monitoring vs infra only

- **Full-stack / deep monitoring:** APM de verdad (servicios, trazas, código).
- **Infraestructura solamente:** ves el host y procesos, con menos detalle de aplicación.

Si “no hay servicios ni PurePath”, el primer chequeo de plataforma es: ¿el proceso tiene **deep monitoring** habilitado y es una tecnología soportada?

### Qué no hace OneAgent por magia

- Entender reglas de negocio que no están en llamadas (un batch mal diseñado puede verse “verde”).
- Instrumentar un binario custom sin soporte ni OTel.
- Corregir una app mala: solo la hace visible.

## ActiveGate

**ActiveGate** es un componente intermedio. Usos típicos:

- Salida controlada hacia SaaS (proxy, TLS, red corporativa)
- Monitoreo de cloud APIs
- **Sintéticos privados** (el robot corre dentro de la red)
- Kubernetes / OpenTelemetry ingest, según diseño

Como junior no instalas ActiveGates. Sí debes saber que “no llegan datos” a veces es **red / ActiveGate / firewall**, no la aplicación.

## Entidades y IDs

Cada host, proceso, servicio, etc. tiene un identificador. Los enlaces de Dynatrace llevan ese ID. **Copia siempre la URL del Problem o de la entidad** en el ticket: es más útil que una captura recortada.

## Smartscape

Smartscape es el **mapa de dependencias** (quién habla con quién) construido a partir de lo observado, no de un Visio. Cambia con deploys y tráfico.

Úsalo para:

- Ver si un servicio llama a una DB o a un HTTP externo
- Entender blast radius (“si cae X, ¿quién depende?”)

No lo uses como CMDB legal: es topología **observada**.

## Grail (tenants modernos)

**Grail** es el almacén unificado (métricas, logs, eventos, a veces trazas/biz) consultable con **DQL**. La UI nueva (Apps, Notebooks, Dashboards) vive sobre Grail.

Puedes convivir un tiempo con:

- UI **classic** (Data Explorer, Log viewer clásico, Dashboards clásicos)
- UI **Grail** (DQL)

Pregunta a tu buddy: “¿nuestro tenant ya usa Grail para logs?”. La respuesta cambia el menú del día 2.

## Tokens y API (solo concepto)

Hay tokens para ingest, API v1/v2, Platform tokens, etc. **Nunca** los pegas en código ni en este repo. Si un ejercicio pide API, usa un token de **no-prod** con el mínimo scope y revócalo al terminar, según política de la empresa.

## Fallos típicos de “no veo datos”

1. OneAgent no instalado o no en la imagen/pod
2. Deep monitoring off o tecnología no soportada
3. Management Zone: el servicio existe pero **tú no lo ves**
4. Reloj / filtro de tiempo mal puesto (último 30 min vs incidente de ayer)
5. Entorno equivocado (miras QA y el incidente es PROD)
6. Sampling o capturas limitadas en picos (pregunta a plataforma)

Tu mensaje de escalamiento debe incluir: **entorno, nombre del servicio/host, ventana de tiempo, URL de Dynatrace**.
