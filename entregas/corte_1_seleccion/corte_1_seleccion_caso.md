**Universidad de Costa Rica**

**Sede del Sur**

Informática Aplicada a los Negocios IF6201

EXAMEN PRÁCTICO I

**Estudiantes**

Meylin Iliana López Solera; C4G662 

Yileidy Rivera Granados; C4I972

**Profesor**

Juan Gamboa Abarca

II Semestre, año 2026

# Resolución del examen
## 2. Revisión de los casos

| Caso | Problema principal identificado | ¿Permite Process Mining? ¿Por qué? | Posible uso posterior de ML | Nivel de interés para la pareja: Alto/Medio/Bajo			
|---|---|---|---|---|
| 01. Salud | Tiempos de espera prolongados y diferencias en los recorridos de atención de los pacientes. | Sí, porque permite reconstruir las actividades realizadas durante la atención, identificar retrasos y comparar recorridos. | Predecir si un paciente tendrá un tiempo de atención prolongado. | Medio. |
| 02. Pedidos | Retrasos durante la preparación, despacho y entrega de pedidos. | Sí, porque cada pedido pasa por una secuencia de actividades que permite identificar variantes y cuellos de botella. | Predecir si un pedido será entregado con retraso. | Bajo. |
| 03. Créditos | Algunas solicitudes requieren múltiples revisiones y presentan tiempos de procesamiento elevados. | Sí, porque permite reconstruir el recorrido de cada solicitud, detectar revisiones repetidas y analizar los tiempos entre actividades. | Predecir la aprobación o el rechazo de una solicitud de crédito. | Alto. |
| 04. Soporte TI | Incidentes que requieren múltiples reasignaciones o escalamientos antes de resolverse. | Sí, porque permite analizar las actividades realizadas desde el registro hasta la resolución de cada incidente. | Predecir si un incidente superará el tiempo esperado de resolución. | Medio. |
| 05. Académico | Diferencias en la participación y el recorrido académico de los estudiantes que podrían relacionarse con resultados desfavorables. | Sí, porque permite estudiar las secuencias de actividades académicas registradas y comparar diferentes recorridos. | Predecir el riesgo de reprobación o abandono académico. | Medio. |
| 06. Mantenimiento | Reparaciones que requieren múltiples intervenciones y mantienen equipos fuera de servicio durante periodos prolongados. | Sí, porque permite estudiar las actividades de mantenimiento, sus repeticiones y los tiempos de ejecución. | Predecir si una reparación superará el tiempo esperado. | Bajo. |

## 3. Caso seleccionado
**Número y nombre del caso:** Caso 03 – Solicitud y aprobación de créditos.

**Archivo de datos:** datos/raw/caso_03_creditos.csv 

## 4. Justificación de la selección

Seleccionamos el caso 03, relacionado con la solicitud y aprobación de créditos, porque consideramos que permite estudiar un proceso que incluye diferentes actividades de revisión, evaluación y toma de decisiones. Nos interesa especialmente identificar las rutas que siguen las solicitudes, determinar si existen actividades repetidas y analizar las posibles diferencias en los tiempos de procesamiento.

Además, este caso permite relacionar la minería de procesos con una posible aplicación futura de Machine Learning, mediante la predicción de aprobación o rechazo de las solicitudes a partir de sus características económicas y laborales. Por estas razones, consideramos que es un caso adecuado para aplicar los conocimientos del curso y comprender cómo el análisis de datos puede apoyar la toma de decisiones en una institución financiera.

## 5. Comprensión inicial del proceso
**5.1 ¿Cuál considera que es el inicio del proceso?**

Consideramos que el proceso inicia con una "Solicitud recibida", cuando la persona presenta una solicitud de crédito ante la institución financiera. En esta etapa se registra la información inicial del solicitante, como el monto solicitado, el tipo de crédito, los ingresos y otros datos necesarios para comenzar la evaluación. A partir de este momento, la solicitud pasa por diferentes actividades de revisión y análisis.

**5.2 ¿Cuál considera que es el final del proceso?**

Consideramos que el proceso puede finalizar de dos maneras principales: la primera ocurre cuando la solicitud es rechazada, por lo que el trámite concluye sin otorgar el crédito, mientras que, la segunda ocurre cuando la solicitud es aprobada y posteriormente pasa por la etapa de formalización, donde se completan los procedimientos necesarios para otorgarlo. Por lo tanto, el proceso puede finalizar con un rechazo o con la formalización de un crédito aprobado.

**5.3 ¿Qué actividades principales espera encontrar?**

Se espera encontrar las siguientes actividades:

**A. Solicitud recibida:** registro inicial de la solicitud de crédito.

**B. Verificación de documentos:** revisión de la documentación presentada por el solicitante.

**C. Análisis financiero:** evaluación de la capacidad económica y de pago del solicitante.

**D. Solicitud de información adicional:** petición de documentos o información faltante cuando sea necesario.

**E. Reevaluación:** nueva revisión de la solicitud después de recibir información adicional.

**F. Aprobación:** decisión favorable sobre la solicitud de crédito.

**G. Rechazo:** decisión desfavorable sobre la solicitud.

**H. Formalización:** realización de los trámites finales de un crédito aprobado.


**5.4 Componentes iniciales del Event Log**

