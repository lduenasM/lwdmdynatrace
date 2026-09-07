# 27 — SRE: incidente, game days, postmortem (Senior / Master)

## Observabilidad al servicio del incidente, no al revés

Senior de observabilidad **diseña** cómo se manda un incidente, no solo cómo se ve un gráfico.

## Comando de incidente (mínimo)

| Rol | Función |
| --- | --- |
| Incident commander | Prioridad, comunicación, no debug profundo |
| Tech lead | Hipótesis, Dynatrace, cambios |
| Comms | Usuarios/negocio |
| Scribe | Timeline |

Dynatrace: **un** Problem canónico + dashboard de journey + notebook de runbook. Evita 12 enlaces contradictorios.

## MTTD / MTTR

Mide con honestidad:

- **MTTD:** primer síntoma (sintético, RUM, usuario, Davis) hasta “sabemos que es real”
- **MTTR:** hasta mitigación, no hasta el postmortem perfecto

La plataforma Senior se evalúa si **bajó** esos números, no si hay más paneles.

## Game days

Ejercicios controlados (fallo de dependencia, kill de pod, saturación) en **no-prod** o con reglas estrictas.

Objetivo de observabilidad:

- ¿Davis abrió el Problem correcto?
- ¿El routing llegó al equipo dueño?
- ¿El SLI se movió?
- ¿El runbook DQL sirvió?

No es un ataque a sistemas ajenos ni “probar exploits”. Es **verificar detección y respuesta**.

## Postmortem

- Timeline con evidencias (URLs Dynatrace, no capturas ilegibles)
- Acción: instrumentación que faltaba, SLO mentiroso, alerta ruidosa, gap de MZ
- Dueños y fechas
- Blameless: se arregla el sistema

## Criterio Senior

Has **facilitado** un game day o un postmortem donde salió un cambio concreto de observabilidad (evento, SLO, tag, as-code), no solo “mejorar comunicación”.
