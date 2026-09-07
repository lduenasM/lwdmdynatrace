# Quiz IA (cierre)

≥ 8 / 10.

1. Davis vs Copilot del tenant: ¿cuál abre Problems correlacionando topología?
2. ¿Puedes ejecutar DQL de Copilot sin leerla?
3. ¿Qué no va a un LLM público?
4. Un modelo inventa una métrica `dt.service.magic.latency`: ¿qué haces?
5. ¿La IA generativa mide si la respuesta de un chatbot es “verdadera”?
6. Junior: ¿ChatGPT con logs de prod para “ahorrar tiempo”?
7. Query generada de 14 días de todos los logs: ¿riesgo principal además de PII?
8. ¿Quién define herramientas de IA aprobadas?
9. Tokens in/out como métrica vs loguear el prompt completo: ¿cuál es más sano?
10. “La IA dijo rollback”: ¿es suficiente para ejecutar en prod?

## Respuestas

1. Davis  
2. No  
3. Tokens, PII, payloads, cookies, dumps prod, session replay  
4. Verificar en el catalog del tenant; no confiar  
5. No (hace falta eval de producto; APM ve latencia/errores de integración)  
6. No  
7. Costo / saturación de Grail  
8. CoE / Senior + seguridad/legal  
9. Métrica agregada de tokens (si el contrato lo permite); no el prompt  
10. No: humano verifica señales y runbook  
