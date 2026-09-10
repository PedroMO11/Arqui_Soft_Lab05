# Arqui_Soft_Lab05

- RAG / Re training periodico / 

También hay múltiples quejas de que el LLM no está aprendiendo y 
Reverificador de respuestas o finetunear el modelo antes de la empresa. Tal vez la LLM local no ha sido entrenada correctamente. Periodicamente uso de re training con new data, pero mantener memoria de conversaciones recientes puede ser respaldo de informacion.

Al mismo tiempo, soporte se queja de que Genius a veces responde con el estado de un incidente que ya fue cerrado hace horas.
(Filtro determinista. Force llm bring specific format to filter date.)

Requirement
L - Diagrama
Escalar

-------------------

Y tenemos un conjunto de preguntas comunes que cada día el LLM responde en forma diferente.  
Parser -> conjunto de preguntas comunes -> (cache?)

El día con más incidentes es la primera semana de cada mes donde tenemos que el
LLM tiende a no responder (Carga de trabajo por eso falla? Load balance puede solucionar? Priority queue soluciona!) Too many requests response if queue fills.

O responde en forma errada. siempre da respuestas erróneas o de data pasada.
X data no actualizada (no puede suceder en common requests, database)
straight up la llm no sabe la respuesta
halucinar (obtiene algo o no y pero empieza a inventar)

- Verificador de confianza

El último mes hubo un problema en el que un ingeniero de soporte dio instrucciones
para una escalamiento de un cliente en la cual el LLM decidió borrar la base de datos.
(Establecer tipos de permisos que la LLM puede ejecutar autonomamente y no.)