| Componente | Variable o propuesta | Justificación |
|---|---|---|
| Case ID | case_id | Permite identificar cada solicitud de crédito y agrupar todos los eventos que corresponden a ella. |
| Activity | act | Permite conocer las actividades realizadas durante el trámite, como verificación de documentos, análisis financiero, aprobación o rechazo. | 
| Timestamp | tstamp | Registra la fecha y hora de los eventos, lo que permite ordenar las actividades y analizar los tiempos del proceso. | 
| Recurso, si aplica | resource | Permite identificar al responsable de realizar cada actividad, cuando esta información se encuentre disponible.
| Otros atributos | Monto solicitado, ingresos, antigüedad laboral, nivel de endeudamiento y tipo de crédito. | Permiten analizar posibles relaciones entre las características de las solicitudes, sus recorridos y los resultados del proceso. | 

## 6. Preguntas iniciales para Process Mining

**Pregunta 1**
¿Cuáles son las rutas más frecuentes que siguen las solicitudes de crédito aprobadas y rechazadas, y qué diferencias existen entre ellas?

**Pregunta 2**
¿Existen diferencias en el recorrido de las solicitudes de crédito según el tipo de crédito o el monto solicitado?

**Pregunta 3** 
¿Con qué frecuencia las solicitudes requieren información adicional o reevaluaciones y cómo influyen estas actividades en el tiempo total de procesamiento?


## 7. Hipótesis o sospechas iniciales

Consideramos que algunas solicitudes de crédito podrían presentar retrasos debido a la necesidad de solicitar información adicional o realizar múltiples revisiones. Esto podría provocar que ciertas solicitudes regresen a etapas anteriores, generando reprocesos y aumentando el tiempo necesario para obtener una decisión. También, que las solicitudes de montos elevados o aquellas que requieren evaluaciones especiales podrían tardar más tiempo, debido a que deben pasar por revisiones adicionales. Además, esperamos encontrar diferentes rutas según el resultado de cada solicitud. Algunas podrían seguir una secuencia directa hasta su aprobación o rechazo, mientras que otras podrían necesitar información adicional y reevaluaciones.

## 8. Posible problema futuro de Machine Learning

**Pregunta propuesta:**

¿Es posible predecir si una solicitud de crédito será aprobada o rechazada a partir de las características económicas y laborales del solicitante y del tipo de crédito solicitado?

**Posible variable objetivo**

La posible variable objetivo sería el resultado final de la solicitud de crédito, representado mediante dos categorías:

**Aprobado:** cuando la institución acepta la solicitud de crédito.

**Rechazado:** cuando la institución determina que la solicitud no cumple con los criterios necesarios para su aprobación.

Esta variable permitiría identificar el resultado que se desea predecir a partir de las características de cada solicitud. Las solicitudes que todavía no tengan una decisión final deberán identificarse y tratarse de forma separada.

**Posibles variables predictoras**

Las posibles variables predictoras serían:

**A. Tipo de crédito:** permite distinguir las diferentes categorías de crédito, que podrían tener requisitos de evaluación distintos.

**B. Monto solicitado:** representa la cantidad de dinero que solicita el cliente.

**C. Ingresos:** proporciona información sobre la capacidad económica declarada del solicitante.

**D. Nivel de endeudamiento:** permite conocer qué proporción de los ingresos se encuentra comprometida en otras deudas.

**E. Antigüedad laboral:** permite conocer cuánto tiempo lleva el solicitante en su empleo actual.

**F. Canal de solicitud, si está disponible:** identifica el medio mediante el cual ingresó la solicitud.

**Tipo de problema que podría resultar**

- [x] Clasificación
- [ ] Regresión
- [ ] Agrupamiento
- [ ] Aún no se puede determinar

**Justificación**

Consideramos que este problema corresponde a una tarea de clasificación porque se busca anticipar un resultado que pertenece a dos categorías: aprobación o rechazo de una solicitud de crédito. A diferencia de un problema de regresión, donde se pretende estimar un valor numérico, en este caso el objetivo consiste en determinar a cuál de las dos categorías podría pertenecer cada solicitud. Las variables propuestas contienen información sobre las características económicas y laborales de los solicitantes, las cuales podrían estar relacionadas con las decisiones tomadas por la institución financiera. Mediante Machine Learning se podría investigar si existen patrones en estos datos que permitan anticipar el resultado de una solicitud.

## 9. Decisión final de la pareja 

Después de revisar los seis casos propuestos, decidimos trabajar con el caso 03, solicitud y aprobación de créditos, debido a nuestro interés en comprender las actividades que forman parte de este proceso y los posibles factores relacionados con sus resultados.

Consideramos que, mediante Process Mining, podremos identificar las rutas más frecuentes, analizar los tiempos de procesamiento y detectar posibles reprocesos. Asimismo, nos interesa explorar posteriormente la posibilidad de utilizar Machine Learning para predecir si una solicitud será aprobada o rechazada.

Nuestra decisión se basa en el interés por analizar un proceso financiero que combina actividades operativas con decisiones institucionales y que podría ofrecer oportunidades de mejora a partir de los datos disponibles.

## 10. Declaración de uso de IA generativa


| **Herramienta** | **Para qué se utilizó** | **En qué parte del trabajo** |
|---|---|---|
| ChatGPT | Apoyo en la mejora de la redacción académica, comprensión de conceptos técnicos y aclaración de dudas puntuales. | Revisión de la redacción de algunas respuestas, comprensión de los componentes del Event Log y orientación sobre las posibles variables predictoras de Machine Learning. |

