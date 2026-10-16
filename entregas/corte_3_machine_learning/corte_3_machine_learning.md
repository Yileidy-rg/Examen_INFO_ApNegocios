# Corte 3 – Machine Learning e integración final

## 1. Integrantes

- Meylin Iliana López Solera
- Yileidy Rivera Granados

## 2. Caso seleccionado

**Caso 03: Solicitud y aprobación de créditos.**

Trabajamos con el archivo `datos/raw/caso_03_creditos.csv`. En el Corte 1 escogimos este caso porque queríamos estudiar cómo se revisan las solicitudes y qué influye en su aprobación o rechazo. En el Corte 2 analizamos las actividades y tiempos del proceso. En este corte utilizamos Machine Learning para intentar anticipar sus resultados.

## 3. Hallazgo de Process Mining que motiva el modelo

En el Corte 2 encontramos que las solicitudes no siempre siguen el mismo recorrido y que algunas requieren revisiones y documentos adicionales. Se identificaron 69 variantes del proceso y una mediana de espera de 3,14 días entre la solicitud y la recepción de información adicional. Esto nos mostró que es importante tanto conocer el resultado final del crédito como estudiar las actividades que pueden retrasar su trámite.

El **modelo principal** mantiene la pregunta propuesta en el Corte 1: predecir si una solicitud será aprobada o rechazada. Como **análisis complementario**, hicimos otro modelo para intentar anticipar la necesidad de información adicional, ya que este fue uno de los problemas encontrados con Process Mining.

## 4. Problema de Machine Learning

### Pregunta

**Principal:** ¿Es posible predecir si una solicitud de crédito será aprobada o rechazada con la información inicial del crédito y del solicitante?

**Complementaria:** ¿Es posible predecir si una solicitud necesitará información adicional durante el proceso?

### Variable objetivo

- **Principal:** `aprobada`: 1 si la solicitud fue aprobada y 0 si fue rechazada. No incluimos los desistimientos ni los casos sin decisión registrada.
- **Complementaria:** `necesita_info`: 1 si el registro contiene una solicitud de información adicional y 0 si no la contiene. Utilizamos solicitudes con una decisión de aprobación, rechazo o desistimiento registrada.

### Tipo de problema

- [x] Clasificación
- [ ] Regresión
- [ ] Agrupamiento
- [ ] Otro

### Justificación

Se trata de clasificación porque en los dos análisis buscamos anticipar una respuesta entre dos categorías. No queremos calcular un monto ni la cantidad de días que durará el trámite, sino intentar identificar una clase a partir de características conocidas al recibir la solicitud.

## 5. Variables predictoras

| Variable | Tipo | Origen | Justificación |
|---|---|---|---|
| `tipo_credito` | Categórica | Datos de la solicitud | Los requisitos pueden cambiar según el tipo de crédito. |
| `monto_solicitado` | Numérica | Datos de la solicitud | Permite estudiar si el monto se relaciona con el resultado. |
| `ingreso_mensual` | Numérica | Datos del solicitante | Describe la capacidad económica declarada. |
| `nivel_endeudamiento` | Numérica | Datos del solicitante | Permite considerar las deudas existentes. |
| `antiguedad_laboral_meses` | Numérica | Datos del solicitante | Representa el tiempo que lleva trabajando. |
| `edad_solicitante` | Numérica | Datos del solicitante | Se utilizó como característica adicional disponible en el registro. |
| `canal` | Categórica | Registro inicial | Identifica por qué medio ingresó la solicitud. |

**Variables derivadas del proceso:** no se incorporaron características calculadas a partir de recorridos o duraciones como entradas a los modelos. Sí utilizamos las actividades registradas para construir las **variables objetivo**: aprobación/rechazo y necesidad de información adicional. Esto es diferente de utilizarlas como variables predictoras.

## 6. Preparación de datos

Primero cargamos el archivo de eventos y unificamos los nombres de las actividades, eliminando diferencias de tildes, mayúsculas y espacios. Se encontraron **14 eventos sin fecha válida**; estos eventos no se eliminaron de forma automática porque la clasificación se basa en qué actividades aparecen, no en su duración. Después ordenamos los eventos y construimos una tabla con una sola fila por solicitud.

En total se identificaron **1.200 solicitudes**. Para el objetivo principal quedaron **1.162**: 864 aprobadas y 298 rechazadas. Se excluyeron 37 desistidas y una solicitud sin decisión registrada. Para el objetivo complementario se utilizaron **1.199** solicitudes con una decisión registrada: 307 con información adicional solicitada y 892 sin esa actividad.

