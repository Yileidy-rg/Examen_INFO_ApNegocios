# Fuga de información en modelos basados en procesos

## ¿Qué es?

Hay **fuga de información** (*data leakage*) cuando un modelo se entrena con información que **no estaría disponible en el momento en que se quiere usar la predicción**.

El modelo obtiene métricas excelentes en el examen, pero sería inútil en la práctica, porque en la realidad esa información todavía no existe.

En problemas construidos a partir de un Event Log, la fuga es **muy fácil de cometer**. Cada caso tiene un inicio, un desarrollo y un final, y las variables calculadas sobre el caso completo ya "conocen" el desenlace.

---

## Ejemplo

Se quiere predecir si un pedido será **entregado con atraso**.

| Variable | ¿Se conoce al recibir el pedido? | ¿Se puede usar? |
|---|---|---|
| Categoría, monto, método de pago, región | Sí | ✅ |
| Fecha prometida | Sí | ✅ |
| Tiempo entre "Pedido recibido" y "Validación" | Solo después de la validación | ⚠️ Depende del momento de predicción |
| Hubo "Entrega fallida" | No: ocurre al final | ❌ |
| Duración total del pedido | No: el atraso se calcula con ella | ❌ |

Un modelo que use la **duración total** para predecir el atraso tendrá un rendimiento casi perfecto y no aportará nada.

---

## La pregunta clave: ¿en qué momento se hace la predicción?

Antes de elegir variables, la pareja debe definir **el momento de predicción**. Por ejemplo:

- al registrar el caso (solo atributos iniciales);
- después de cierta actividad (por ejemplo, después de "Diagnóstico");
- después de cierto tiempo (por ejemplo, al terminar la semana 5 del curso).

Todas las variables deben calcularse **únicamente con los eventos ocurridos hasta ese momento**. A esa porción inicial de la traza se le llama **prefijo**.

```text
Traza completa:  Registro → Clasificación → Asignación → Diagnóstico → Reasignación → Diagnóstico → Resolución → Cierre
Momento de predicción: después del primer "Diagnóstico"
Prefijo usable:  Registro → Clasificación → Asignación → Diagnóstico
```

Con el prefijo se pueden construir variables como:

- tiempo transcurrido hasta el momento de predicción;
- cantidad de actividades realizadas;
- si ya ocurrió determinada actividad;
- recurso o grupo asignado;
- hora o día de la semana del inicio.

---

## Señales de alerta

Sospeche que hay fuga si:

- el modelo obtiene métricas casi perfectas;
- la variable más importante es una duración total, un conteo sobre el caso completo o una actividad que solo ocurre al final;
- alguna variable se calcula con el mismo dato que define la variable objetivo;
- usó el atributo de resultado (`resultado_final`, `condicion_final`…) o un derivado de él como predictor.

## Otras formas de fuga

- **Preparación antes de separar:** escalar, imputar o balancear con **todo** el dataset y luego separar entrenamiento y prueba. Separe primero y ajuste las transformaciones solo con el conjunto de entrenamiento.
- **Fuga temporal:** entrenar con casos posteriores a los de prueba. Cuando el tiempo importa, conviene separar por fecha: entrenar con los casos más antiguos y probar con los más recientes.
- **Mismo caso en ambos conjuntos:** si el dataset tiene una fila por evento o por prefijo, todas las filas de un mismo caso deben quedar en el mismo conjunto.
- **Etiquetas censuradas:** casos sin tiempo suficiente de observación (por ejemplo, abiertos a la fecha de corte) no deben etiquetarse como "no ocurrió". Deben excluirse o tratarse aparte.

---

## Qué se espera en el Corte 3

En la sección de preparación de datos, la pareja debe:

1. indicar el momento de predicción;
2. listar las variables descartadas por fuga y explicar por qué;
3. explicar cómo separó los conjuntos de entrenamiento y prueba.
