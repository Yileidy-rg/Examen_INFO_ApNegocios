# Corte 2 – Process Mining

## 1. Integrantes

- Integrante 1: **Yileidy Rivera Granados**
- Integrante 2: **Meylin Iliana López Solera**

## 2. Caso seleccionado

**Caso 3: Solicitud y aprobación de créditos.**

El análisis estudia la secuencia real de actividades de las solicitudes de crédito, sus rutas alternativas, los tiempos transcurridos y la presencia de actividades obligatorias según las reglas de negocio. Para ello, se utiliza minería de procesos sobre un registro de eventos sintético, preparado con Python y PM4Py en Google Colab.

## 3. Descripción de los datos utilizados

El conjunto de datos utilizado corresponde al Caso 3: Solicitud y aprobación de créditos. Contiene información sobre las actividades realizadas durante el procesamiento de solicitudes de crédito, desde su recepción hasta los posibles resultados del trámite.

Cantidad de registros: 7.978 eventos originales.

Cantidad de casos: 1.200 solicitudes de crédito.

Cantidad de actividades: 37 denominaciones originales, que se redujeron a 12 actividades después de la normalización.

Rango temporal: del 5 de enero de 2026 a las 07:29:11 al 30 de junio de 2026 a las 10:55:12.

Variables principales: case_id, activity, timestamp, resource, tipo_credito, monto_solicitado, ingreso_mensual, nivel_endeudamiento, antiguedad_laboral_meses, canal y edad_solicitante.

Problemas de calidad detectados: se identificaron 14 eventos sin marca temporal válida, equivalentes aproximadamente al 0,18 % de los registros originales. También se encontraron 31 filas completamente duplicadas y diferencias en la escritura de los nombres de las actividades. No se detectaron valores nulos en las columnas case_id ni activity.

Los problemas encontrados evidencian la necesidad de preparar los datos antes de realizar el análisis de minería de procesos. En particular, las fechas faltantes limitan el análisis cronológico, mientras que los registros duplicados requieren revisión para determinar si representan eventos repetidos válidos o errores de registro.

## 4. Preparación de datos

| **Transformación** | **Motivo** | **Efecto sobre los datos** |
| --- | --- | --- |
| Conservación de una copia del conjunto original | Evitar perder evidencia durante la limpieza y permitir validaciones posteriores | Se mantiene el conjunto completo de 7.978 eventos para comprobaciones de presencia de actividades. |
| Normalización de `activity`: minúsculas, espacios y eliminación de tildes | Unificar etiquetas equivalentes escritas de distintas maneras | De 37 denominaciones diferentes se pasa a 12 actividades normalizadas, sin eliminar eventos por esta operación. |
| Conversión de `timestamp` a tipo fecha-hora con `errors="coerce"` | Habilitar el ordenamiento y las mediciones temporales | Se identifican 14 registros con fecha ausente o no interpretable. |
| Exclusión de eventos sin fecha del Event Log temporal | Evitar secuencias o duraciones basadas en fechas desconocidas | El Event Log temporal queda con **7.964 eventos** y mantiene **1.200 solicitudes**. |
| Ordenamiento por `case_id` y `timestamp` | Reconstruir secuencias de actividades por solicitud | Permite identificar variantes y calcular tiempos entre eventos consecutivos. |
| Conservación de eventos sin fecha para validaciones de presencia | No confundir falta de fecha con falta de actividad | Las comprobaciones de reglas utilizan los eventos originales, incluidos los 14 que no tienen fecha. |

## 5. Event Log final

El Event Log del Caso 3: Solicitud y aprobación de créditos se construyó mediante Python en Google Colab, utilizando la biblioteca pandas para la preparación y organización de los datos. El código correspondiente se encuentra en el notebook scripts/Colab_Corte2.ipynb.

El archivo de salida establecido es datos/processed/caso_03_event_log.csv. Los datos procesados pueden regenerarse a partir del conjunto de datos original mediante la ejecución del notebook, por lo que no es necesario almacenarlos permanentemente en GitHub.

Para la construcción del Event Log se utilizaron los siguientes atributos:

Case ID: case_id, identificador único de cada solicitud de crédito, que permite agrupar los eventos correspondientes a un mismo trámite.

Activity: activity, nombre normalizado de la actividad realizada durante el procesamiento de la solicitud.

Timestamp: timestamp, fecha y hora en que se registró cada actividad, utilizada para ordenar cronológicamente los eventos.