En las siete características elegidas no se encontraron valores faltantes en la tabla utilizada para el modelo principal. Aun así, el código incluye una forma de completar posibles valores faltantes: mediana para columnas numéricas y valor más frecuente para columnas categóricas. También se estandarizaron las variables numéricas y se aplicó **One-Hot Encoding** para convertir las categorías en columnas numéricas.

En ambos modelos se separaron los datos de forma aleatoria y estratificada: **80 % para entrenamiento y 20 % para prueba**, con `random_state=42`. Esto permite mantener proporciones parecidas de las clases en ambas partes. El modelo principal utilizó 929 solicitudes para entrenar y 233 para probar; el complementario, 959 para entrenar y 240 para probar. No aplicamos técnicas de remuestreo o balance de clases; el desbalance se tuvo en cuenta al interpretar las métricas.

**Momento de predicción:** al recibir la solicitud, usando las características iniciales del crédito y del solicitante. El notebook comprobó que estas características no cambiaban entre eventos de una misma solicitud.

**Prevención de fuga de información:** no usamos como predictores la decisión final, la duración total, el número final de actividades, la solicitud de información adicional ni otras actividades que pueden ocurrir después de recibir la solicitud. La decisión y las actividades posteriores solamente sirven para conocer la respuesta verdadera durante el entrenamiento y la evaluación. El preprocesamiento se ajusta con los datos de entrenamiento mediante un `Pipeline`.

**Limitación:** la división es aleatoria y no temporal; por eso evalúa la generalización dentro de estos datos, pero no demuestra por sí sola cómo se comportaría el modelo con solicitudes futuras.

## 7. Modelo o modelos utilizados

### 7.1 Modelo base (baseline)

Utilizamos `DummyClassifier(strategy='most_frequent')`, que predice siempre la clase que aparece más veces. Es una referencia sencilla para comprobar si los demás modelos realmente aportan algo más que elegir siempre la respuesta más común.

En aprobación/rechazo, el modelo base obtuvo **Accuracy = 0,742** y **F1 de la clase aprobada = 0,852**. En información adicional obtuvo **Accuracy = 0,746**, pero **Precision, Recall y F1 de la clase positiva = 0**: no detectó ninguna solicitud que necesitara documentación adicional.

### 7.2 Modelos entrenados

**Regresión logística:** la utilizamos porque es un método sencillo de clasificación y permite tener una comparación interpretable. Se configuró con `max_iter=1000` y `random_state=42`.

**Random Forest:** lo utilizamos porque combina distintos árboles de decisión y puede reconocer relaciones más complejas entre las características. Se configuró con `n_estimators=200`, `min_samples_leaf=3` y `random_state=42`.

Los tres modelos utilizaron la misma separación de entrenamiento y prueba dentro de cada objetivo y el mismo tipo de preparación de datos. **No se realizó validación cruzada ni una búsqueda de parámetros**: se probaron las configuraciones establecidas en el notebook. Los resultados obtenidos son una comparación inicial; no deben presentarse como una optimización rigurosa.

## 8. Evaluación

### Modelo principal: aprobación o rechazo

En esta tabla, **Precision, Recall y F1-score corresponden a la clase positiva (aprobada = 1)**.

| Modelo | Accuracy | Precision | Recall | F1-score |
|---|---:|---:|---:|---:|
| DummyClassifier | 0,742 | 0,742 | 1,000 | 0,852 |
| Regresión logística | 0,773 | 0,800 | 0,925 | 0,858 |
| Random Forest | **0,777** | **0,804** | 0,925 | **0,860** |

Random Forest obtuvo la mejor Accuracy y el mejor F1 para la aprobación, aunque la mejora respecto al modelo base fue pequeña. En el conjunto de prueba hubo **173 aprobadas y 60 rechazadas**. Random Forest identificó bien aproximadamente **160 aprobaciones y 21 rechazos**; confundió **39 rechazos con aprobaciones** y **13 aprobaciones con rechazos**. Por eso es importante revisar la clase rechazada por separado: su Recall fue **0,35** y su F1-score, **0,45**.

### Modelo complementario: necesidad de información adicional

En esta tabla, la clase positiva corresponde a **sí necesita información adicional = 1**.

| Modelo | Accuracy | Precision | Recall | F1-score |
|---|---:|---:|---:|---:|
| DummyClassifier | 0,746 | 0,000 | 0,000 | 0,000 |
| Regresión logística | 0,725 | 0,273 | 0,049 | 0,083 |
| Random Forest | 0,708 | **0,333** | **0,148** | **0,205** |

Random Forest obtuvo el mejor F1 para detectar información adicional, pero su Recall fue bajo. En el conjunto de prueba, detectó **9 de 61** solicitudes que sí necesitaron información adicional; dejó sin detectar **52** y marcó incorrectamente **18** solicitudes como positivas.

En el notebook se incluyen gráficos de comparación, reportes de clasificación y matrices de confusión para respaldar las cifras.

