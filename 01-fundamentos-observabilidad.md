# 01 — Fundamentos de observabilidad

## Qué problema resuelve

En un sistema moderno no basta con “el servidor está encendido”. Hay decenas de servicios, colas, bases de datos y llamadas HTTP. Cuando algo falla, necesitas **preguntarle al sistema** qué pasó, no adivinar.

**Observabilidad** es la capacidad de inferir el estado interno del sistema a partir de sus salidas: métricas, logs y trazas (y, en la práctica, también eventos y perfiles).

**Monitoreo** clásico es más estrecho: vigilas umbrales que ya definiste (CPU > 80 %). La observabilidad te permite investigar **preguntas nuevas** (¿por qué este usuario tarda 12 s solo los martes?).

## Los tres pilares (más uno)

### 1. Métricas

Números agregados en el tiempo: CPU, memoria, requests por segundo, latencia p95, tasa de error.

- **Útiles para:** tendencias, dashboards, SLI.
- **Limitación:** no te dicen *qué* request falló ni el payload.

Ejemplo: `http_server_requests_seconds{status="500"}` sube. Sabes que hay errores, no sabes el `trace_id`.

### 2. Logs

Líneas de texto (o JSON) que la aplicación escribe: errores, auditoría, mensajes de negocio.

- **Útiles para:** detalle, excepciones, correlacionar con un usuario o un `orderId`.
- **Limitación:** volumen alto, caro, fácil de filtrar mal; sin correlación son un pajar.

### 3. Trazas (distributed tracing)

Una **traza** es el recorrido de una petición a través de varios servicios. Cada salto es un **span**.

- **Útiles para:** “¿dónde se fue el tiempo?”, “¿quién llamó a quién?”
- **Limitación:** hay que instrumentar (o usar un agente que lo haga por ti).

En Dynatrace el modelo de traza de punta a punta se llama **PurePath**.

### 4. Experiencia de usuario y sintéticos

- **RUM (Real User Monitoring):** lo que viven navegadores y apps reales.
- **Sintéticos:** robots que recorren un flujo (login, pago) cada X minutos desde una ubicación.

Sirven para detectar “está caído para el usuario” aunque el servidor “esté verde”.

## Señales que un junior debe nombrar bien

| Señal | Pregunta que responde |
| --- | --- |
| Disponibilidad | ¿El servicio responde? |
| Tráfico | ¿Cuánta carga hay? |
| Errores | ¿Cuántas peticiones fallan? |
| Latencia | ¿Cuánto tarda (p50, p95, p99)? |
| Saturación | ¿Se está quedando sin CPU, memoria, hilos, conexiones? |

Este conjunto se parece al modelo **USE** (Utilization, Saturation, Errors) para recursos y **RED** (Rate, Errors, Duration) para servicios.

## SLI, SLO y error budget (versión junior)

- **SLI (Service Level Indicator):** la métrica que importa. Ejemplo: porcentaje de requests HTTP con status 2xx/3xx en menos de 2 s.
- **SLO (Service Level Objective):** el objetivo. Ejemplo: 99,5 % de esas requests en un mes.
- **Error budget:** lo que te “queda” para fallar. Si el SLO es 99,5 %, puedes fallar el 0,5 %. Cuando el presupuesto se acaba, se frena el riesgo (menos deploys agresivos).

No inventes SLO de negocio tú solo. El equipo de producto/plataforma define el número; tú aprendes a **leerlo** en Dynatrace.

## Síntoma vs causa vs impacto

| Concepto | Ejemplo |
| --- | --- |
| Síntoma | CPU al 95 % en un pod |
| Causa | Loop infinito tras un deploy, o N+1 a la base de datos |
| Impacto | Checkout con timeout; usuarios no compran |

Davis (en Dynatrace) intenta agrupar síntomas y apuntar a una **causa raíz**. Tú igual debes validar con trazas y logs: la IA ayuda, no sustituye el criterio.

## Qué no es observabilidad

- Un PDF de arquitectura desactualizado.
- “Ping al host” como única prueba.
- Guardar todos los logs para siempre “por si acaso” (caro e inútil si no hay retención ni índices pensados).
- Alertar por cada métrica que se mueve (fatiga de alertas).

## Mini ejercicio mental

Un usuario dice: “la app está lenta”.

Orden sano de pensamiento:

1. ¿Es un usuario, un país, un navegador, un flujo (login vs reporte)?
2. ¿Hay un **Problem** abierto en Dynatrace?
3. ¿El **servicio** tiene error rate o response time anómalo?
4. ¿Un **PurePath** muestra el span lento (DB, HTTP externo, CPU)?
5. ¿El **log** de esa traza confirma la excepción?

Ese orden es el hilo de casi toda esta capacitación.
