# Quiz Senior (Master)

≥ 8 / 10. Senior = Master.

1. ¿Por qué un ActiveGate único es un problema arquitectónico?
2. Tags manuales en hosts que rotan cada hora: ¿alternativa?
3. ¿Qué objeto priorizarías en Git (as-code) vs UI?
4. MTTD ¿hasta cuándo se mide?
5. Un CoE que construye todos los dashboards de 40 squads: ¿qué falla?
6. Funnel de negocio con `email` como dimensión: ¿ok?
7. Dos tenants prod/no-prod vs uno con MZ: un riesgo de cada patrón.
8. Game day en prod sin reglas: ¿aceptable?
9. Métrica de programa “número de dashboards”: ¿por qué es vanidad?
10. ¿Se puede firmar Senior en este kit sin el módulo de IA?

## Respuestas

1. SPOF de ingest/sintéticos/cloud según el rol del AG  
2. Labels/rules automáticas (`app`, `env`, `team`)  
3. MZ, SLO tier-1, profiles, detection rules, dashboards oficiales → Git; notebooks efímeros → UI  
4. Hasta que el equipo **sabe** que el incidente es real (no hasta el postmortem)  
5. No escala; el CoE debe ser estándar + plataforma, no factoría de widgets  
6. No  
7. Un tenant: error de permiso ve prod; dos tenants: doble mantenimiento/drift  
8. No  
9. Incentiva paneles basura, no MTTD/cobertura SLO  
10. No (en este kit el cierre IA es obligatorio)  