## 9. Interpretación del modelo

En el modelo principal, Random Forest logró predecir correctamente la mayoría de las aprobaciones, pero le costó más identificar los rechazos. Su Accuracy de **77,7 %** no significa que acierte igual de bien en ambas clases. Como hay muchas más aprobaciones que rechazos, debemos observar también el Recall de la clase rechazada, que fue de **35 %**.

Al analizar la importancia de las variables mediante permutación, el **nivel de endeudamiento** presentó la mayor importancia positiva para el modelo principal (**0,0344 de caída media de F1 al desordenarlo**). Le siguieron el monto solicitado y el ingreso mensual. Esta medida muestra cuánto cambia el rendimiento del modelo y no demuestra que una variable sea causa de aprobación o rechazo.

En el modelo complementario, el **canal de solicitud** presentó la mayor importancia positiva (aproximadamente **0,0978**). Algunas variables tuvieron valores de importancia negativos, lo cual no significa que sean perjudiciales; puede deberse a variaciones o ruido en la evaluación. El principal límite de este modelo es que detectó apenas el **14,8 %** de los casos con información adicional.

Ambos modelos son exploratorios. No demostramos que puedan tomar decisiones de crédito de forma segura ni que su uso reduzca automáticamente los retrasos. Tampoco medimos su rendimiento con solicitudes verdaderamente futuras o con información adicional sobre requisitos documentales.

## 10. Integración Process Mining + Machine Learning

**1. ¿Qué hallazgo del proceso dio origen al problema de ML?**

En el Corte 1 ya habíamos propuesto predecir aprobación o rechazo, así que mantuvimos esa pregunta como objetivo principal. En el Corte 2 identificamos distintos recorridos, revisiones y un posible cuello de botella al solicitar información adicional: la mediana de espera entre solicitud y recepción fue de **3,14 días**. Ese hallazgo dio lugar al segundo objetivo de ML, centrado en intentar anticipar la necesidad de información adicional.

**2. ¿Qué variables del proceso ayudaron al modelo?**

Los modelos utilizaron siete características iniciales de las solicitudes, no características de secuencias o tiempos calculadas mediante Process Mining. El registro de actividades sí permitió saber el resultado real de cada solicitud y si se había pedido información adicional; esas actividades sirvieron para construir las etiquetas de entrenamiento. No podemos afirmar que variables derivadas del recorrido hayan mejorado el desempeño porque no fueron usadas como predictores.

**3. ¿El modelo confirma o contradice lo observado mediante Process Mining?**

El análisis confirma que la solicitud de información adicional es una actividad que aparece en una parte importante de los casos, pero no demuestra que los datos iniciales permitan anticiparla bien. De las solicitudes aprobadas, **199 de 864 (23,0 %)** necesitaron información adicional; de las rechazadas, **71 de 298 (23,8 %)**. Los porcentajes son parecidos, por lo que necesitar información adicional no distingue claramente entre aprobación y rechazo en estos datos. Además, las **37 solicitudes desistidas** tenían registrada una solicitud de información adicional, aunque eso no demuestra que pedir documentos haya causado su desistimiento.

**4. ¿Qué aporta ML que no era visible únicamente en el mapa del proceso?**

Machine Learning permite probar si los datos iniciales contienen patrones útiles para anticipar un resultado antes de que finalice el trámite. En nuestro caso, pudimos medir cuántas decisiones predice correctamente cada modelo, comparar los algoritmos con una referencia básica e identificar sus errores. También detectamos que el nivel de endeudamiento tuvo la mayor importancia positiva en la predicción principal, sin interpretar esto como causalidad.

**5. ¿Qué aporta Process Mining que no puede observarse solamente mediante ML?**

Process Mining muestra las rutas y el orden de las actividades, las revisiones repetidas, las variantes y los tiempos de espera. Estos aspectos ayudan a localizar en qué parte del proceso podría intervenirse. Un clasificador que devuelve aprobación o rechazo no explica por sí solo dónde aparecen las demoras, mientras que Process Mining permite localizar el posible cuello de botella relacionado con la documentación adicional.

En conjunto, las dos técnicas permiten **comprender el funcionamiento del proceso y evaluar si es posible anticipar determinados resultados**. Los resultados actuales todavía no justifican automatizar decisiones crediticias.

## 11. Conclusiones

