# 12 — RUM y monitoreo sintético (Intermedio)

## Dos formas de ver al usuario

| | **RUM** (Real User Monitoring) | **Sintético** |
| --- | --- | --- |
| Quién | Usuarios reales (browser/app) | Robot programado |
| Cubre | Dispositivos, redes, geografía, JS, terceros | El flujo que escribiste, 24/7, desde ubicaciones |
| No cubre | Lo que nadie usa a las 3 a.m. | La diversidad real (IE interno, 3G, adblock) |
| Riesgo | PII, consent, sampling | Falsos positivos (bot bloqueado, MFA) |

En un incidente de “la web está lenta”, mira **ambos**. Backend verde + RUM rojo = front, CDN, terceros o red del cliente. RUM verde + sintético rojo = a veces ubicación del robot o un paso del script.

## RUM — qué mirar

- **Load / Core Web Vitals** (LCP, INP, CLS) si están habilitados
- **XHR/fetch** hacia tus APIs (enlace a servicio / PurePath)
- **Errores JavaScript**
- **Sesiones de usuario** (con cuidado de PII)
- **Aplicación vs user action** (un “click Pagar” no es lo mismo que GET `/`)

Correlación: de una acción lenta pasas al **servicio** y al **PurePath**. Esa es la joya del full-stack.

### Límites que un Intermedio debe respetar

- Session replay / detalles de sesión pueden contener datos personales. **Política de la empresa primero.**
- No exportes listados de usuarios a Excel “para analizar”.
- Sampling: no todas las sesiones están. Una queja puntual puede no aparecer.

## Sintéticos — tipos

- **HTTP monitor:** ping de URL/API (barato, poco fiel al usuario)
- **Browser monitor:** clickpath (login, buscar, pagar)
- **Terceros / API** con validación de contenido y SSL
- **Privados (ActiveGate):** el robot está **dentro** de la red (apps internas)

### Cómo investigar un sintético rojo

1. ¿Falló la locación (un POP) o todas?
2. ¿Timeout, HTTP 5xx, selector CSS, MFA, captcha?
3. Compara con RUM y con el servicio backend en la misma ventana
4. ¿Cambio de UI que rompió el script? (falso positivo clásico)

Los sintéticos son **contratos de recorrido**, no la realidad completa. Un Master los trata como SLI de disponibilidad de journey, con dueño que mantiene el script.

## Primera línea vs ingeniería

- Intermedio: interpreta, enlaza a backend, dice si es script o outage
- Avanzado: define frecuencia, ubicaciones, umbrales, datos sintéticos de test
- Master: cubre journeys de negocio, no 200 URLs huérfanas

## Criterio de nivel

Puedes contar una historia: **usuario / robot → acción → API → span**. Si solo miras CPU del pod, aún eres Junior en este tema.
