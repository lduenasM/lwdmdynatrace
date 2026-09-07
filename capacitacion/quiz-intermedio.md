# Quiz Intermedio

≥ 8 / 10.

1. Un Problem de Davis ¿es lo mismo que un log ERROR?
2. ¿Qué haces si Davis culpa a la DB y el PurePath muestra pool agotado?
3. RUM verde y sintético rojo: hipótesis más probable?
4. ¿Por qué no identificar un incidente K8s solo por nombre de pod?
5. Un alerting profile ¿detecta o notifica?
6. Error budget a cero: ¿qué implica de negocio/SRE?
7. DQL `fetch logs` 15 días sin filtro: ¿cuál es el problema?
8. Session replay con datos personales en un ticket público?
9. CPU host alta ¿prueba que tu microservicio es el culpable?
10. Smartscape vacío: da 2 causas posibles.

## Respuestas

1. No  
2. Validar; corregir hipótesis; mirar saturación de hilos/pool  
3. Script/selector/MFA/locación del robot, o flujo no usado por humanos  
4. Los pods rotan  
5. Principalmente **notifica/filtra** Problems (la detección es otra capa)  
6. Se acabó el margen de fallo; reducir riesgo (deploys, etc.)  
7. Costo, lentitud, posible timeout  
8. No  
9. No  
10. Ventana sin tráfico; MZ; falta instrumentación; ambiente equivocado  
