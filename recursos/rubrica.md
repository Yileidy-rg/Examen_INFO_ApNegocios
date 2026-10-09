# Rúbrica de evaluación

## Peso de cada corte

| Corte | Entrega | Peso en la nota final |
|---|---|---|
| Corte 1 – Selección y justificación del caso | jueves 8 de octubre de 2026, 11:00 a.m. | **20 %** |
| Corte 2 – Process Mining | martes 13 de octubre de 2026, 23:59 | **35 %** |
| Corte 3 – Machine Learning e integración final | jueves 15 de octubre de 2026, 23:59 | **45 %** |

Cada corte se califica sobre **100 puntos** usando los criterios de abajo.

## Niveles de desempeño

En cada criterio se asigna el porcentaje del puntaje que corresponde al nivel alcanzado.

| Nivel | % del puntaje del criterio | Descripción general |
|---|---|---|
| **Excelente** | 100 % | Completo, correcto, justificado con evidencia e interpretado con criterio propio |
| **Satisfactorio** | 75 % | Completo y correcto, con justificación o interpretación parcial |
| **En desarrollo** | 45 % | Incompleto, con errores relevantes o con afirmaciones sin sustento |
| **Insuficiente** | 0–15 % | Ausente, incorrecto o sin relación con el caso |

---

## Corte 1 – Selección y justificación del caso (100 pts)

| Criterio | Pts | Excelente | Satisfactorio | En desarrollo | Insuficiente |
|---|---|---|---|---|---|
| **Revisión comparativa de los 6 casos** (sección 2) | 15 | Compara los 6 casos con argumentos específicos de cada uno sobre Process Mining y ML | Completa la tabla con observaciones correctas pero genéricas | Tabla incompleta o con respuestas repetidas entre casos | No hay revisión |
| **Justificación de la selección** (secciones 3–4) | 20 | Cubre todos los aspectos pedidos y los relaciona con las columnas reales del dataset | Cubre la mayoría de los aspectos con argumentos razonables | Justificación basada solo en gustos o interés personal | No justifica |
| **Comprensión del proceso** (5.1–5.3) | 15 | Inicio, fin y actividades coherentes con el caso y con los datos | Correcto pero superficial | Confusiones entre actividades, atributos y resultados | Ausente |
| **Propuesta de Event Log** (5.4) | 20 | Case ID, Activity y Timestamp correctos y justificados; atributos útiles distinguiendo nivel caso y nivel evento | Componentes correctos con justificación breve | Algún componente mal identificado (por ejemplo, Case ID que no identifica una instancia) | No propone el Event Log |
| **Preguntas e hipótesis** (secciones 6–7) | 15 | Preguntas respondibles con Process Mining e hipótesis concretas y verificables | Preguntas pertinentes pero generales | Preguntas que no se responden con Process Mining (por ejemplo, solo estadísticas descriptivas) | Ausentes |
| **Línea futura de ML** (secciones 8–9) | 15 | Variable objetivo clara, predictores plausibles, tipo de problema coherente; no construye modelo | Propuesta razonable con detalles faltantes | Objetivo ambiguo o tipo de problema incoherente | Ausente, o construye un modelo antes de tiempo |

---

## Corte 2 – Process Mining (100 pts)

| Criterio | Pts | Excelente | Satisfactorio | En desarrollo | Insuficiente |
|---|---|---|---|---|---|
| **Descripción y calidad de los datos** (sección 3) | 10 | Cifras correctas; identifica y cuantifica los problemas de calidad presentes | Cifras correctas; identifica algunos problemas | Cifras incompletas o erróneas | Ausente |
| **Preparación de datos** (sección 4) | 10 | Cada transformación tiene motivo y efecto cuantificado; el trabajo es reproducible desde `datos/raw` mediante scripts | Transformaciones documentadas con motivo, efecto parcial | Transformaciones sin justificar o hechas a mano | Sin preparación, o elimina datos sin explicar |
| **Event Log final** (sección 5) | 10 | Event Log correcto y ordenado, muestra representativa, decisiones sobre casos incompletos explicadas | Correcto con decisiones poco explicadas | Errores que afectan el análisis (actividades sin estandarizar, orden incorrecto) | Ausente o inválido |
| **Descubrimiento del proceso** (sección 6) | 10 | Modelo legible (filtrado o abstraído si hace falta) e interpretado; lo compara con el proceso esperado en el Corte 1 | Modelo presentado con interpretación general | Captura sin interpretación o modelo ilegible | Ausente |
| **Variantes** (sección 7) | 10 | Cuantifica variantes, explica las diferencias y las relaciona con atributos o resultados | Cuantifica y describe las principales | Solo enumera o reporta una cifra | Ausente |
| **Análisis temporal y cuellos de botella** (sección 8) | 15 | Usa la medida de tiempo correcta para el caso, identifica esperas y transiciones problemáticas y las explica | Identifica cuellos de botella con evidencia | Solo reporta promedios sin localizarlos en el proceso | Ausente |
| **Verificación de cumplimiento** (sección 9) | 10 | Define una referencia justificada, aplica una técnica adecuada, cuantifica las desviaciones y las relaciona con atributos o recursos | Detecta y cuantifica desviaciones con una referencia razonable | Menciona desviaciones sin cuantificarlas o sin referencia clara | Ausente |
| **Hallazgos** (sección 10) | 15 | Al menos 3 hallazgos no triviales, cada uno con evidencia cuantitativa e interpretación | 3 hallazgos con evidencia, interpretación limitada | Hallazgos obvios o sin evidencia | Menos de 2 hallazgos o sin sustento |
| **Implicaciones para ML y conclusión** (secciones 11–12) | 10 | Propone variables derivadas del proceso y distingue qué se conoce en cada momento del caso | Propone variables pertinentes | Lista variables sin relación con los hallazgos | Ausente |

