# Corte 3 – Machine Learning e integración final

> **Fecha límite de entrega: jueves 15 de octubre de 2026, 23:59.**

## 1. Integrantes

- Integrante 1:
- Integrante 2:

## 2. Caso seleccionado

**Caso:**

## 3. Hallazgo de Process Mining que motiva el modelo

Explique qué resultado del Corte 2 llevó a formular el problema de Machine Learning.

## 4. Problema de Machine Learning

### Pregunta

### Variable objetivo

### Tipo de problema

- [ ] Clasificación
- [ ] Regresión
- [ ] Agrupamiento
- [ ] Otro

### Justificación

## 5. Variables predictoras

| Variable | Tipo | Origen | Justificación |
|---|---|---|---|
| | | | |

Indique si alguna variable fue derivada del proceso.

## 6. Preparación de datos

Documente, cuando corresponda:

- valores faltantes;
- codificación;
- escalamiento;
- selección de características;
- balance de clases;
- separación entrenamiento/prueba (aleatoria, estratificada o temporal) y por qué;
- eliminación de variables;
- prevención de fuga de información.

Antes de completar esta sección, lea [recursos/fuga_de_informacion.md](../recursos/fuga_de_informacion.md) y responda:

- **Momento de predicción:** ¿en qué punto del proceso se usaría el modelo?
- **Variables descartadas por fuga:** ¿cuáles y por qué?

## 7. Modelo o modelos utilizados

### 7.1 Modelo base (baseline)

Antes de entrenar cualquier algoritmo, defina un modelo base de referencia. Por ejemplo:

- clasificación: predecir siempre la clase más frecuente (`DummyClassifier`);
- regresión: predecir siempre la media o la mediana (`DummyRegressor`);
- agrupamiento: compararlo con una segmentación simple basada en una sola variable o regla de negocio.

Reporte sus métricas: todo modelo propuesto debe compararse contra él.

### 7.2 Modelos entrenados

Para cada modelo indique:

- algoritmo;
- motivo de selección;
- parámetros relevantes;
- procedimiento de entrenamiento;
- uso de validación cruzada o de un conjunto de validación para elegir parámetros (el conjunto de prueba se usa **una sola vez**, al final).

## 8. Evaluación

Utilice métricas apropiadas al tipo de problema.

### Clasificación

Posibles métricas:

- Accuracy
- Precision
- Recall
- F1-score
- Matriz de confusión

### Regresión

Posibles métricas:

- MAE
- MSE
- RMSE
- R²

### Agrupamiento

Posibles métricas y evidencias:

- Silhouette
- Índice de Davies-Bouldin
- Método del codo o justificación del número de grupos
- Descripción de cada grupo: tamaño, valores típicos de las variables y comportamiento en el proceso (variantes, duraciones)

En agrupamiento, la métrica no basta: cada grupo debe **interpretarse** en términos del caso.

### En todos los casos

Presente las métricas del modelo base junto a las del modelo o modelos propuestos.

No es obligatorio utilizar todas las métricas. Deben seleccionarse e interpretarse las que tengan sentido.

## 9. Interpretación del modelo

Explique:

- qué significa el resultado;
- qué variables parecen importantes;
- qué errores comete el modelo;
- cuáles son sus limitaciones.

## 10. Integración Process Mining + Machine Learning

Responda:

1. ¿Qué hallazgo del proceso dio origen al problema de ML?
2. ¿Qué variables del proceso ayudaron al modelo?
3. ¿El modelo confirma o contradice lo observado mediante Process Mining?
4. ¿Qué aporta ML que no era visible únicamente en el mapa del proceso?
5. ¿Qué aporta Process Mining que no puede observarse solamente mediante ML?

## 11. Conclusiones

Redacte al menos tres conclusiones sustentadas en resultados.

## 12. Recomendaciones

Formule al menos dos recomendaciones concretas.

Cada recomendación debe indicar:

- qué debería modificarse;
- en qué parte del proceso;
- por qué;
- qué evidencia respalda la recomendación.

## 13. Resumen ejecutivo

Redacte un resumen ejecutivo de máximo una página que integre:

- problemática;
- proceso analizado;
- hallazgos principales de Process Mining;
- problema de ML;
- resultado principal del modelo;
- conclusiones;
- recomendaciones.

---

## 14. Declaración de uso de IA generativa

Indique si utilizaron herramientas de IA generativa (ChatGPT, Claude, Copilot, Gemini u otras) en este corte.

| Herramienta | Para qué se utilizó | En qué parte del trabajo |
|---|---|---|
| | | |

Si no se utilizó ninguna, escriba: **"No se utilizaron herramientas de IA generativa."**

> Ambos integrantes deben poder explicar cualquier parte del trabajo, incluida la generada con apoyo de IA.