1. El Corte 2 permitió identificar rutas distintas y un posible cuello de botella en la documentación adicional, con una mediana de espera de **3,14 días**. Esto ofreció una razón concreta para investigar una predicción complementaria relacionada con los documentos.
2. El modelo principal Random Forest logró una Accuracy de **77,7 %**, frente a **74,2 %** del modelo base, pero solo identificó **35 % de los rechazos**. Por lo tanto, el resultado global no es suficiente para considerar que el modelo funciona bien en ambas clases.
3. El modelo complementario obtuvo un F1-score de **0,205** y un Recall de **14,8 %** para las solicitudes que necesitaron información adicional. Esto indica que las características iniciales disponibles no permitieron detectar la mayoría de esos casos.
4. Las solicitudes aprobadas y rechazadas tuvieron proporciones parecidas de información adicional (**23,0 %** y **23,8 %**, respectivamente). En los datos analizados, pedir información adicional no diferencia claramente ambos resultados.
5. La integración entre Process Mining y Machine Learning fue útil para observar tanto las demoras y recorridos como los límites de la predicción. Sin embargo, no se demostró que aplicar estos modelos reduzca tiempos o mejore decisiones reales.

## 12. Recomendaciones

**1. Revisar los documentos desde la recepción de la solicitud.**

- **Qué modificar:** usar una lista de verificación de documentos y requisitos según el tipo de crédito.
- **Dónde:** en la recepción y verificación documental inicial.
- **Por qué:** para intentar reducir solicitudes posteriores de información faltante y tiempos de espera.
- **Evidencia:** el Corte 2 encontró una mediana de **3,14 días** entre pedir y recibir información adicional. La mejora debería evaluarse comparando los tiempos antes y después de implementarla.

**2. Mejorar los datos que se registran durante el trámite.**

- **Qué modificar:** registrar con claridad qué documentos faltaron, cuándo se pidieron y recibieron, qué revisiones se realizaron y cuál fue la decisión final.
- **Dónde:** en verificación documental y evaluación crediticia.
- **Por qué:** estas características podrían ser más útiles para futuros modelos que solamente los datos económicos iniciales, siempre que estén disponibles en el momento de predecir.
- **Evidencia:** el modelo de información adicional detectó apenas **14,8 %** de los casos positivos y se encontraron **14 eventos sin fecha válida** en el archivo.

**3. No automatizar la aprobación o el rechazo con el modelo actual.**

- **Qué modificar:** mantener la revisión humana y evaluar nuevas configuraciones, más datos y pruebas temporales antes de usar predicciones como apoyo.
- **Dónde:** en la etapa de análisis y decisión crediticia.
- **Por qué:** un error puede afectar solicitudes reales y el modelo todavía no reconoce bien los rechazos.
- **Evidencia:** Random Forest identificó correctamente solo **21 de 60** rechazos del conjunto de prueba.

## 13. Resumen ejecutivo

En este proyecto estudiamos el proceso de solicitud y aprobación de créditos mediante Process Mining y Machine Learning. En el Corte 1 propusimos investigar si era posible predecir la aprobación o el rechazo de una solicitud. En el Corte 2 analizamos las actividades, los recorridos y los tiempos, encontrando distintas variantes y un posible cuello de botella relacionado con la información adicional, cuya solicitud y recepción presentaron una mediana de espera de 3,14 días.

En el Corte 3 construimos un modelo principal para predecir aprobación o rechazo y otro complementario para predecir la necesidad de información adicional. Para ambos comparamos DummyClassifier, Regresión Logística y Random Forest usando características iniciales de las solicitudes. En la predicción principal utilizamos 1.162 casos. Random Forest obtuvo la mejor Accuracy, con 77,7 %, pero solamente identificó el 35 % de los rechazos. En el análisis complementario utilizamos 1.199 casos y Random Forest detectó el 14,8 % de las solicitudes que realmente necesitaron documentación adicional.

Estos resultados muestran que el Machine Learning permitió encontrar algunos patrones, pero sus errores todavía son importantes. También observamos que necesitar información adicional fue casi igual de frecuente entre aprobaciones (23,0 %) y rechazos (23,8 %). El análisis de Process Mining nos ayudó a identificar dónde aparecen esperas, mientras que Machine Learning nos permitió evaluar cuánto pueden anticipar los datos iniciales.

Recomendamos revisar los documentos desde el inicio del trámite, mejorar el registro de información sobre documentos y revisiones, y realizar nuevas evaluaciones antes de utilizar modelos para apoyar decisiones crediticias. No consideramos adecuado automatizar aprobaciones o rechazos con los resultados actuales.

## 14. Declaración de uso de IA generativa

| Herramienta | Para qué se utilizó | En qué parte del trabajo |
|---|---|---|
| ChatGPT | Apoyo para organizar el notebook, redactar y simplificar explicaciones, revisar el código y comprender las métricas y limitaciones de los modelos. | Preparación del Corte 3, explicación de resultados, integración con Process Mining y elaboración del informe. |

Las integrantes deben revisar el contenido, comprobar que coincida con los resultados ejecutados en el notebook y poder explicar los procedimientos y conclusiones.