---

## Corte 3 – Machine Learning e integración final (100 pts)

| Criterio | Pts | Excelente | Satisfactorio | En desarrollo | Insuficiente |
|---|---|---|---|---|---|
| **Formulación del problema** (secciones 3–4) | 10 | Pregunta, variable objetivo y tipo de problema derivados explícitamente de un hallazgo del Corte 2; se indica en qué momento del proceso se haría la predicción | Problema coherente con el Corte 2 | Problema poco relacionado con el proceso | Ausente o incoherente |
| **Variables y prevención de fuga de información** (secciones 5–6) | 15 | Predictores justificados que incluyen variables derivadas del proceso; excluye de forma explícita las variables que solo se conocen después del momento de predicción | Variables justificadas, sin fuga evidente | Usa sin advertirlo variables que revelan el resultado | Fuga grave que invalida el modelo |
| **Preparación y procedimiento de entrenamiento** (secciones 6–7) | 10 | Separación entrenamiento/prueba adecuada, tratamiento de faltantes, codificación y desbalance justificados; reproducible | Procedimiento correcto con justificación parcial | Errores metodológicos (por ejemplo, evaluar con los datos de entrenamiento) | Ausente |
| **Selección del modelo** (sección 7) | 10 | Compara contra un modelo base (baseline) y justifica el algoritmo por las características del problema | Justifica el algoritmo | Elige el modelo solo por la métrica más alta | Sin justificación |
| **Evaluación** (sección 8) | 15 | Métricas adecuadas al problema y al desbalance, interpretadas en términos del negocio (qué significa un falso negativo en el caso) | Métricas adecuadas con interpretación técnica | Solo accuracy en un problema desbalanceado, o métricas sin interpretar | Ausente |
| **Interpretación y limitaciones** (sección 9) | 10 | Explica las variables importantes, analiza los errores y reconoce limitaciones de los datos | Interpretación correcta pero general | Interpretación superficial | Ausente |
| **Integración Process Mining + ML** (sección 10) | 15 | Responde las 5 preguntas con evidencia de ambos análisis; muestra qué aporta cada enfoque | Responde las 5 preguntas de forma coherente | Respuestas genéricas o parciales | Ausente |
| **Conclusiones, recomendaciones y resumen ejecutivo** (secciones 11–13) | 15 | Recomendaciones concretas, ubicadas en el proceso y respaldadas por evidencia; resumen ejecutivo claro de una página | Conclusiones y recomendaciones sustentadas con detalles faltantes | Recomendaciones genéricas que no se desprenden de los resultados | Ausentes |

---

## Aspectos transversales

Estos aspectos pueden **restar hasta 10 puntos** en cualquier corte:

- **Reproducibilidad:** desde el Corte 2, todo el análisis se hace en Python (pm4py, pandas, scikit-learn) y los resultados deben poder regenerarse ejecutando en Google Colab los notebooks de `scripts/` a partir de `datos/raw/`. No se aceptan resultados obtenidos con herramientas gráficas o hojas de cálculo que no puedan reproducirse en código.
- **Organización del repositorio:** archivos en la carpeta correcta de `entregas/` y nombres descriptivos.
- **Participación de ambos integrantes:** debe observarse en el historial del repositorio (commits de ambos). El docente puede pedir a cualquier integrante que explique cualquier parte del trabajo. Si no puede hacerlo, la nota de esa parte puede ajustarse de forma individual.
- **Declaración de uso de IA:** debe estar completa en cada entrega (última sección de cada plantilla). Omitirla, o declarar algo que no corresponde a lo observado, resta puntos.
- **Coherencia entre cortes:** el caso no cambia, y si una decisión de un corte anterior se corrige, la corrección se explica.

Además, aplica lo indicado en [criterios_generales.md](criterios_generales.md) sobre lo que **no se considera suficiente**.