Otros atributos utilizados: resource, tipo_credito, monto_solicitado, ingreso_mensual, nivel_endeudamiento, antiguedad_laboral_meses, canal y edad_solicitante. También se conservó activity_original para mantener la trazabilidad de los nombres originales de las actividades.

Muestra representativa del Event Log

La siguiente tabla presenta la secuencia de eventos correspondiente a la solicitud SOL00001, mostrando las actividades realizadas, sus marcas temporales y los recursos asociados.


| Case ID | Actividad | Timestamp | Recurso |
|---|---|---|---|
| SOL00001 | solicitud recibida | 2026-01-16 08:27:56 | Ejecutivo_03 |
| SOL00001 | verificacion de documentos | 2026-01-19 08:00:00 | Verificador_01 |
| SOL00001 | analisis financiero | 2026-01-20 08:00:00 | Analista_02 |
| SOL00001 | aprobacion | 2026-01-20 15:22:31 | Analista_02 |
| SOL00001 | formalizacion | 2026-01-26 08:00:00 | Ejecutivo_01 |
| SOL00001 | desembolso | 2026-01-27 08:00:00 | Tesoreria |


El Event Log preparado para el análisis cronológico contiene 7.964 eventos correspondientes a 1.200 solicitudes de crédito. Esta cantidad es inferior a los 7.978 registros originales debido a que se excluyeron 14 eventos que no contaban con una marca temporal válida.

Los eventos sin fecha se conservaron en los datos originales para las comprobaciones que no requieren información cronológica, especialmente la verificación de presencia de actividades.

La construcción del Event Log permite representar las actividades realizadas en cada solicitud, identificar las diferentes rutas del proceso y desarrollar los análisis de variantes, tiempos y cumplimiento de las reglas establecidas.

## 6. Descubrimiento del proceso
Para analizar el comportamiento real del proceso de solicitud y aprobación de créditos, se aplicó la técnica Directly-Follows Graph (DFG) mediante la biblioteca PM4Py en Python.

Esta técnica permite identificar las relaciones directas entre las actividades registradas en el Event Log, representando gráficamente las secuencias observadas y la frecuencia con la que se ejecutan.

El análisis se realizó utilizando los 7.964 eventos con marcas temporales válidas, correspondientes a 1.200 solicitudes de crédito. A partir de estos registros, se generó un mapa que permite visualizar las principales rutas del proceso y las actividades que intervienen en su ejecución.

Mapa del proceso descubierto

![Mapa del proceso descubierto mediante DFG](img/mapa_proceso.png)

Interpretación

El proceso descubierto presenta una ruta predominante conformada por las siguientes actividades:

Solicitud recibida → Verificación de documentos → Análisis financiero → Aprobación → Formalización → Desembolso.

Esta secuencia representa el recorrido habitual de una solicitud que completa satisfactoriamente las etapas de evaluación, aprobación y otorgamiento del crédito.

Sin embargo, el análisis también permitió identificar rutas alternativas que incluyen actividades como la solicitud y recepción de información adicional, la participación del comité de crédito y la reevaluación de solicitudes. Asimismo, existen recorridos que finalizan con rechazo o desistimiento, lo que evidencia que no todas las solicitudes siguen el mismo procedimiento hasta su finalización.

La existencia de estas rutas alternativas refleja la variabilidad del proceso, que puede estar relacionada con las características de cada solicitud, los requisitos de evaluación y las decisiones tomadas durante el trámite.

El mapa DFG permite reconocer las actividades y transiciones más frecuentes, así como identificar posibles puntos del proceso que requieren un análisis más detallado. No obstante, esta representación por sí sola no permite determinar si una secuencia cumple correctamente todas las políticas establecidas ni identificar las causas de las diferencias observadas.

## 7. Variantes

El análisis de variantes permitió identificar **69 secuencias diferentes** en las 1.200 solicitudes de crédito estudiadas. Esto evidencia que, aunque existe una ruta predominante, no todas las solicitudes siguen exactamente las mismas actividades.

La variante más frecuente corresponde a la siguiente secuencia:

**Solicitud recibida → Verificación de documentos → Análisis financiero → Aprobación → Formalización → Desembolso.**

Esta ruta se presentó en **483 solicitudes**, equivalentes al **40,25 %** del total.

### Variantes más frecuentes

