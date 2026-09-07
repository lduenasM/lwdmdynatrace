# Quiz Avanzado

≥ 8 / 10.

1. ¿Cuándo elegirías OTel además de (o en vez de) OneAgent?
2. Una dimensión `userId` en una métrica de rate: ¿qué riesgo?
3. Head sampling al 1 %: ¿qué puede pasar con un error raro?
4. ¿Por qué un metric event de “CPU > 80 % todos los hosts” es mala idea?
5. ¿Qué debe llevar un ADR de instrumentación?
6. Token API en un notebook compartido?
7. SLI de “CPU &lt; 70 %” para checkout: ¿por qué es débil?
8. Health probes mezclados en el servicio APM: ¿efecto?
9. Auto-restart ilimitado vía Workflow ante cualquier Problem?
10. ¿Logs con body de pago en Grail 90 días?

## Respuestas

1. Runtime no cubierto, estándar de equipo, functions/jobs, evitar lock-in, etc.  
2. Cardinalidad explosiva / costo / UI inútil  
3. Puede no guardarse la traza  
4. Fatiga, poco impacto de usuario, no escala  
5. Decisión OneAgent/OTel/híbrido, qué no se captura, dueño, volumen  
6. No  
7. No mide el journey del usuario  
8. Distorsiona throughput y a veces errores  
9. No (amplificación, sin kill switch)  
10. No (PII, costo, legal)  
