# Scripts y notebooks

Toda la preparación de datos, el análisis de Process Mining y el Machine Learning se hacen **en Python, con notebooks de Google Colab** guardados en esta carpeta. Las instrucciones de configuración están en la sección *Entorno de trabajo* del [README principal](../README.md).

## Notebook de arranque

[00_inicio_pm4py.ipynb](00_inicio_pm4py.ipynb) muestra cómo cargar un event log, revisarlo, convertirlo al formato de pm4py y dibujar un primer mapa del proceso. **No resuelve el examen**: ábralo en Colab, guarde una copia con otro nombre y adáptela a su caso.

## Organización sugerida

```text
01_exploracion.ipynb
02_preparacion_event_log.ipynb
03_process_mining.ipynb
04_machine_learning.ipynb
```

## Reglas

- Todo notebook comienza con la **celda de configuración de Colab**, que clona el repositorio y se ubica en `scripts/`.
- Se lee siempre desde `../datos/raw/` y se escribe en `../datos/processed/`.
- Al entregar, los notebooks deben poder ejecutarse **de principio a fin** sin errores y en orden, en Colab (**Entorno de ejecución → Reiniciar y ejecutar todo**).
- Guarde los notebooks en GitHub (**Archivo → Guardar una copia en GitHub**) **con las salidas visibles**, para que el docente pueda revisar los resultados sin ejecutarlos.
- Cada integrante trabaja en su propio notebook a la vez: si dos personas guardan el mismo archivo, el último guardado reemplaza al anterior.
- Las figuras que use en las entregas pueden guardarse en `entregas/<corte>/img/`.
