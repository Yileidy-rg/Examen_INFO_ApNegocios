# Corte 2 – Process Mining

> **Fecha límite de entrega: martes 13 de octubre de 2026, 23:59.**

## 1. Integrantes

- Integrante 1:
- Integrante 2:

## 2. Caso seleccionado

**Caso:**

## 3. Descripción de los datos utilizados

Indique:

- cantidad de registros;
- cantidad de casos;
- cantidad de actividades;
- rango temporal;
- variables principales;
- problemas de calidad detectados.

## 4. Preparación de datos

Documente las transformaciones realizadas.

| Transformación | Motivo | Efecto sobre los datos |
|---|---|---|
| | | |

## 5. Event Log final

Indique en qué notebook o script de `scripts/` se construye el Event Log y en qué archivo de `datos/processed/` se guardó.


Indique claramente:

- **Case ID:**
- **Activity:**
- **Timestamp:**
- **Otros atributos utilizados:**

Incluya una muestra representativa del Event Log.

## 6. Descubrimiento del proceso

Incluya el modelo o mapa de proceso generado.

### Interpretación

Explique qué muestra el proceso descubierto.

## 7. Variantes

Indique como mínimo:

- cantidad de variantes;
- variante más frecuente;
- porcentaje o frecuencia;
- principales diferencias entre variantes.

## 8. Análisis temporal

Analice:

- duración promedio;
- duración mínima y máxima;
- actividades con mayor espera;
- transiciones problemáticas;
- posibles cuellos de botella.

## 9. Verificación de cumplimiento (conformance)

Compare el comportamiento registrado con el proceso esperado. Como referencia puede usar:

- el proceso que la pareja describió en el Corte 1;
- las reglas o políticas indicadas en `datos/README.md` para su caso;
- un modelo descubierto a partir de las variantes principales.

Indique:

- qué referencia utilizó y por qué;
- qué técnica aplicó (por ejemplo, *token-based replay*, alineamientos o verificación de reglas con código);
- qué desviaciones encontró: actividades omitidas, orden incorrecto, actividades no esperadas;
- cuántos casos presentan cada desviación;
- si las desviaciones se relacionan con algún atributo, recurso o resultado.

**Respuesta:**

## 10. Hallazgos

Documente al menos tres hallazgos.

### Hallazgo 1

**Evidencia:**

**Interpretación:**

### Hallazgo 2

**Evidencia:**

**Interpretación:**

### Hallazgo 3

**Evidencia:**

**Interpretación:**

## 11. Implicaciones para Machine Learning

A partir de lo descubierto, indique:

- qué resultado sería útil predecir o explicar;
- qué variables podrían utilizarse;
- qué variables derivadas del proceso podrían construirse.

Ejemplos de variables derivadas:

- duración acumulada;
- cantidad de actividades;
- número de repeticiones;
- cantidad de reasignaciones;
- presencia de determinada actividad;
- variante del proceso.

## 12. Conclusión del corte

Explique qué se aprendió del proceso y cuál será la dirección del análisis de Machine Learning.

---

## 13. Declaración de uso de IA generativa

Indique si utilizaron herramientas de IA generativa (ChatGPT, Claude, Copilot, Gemini u otras) en este corte.

| Herramienta | Para qué se utilizó | En qué parte del trabajo |
|---|---|---|
| | | |

Si no se utilizó ninguna, escriba: **"No se utilizaron herramientas de IA generativa."**

> Ambos integrantes deben poder explicar cualquier parte del trabajo, incluida la generada con apoyo de IA.
