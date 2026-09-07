# 21 — Diseño de SLI, SLO y error budget (Avanzado)

## SLO es un contrato, no un gráfico verde

Producto + ingeniería acuerdan **qué significa “suficientemente bueno”**. Dynatrace **mide**; no inventa el número.

## Elegir el SLI (calidad)

Un SLI bueno:

- Se acerca al **journey** (pagar, firmar, consultar saldo), no a “CPU del pod”
- Es una **proporción** de eventos buenos / totales (o equivalente)
- Tiene **exclusiones** conscientes (health, probes, bots si aplica)
- Es **observable** hoy (métrica o DQL), no un deseo

Ejemplos:

- Fracción de requests `checkout-api` `http.route=/pay` con status 2xx/3xx y duración &lt; 2 s
- Fracción de ejecuciones sintéticas del clickpath “login+saldo” OK
- Fracción de sesiones RUM del user action “Confirmar” sin error JS y LCP bajo umbral — **solo si** sampling y privacidad están OK

Malos SLI: CPU &lt; 70 %; “el dashboard se ve verde”; disponibilidad del host.

## Objetivo y ventana

- 99,9 % no es “más profesional” que 99,5 %: es **10× menos presupuesto de error**
- Ventana rolling 28–30 días es común; alinea con el ciclo de producto
- Distintos journeys → distintos SLO (login puede ser más estricto que un reporte)

## Error budget y burn

- Presupuesto = 1 − objetivo
- **Burn rápido** (se acaba en horas): incidente, congelar riesgo
- **Burn lento:** deuda, no war room

Alertar burn es a menudo mejor que alertar un pico de 1 minuto que el SLO mensual absorbe.

## Multi-SLO y política de deploy

Senior/SRE: si el presupuesto está en rojo, **menos** deploys agresivos. El Avanzado **propone** la política; no la impone solo.

## Implementación en Dynatrace

- SLO nativos y/o DQL
- Dashboard de presupuesto + enlace al runbook
- Un dueño (equipo), no “todos”
- Revisión trimestral: ¿el SLI aún mide el dolor del usuario?

## Criterio Avanzado

Entregas una **propuesta de una página** (SLI, objetivo, exclusiones, cómo se mide, qué pasa si se quema) para **un** servicio real, lista para review de producto.
