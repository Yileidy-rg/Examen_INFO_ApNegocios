**Universidad de Costa Rica**

**Sede del Sur**

Informática Aplicada a los Negocios IF6201

EXAMEN PRÁCTICO I

**Estudiantes**

Meylin Iliana López Solera 

Yileidy Rivera Granados C4I972

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

# 8. Posible problema futuro de Machine Learning
