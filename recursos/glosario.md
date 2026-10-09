# Glosario

## Process Mining

| Término | Definición |
|---|---|
| **Event Log** | Registro de eventos de un proceso. Cada fila indica qué ocurrió (actividad), a qué instancia le ocurrió (caso) y cuándo (timestamp). |
| **Caso (Case ID)** | Una instancia del proceso: un paciente, un pedido, una solicitud, un incidente, un estudiante en un curso, una orden de trabajo. |
| **Actividad** | Paso o tarea del proceso registrada como evento. |
| **Evento** | Ocurrencia de una actividad, para un caso, en un momento dado. |
| **Recurso** | Persona, equipo, sistema o canal que ejecuta un evento. |
| **Atributo de caso** | Dato que describe al caso completo y no cambia entre sus eventos (por ejemplo, la prioridad o la categoría). |
| **Atributo de evento** | Dato propio de cada evento (por ejemplo, la nota de una entrega o el grupo asignado). |
| **Traza** | Secuencia ordenada de actividades de un caso. |
| **Variante** | Conjunto de casos que siguen exactamente la misma traza. |
| **Prefijo** | Parte inicial de una traza, hasta un momento dado. Ver [fuga_de_informacion.md](fuga_de_informacion.md). |
| **Directly-Follows Graph (DFG)** | Mapa del proceso en el que una flecha A → B indica que B ocurrió inmediatamente después de A en al menos un caso. Puede anotarse con frecuencias o tiempos. |
| **Modelo de proceso** | Representación formal del proceso (red de Petri, árbol de procesos, BPMN) descubierta a partir del log. |
| **Descubrimiento (discovery)** | Construir un modelo de proceso a partir del Event Log. Algoritmos comunes: Alpha Miner, Heuristics Miner, Inductive Miner. |
| **Conformance checking** | Comparar el comportamiento registrado con un modelo o con reglas esperadas para encontrar desviaciones. |
| **Fitness** | Medida de conformance: qué proporción del comportamiento del log puede reproducir el modelo. |
| **Precisión (de un modelo de proceso)** | Medida de conformance: cuánto comportamiento permite el modelo que no aparece en el log. No confundir con la *precision* de clasificación. |
| **Throughput time (lead time)** | Duración total de un caso, desde su primer hasta su último evento relevante. |
| **Tiempo de espera** | Tiempo entre el final de una actividad y el inicio de la siguiente. |
| **Cuello de botella** | Punto del proceso donde se acumulan esperas que alargan la duración de los casos. |
| **Reproceso (rework)** | Repetición de una actividad o grupo de actividades dentro del mismo caso. |
| **Bucle / ciclo** | Patrón en el que el proceso vuelve a una actividad anterior. |
| **Caso incompleto** | Caso que no llegó a una actividad final, normalmente porque seguía abierto a la fecha de extracción. |
| **Filtrado** | Seleccionar casos, actividades o variantes para simplificar o enfocar el análisis. Siempre debe documentarse. |
| **SLA** | *Service Level Agreement*: tiempo máximo comprometido para atender un caso. |

## Machine Learning

| Término | Definición |
|---|---|
| **Variable objetivo** | Lo que el modelo intenta predecir. |
| **Variables predictoras** | Información que el modelo usa para predecir. |
| **Variable derivada** | Variable construida a partir del Event Log (por ejemplo, número de reasignaciones o tiempo hasta el diagnóstico). |
| **Clasificación** | Predecir una categoría (por ejemplo, atrasado / a tiempo). |
| **Regresión** | Predecir un valor numérico (por ejemplo, horas fuera de servicio). |
| **Agrupamiento (clustering)** | Encontrar grupos de casos similares sin una variable objetivo. |
| **Modelo base (baseline)** | Referencia mínima para comparar: por ejemplo, predecir siempre la clase más frecuente o el promedio. Un modelo útil debe superarlo. |
| **Conjunto de entrenamiento / prueba** | Datos usados para ajustar el modelo / datos reservados para evaluarlo. |
| **Validación cruzada** | Repetir el entrenamiento y la evaluación en varias particiones para obtener una estimación más estable. |
| **Sobreajuste (overfitting)** | El modelo memoriza los datos de entrenamiento y rinde mal con datos nuevos. |
| **Desbalance de clases** | Una clase es mucho menos frecuente que otra. Con desbalance, el accuracy puede ser engañoso. |
| **Matriz de confusión** | Tabla que cruza las clases reales con las predichas. |
| **Precision / Recall / F1** | Proporción de aciertos entre los casos predichos como positivos / proporción de positivos reales detectados / media armónica de ambas. |
| **MAE / RMSE / R²** | Error absoluto medio / raíz del error cuadrático medio / proporción de la varianza explicada. |
| **Silhouette** | Medida de qué tan bien separados están los grupos en un agrupamiento (de −1 a 1). |
| **Importancia de variables** | Medida de cuánto contribuye cada predictor a las predicciones del modelo. |
| **Fuga de información (data leakage)** | Usar información no disponible en el momento de predicción. Ver [fuga_de_informacion.md](fuga_de_informacion.md). |