| Variante / diferencia principal | Solicitudes | Porcentaje del total |
|---|---:|---:|
| Ruta principal sin pasos adicionales | 483 | 40,25 % |
| Solicitud y recepción de información adicional, seguida de nueva verificación | 131 | 10,92 % |
| Intervención del comité de crédito antes de la aprobación | 90 | 7,50 % |
| Análisis financiero seguido de rechazo | 87 | 7,25 % |
| Intervención del comité de crédito seguida de rechazo | 76 | 6,33 % |

Las cinco variantes más frecuentes reúnen **867 solicitudes, equivalentes al 72,25 % del total**.

### Interpretación

Los resultados muestran que la ruta principal representa una proporción importante del proceso. Sin embargo, el 59,75 % de las solicitudes sigue recorridos diferentes, lo que evidencia variaciones en las actividades requeridas y en los resultados finales.

Entre las diferencias identificadas destacan las solicitudes que requieren información adicional, aquellas que deben pasar por el comité de crédito y las que terminan en rechazo.

Estas variantes permiten reconocer los puntos en los que cambia el recorrido de las solicitudes y constituyen una base para investigar posibles diferencias en los tiempos de procesamiento y en las características de los créditos.

## 8. Análisis temporal

El análisis temporal permitió evaluar la duración de las solicitudes de crédito y los tiempos transcurridos entre las diferentes actividades del proceso. Para ello, se utilizaron los eventos con marcas temporales válidas, calculando el tiempo entre la primera y la última actividad registrada de cada solicitud.

### Duración de las solicitudes

| Indicador | Duración observada |
|---|---:|
| Promedio | **8,9 días** |
| Mediana | **8,1 días** |
| Mínimo | **1,06 días** |
| Máximo | **41,02 días** |

Los resultados muestran que las solicitudes presentan una duración promedio observada de **8,9 días**, mientras que la mediana es de **8,1 días**. Esto significa que aproximadamente la mitad de las solicitudes registró una duración inferior o igual a 8,10 días y la otra mitad una duración igual o superior.

La diferencia entre el promedio y la mediana sugiere que algunas solicitudes presentan tiempos de procesamiento mayores que los habituales, lo que puede influir en el promedio general.

Sin embargo, estos valores representan el período comprendido entre la primera y la última actividad registrada, por lo que no necesariamente equivalen al tiempo total de resolución de las solicitudes que permanecían abiertas al momento de extraer los datos.

### Tiempos entre actividades

Se identificaron las siguientes transiciones relevantes:

| Transición | Frecuencia | Promedio | Mediana |
|---|---:|---:|---:|
| Solicitud de información adicional → Recepción de información | 323 | 3,85 días | **3,14 días** |
| Análisis financiero → Comité de crédito | 249 | 3,48 días | 2,77 días |
| Aprobación → Formalización | 858 | 3,01 días | 2,87 días |
| Solicitud de información adicional → Desistimiento | 36 | 15,00 días | 15,00 días |

### Identificación de posibles cuellos de botella

Uno de los principales puntos de espera identificados corresponde a la transición entre **Solicitud de información adicional y Recepción de información**, registrada en 323 ocasiones, con un promedio de 3,85 días y una mediana de 3,14 días.

Este resultado sugiere que la recopilación de información adicional puede representar un posible cuello de botella recurrente, ya que introduce períodos de espera en las solicitudes que requieren documentación complementaria.

También se identificó que la transición entre **Análisis financiero y Comité de crédito** presentó una mediana de 2,77 días en 249 ocasiones, mientras que el paso entre **Aprobación y Formalización** registró una mediana de 2,87 días en 858 ocasiones. Estos resultados señalan otras etapas que podrían estudiarse para identificar oportunidades de mejora.

Por otra parte, la transición entre **Solicitud de información adicional y Desistimiento** presentó una duración considerablemente mayor, con una mediana de 15 días y una frecuencia de 36 casos. Sin embargo, este tiempo no necesariamente representa una demora operativa, debido a que el desistimiento puede depender de decisiones del solicitante o de condiciones externas al proceso.

En general, los resultados permiten reconocer etapas con tiempos de espera relevantes y posibles oportunidades para mejorar la eficiencia del procedimiento. No obstante, los intervalos calculados representan el tiempo transcurrido entre eventos registrados y no necesariamente el tiempo de trabajo activo de los responsables.

Por esta razón, se recomienda profundizar en las transiciones con mayor frecuencia y duración, evitando establecer conclusiones a partir de casos aislados.

## 9. Verificación de cumplimiento (conformance)

La verificación de cumplimiento se realizó mediante código Python, tomando como referencia las reglas de negocio establecidas para el Caso 3.

Se evaluaron dos condiciones principales:

Regla 1: toda solicitud aprobada debe contar con un análisis financiero. Se identificaron 18 solicitudes sin esta actividad registrada, de un total de 862 aprobadas (2,09 %).

Regla 2: los créditos hipotecarios o superiores a $30.000 requieren intervención del comité de crédito. Se identificaron 7 solicitudes sin este evento registrado, de 256 que cumplían las condiciones (2,73 %).

Estas ausencias requieren revisión, ya que no necesariamente representan incumplimientos definitivos. Adicionalmente, dos solicitudes presentaron fechas incompletas que impidieron verificar el orden de sus actividades.

El análisis detallado se encuentra en el notebook scripts/Colab_Corte2.ipynb, en la sección Verificación de cumplimiento.

## 10. Hallazgos

A partir de los resultados del análisis se identificaron cuatro hallazgos principales:

### Hallazgo 1. Calidad de los datos

Evidencia: se identificaron 14 eventos sin fecha de un total de 7.978 registros, además de diferencias en los nombres de las actividades.

Interpretación: estos problemas pueden afectar la reconstrucción cronológica del proceso, por lo que fue necesario normalizar las actividades y tratar las fechas faltantes.

### Hallazgo 2. Variabilidad del proceso

Evidencia: se identificaron 69 variantes, de las cuales la más frecuente reúne 483 solicitudes (40,25 %).

Interpretación: existe una ruta predominante, pero una proporción considerable de solicitudes sigue recorridos alternativos.

### Hallazgo 3. Cumplimiento de reglas

Evidencia: 18 solicitudes aprobadas no tienen análisis financiero registrado y 7 solicitudes que requerían comité no presentan dicha actividad.

Interpretación: estos registros requieren revisión para determinar si corresponden a desviaciones del proceso o a limitaciones de los datos disponibles.

### Hallazgo 4. Posibles cuellos de botella

Evidencia: la transición entre solicitud y recepción de información adicional presenta 323 ocurrencias y una mediana de espera de 3,14 días.

Interpretación: esta transición constituye un posible punto de demora recurrente que podría afectar la duración de los trámites.

El análisis completo se encuentra en scripts/Colab_Corte2.ipynb, en las sección de hallazgo.

## 11. Implicaciones para Machine Learning

Los resultados obtenidos permiten identificar oportunidades para desarrollar modelos predictivos orientados a anticipar la necesidad de información adicional, estimar la duración restante de las solicitudes y detectar posibles riesgos de desistimiento.

Para ello, podrían utilizarse variables como el tipo de crédito, monto solicitado, ingreso mensual, nivel de endeudamiento y características del recorrido de cada solicitud.

Entre las variables derivadas del proceso se consideran la duración acumulada, cantidad de actividades realizadas, repeticiones de actividades y tiempos de espera.

Es importante utilizar únicamente la información disponible antes del momento de la predicción para evitar la fuga de información. Asimismo, deberán considerarse las fechas faltantes y las solicitudes que todavía se encuentren abiertas.

La explicación detallada se encuentra en scripts/Colab_Corte2.ipynb, en el apartado de implicaciones para Machine Learning.

## 12. Conclusión del corte

La aplicación de minería de procesos permitió analizar 1.200 solicitudes de crédito, identificar 69 variantes y reconocer posibles puntos de demora y registros que requieren revisión de cumplimiento.

Los resultados evidencian que, aunque existe una ruta predominante, el proceso presenta recorridos alternativos relacionados con información adicional, intervención del comité y diferentes decisiones sobre las solicitudes.

Como continuación del proyecto, se propone explorar modelos de Machine Learning que permitan anticipar retrasos o necesidades de documentación adicional. Para ello, será importante fortalecer la calidad de los datos y utilizar variables disponibles en cada etapa del proceso.

La conclusión detallada se encuentra en scripts/Colab_Corte2.ipynb.

## 13. Declaración de uso de IA generativa

Se utilizó asistencia de IA generativa para apoyar la revisión de código, la organización del cuaderno y la redacción del documento, con supervisión y contraste frente a las salidas registradas del análisis.

| **Herramienta** | **Para qué se utilizó** | **En qué parte del trabajo** |
| --- | --- | --- |
| ChatGPT | Apoyo para revisar la ejecución, detectar limitaciones de las verificaciones, organizar el análisis y redactar explicaciones a partir de los resultados del notebook | Revisión del Colab, interpretación de indicadores y redacción de este documento|
