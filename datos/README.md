# Datos

Esta carpeta contiene un **event log sintético por cada caso**. Los datos fueron generados por simulación para este examen: no corresponden a personas, pacientes, clientes ni organizaciones reales.

```text
datos/
├── raw/          ← archivos originales: NO se modifican
└── processed/    ← aquí guarda la pareja sus versiones limpias o transformadas
```

> **Regla:** nunca edite los archivos de `raw/`. Toda limpieza debe hacerse mediante un script o notebook en `scripts/`, que lea `raw/` y escriba en `processed/`. Así el trabajo es reproducible.

---

## Formato común

- CSV separado por comas, codificación **UTF-8**, con encabezado.
- Cada fila es **un evento**.
- Las primeras cuatro columnas son iguales en todos los archivos:

| Columna | Descripción |
|---|---|
| `case_id` | Identificador de la instancia del proceso |
| `activity` | Nombre de la actividad registrada |
| `timestamp` | Fecha y hora del evento |
| `resource` | Persona, equipo, sistema o canal que ejecutó el evento |

- Algunas columnas describen **el evento** (cambian de fila en fila dentro de un caso).
- Otras describen **el caso** (se repiten en todas las filas del mismo `case_id`).
- Los archivos están ordenados por `timestamp` global, **no** por caso.
- Los datos corresponden a una **extracción realizada en una fecha de corte**. Algunos casos pueden no haber terminado a esa fecha.

> Los datos **no están limpios**. Como ocurre con extracciones reales, pueden contener problemas de calidad. Detectarlos, explicarlos y decidir qué hacer con ellos forma parte de la evaluación del Corte 2.

Lectura sugerida en Python:

```python
import pandas as pd
df = pd.read_csv("datos/raw/caso_02_pedidos.csv", encoding="utf-8")
```

---

## Caso 01 – Atención de pacientes

**Archivo:** `raw/caso_01_salud.csv` · aprox. 1 500 episodios · enero–junio 2026 · corte: 30/06/2026

| Columna | Nivel | Descripción |
|---|---|---|
| `tipo_examen` | evento | `Laboratorio` o `Imagenología`, solo en actividades de examen |
| `edad` | caso | Edad del paciente en años |
| `sexo` | caso | `F` / `M` |
| `prioridad` | caso | Prioridad asignada en triage: 1 (crítica) a 5 (no urgente) |
| `area` | caso | Área de atención |
| `resultado_final` | caso | `Alta`, `Hospitalización` o `Traslado` |

---

## Caso 02 – Gestión de pedidos

**Archivo:** `raw/caso_02_pedidos.csv` · aprox. 2 000 pedidos · marzo–junio 2026 · corte: 30/06/2026

| Columna | Nivel | Descripción |
|---|---|---|
| `categoria` | caso | Categoría principal del pedido |
| `monto` | caso | Monto del pedido (USD) |
| `metodo_pago` | caso | `Tarjeta`, `Transferencia` o `Contra entrega` |
| `region` | caso | Región de destino |
| `transportista` | caso | Empresa transportista asignada |
| `cantidad_productos` | caso | Número de artículos |
| `canal_venta` | caso | `Web`, `App` o `Teléfono` |
| `fecha_prometida` | caso | Fecha de entrega comprometida con el cliente (`AAAA-MM-DD`) |

Un pedido se considera **atrasado** si la entrega efectiva ocurre después de `fecha_prometida`.

---

## Caso 03 – Solicitud y aprobación de créditos

**Archivo:** `raw/caso_03_creditos.csv` · aprox. 1 200 solicitudes · enero–junio 2026 · corte: 30/06/2026

| Columna | Nivel | Descripción |
|---|---|---|
| `tipo_credito` | caso | `Personal`, `Consumo`, `Vehicular` o `Hipotecario` |
| `monto_solicitado` | caso | Monto solicitado (USD) |
| `ingreso_mensual` | caso | Ingreso mensual declarado (USD) |
| `nivel_endeudamiento` | caso | Proporción del ingreso comprometida en deudas (0 a 1) |
| `antiguedad_laboral_meses` | caso | Meses en el empleo actual |
| `canal` | caso | Canal de ingreso de la solicitud |
| `edad_solicitante` | caso | Edad en años |

Política interna de la institución: **toda solicitud aprobada debe pasar por "Análisis financiero"**. Las solicitudes con monto mayor a 30 000 USD o de tipo hipotecario deben pasar además por el **Comité de crédito**, que sesiona una vez por semana.

---

## Caso 04 – Mesa de servicio de TI

**Archivo:** `raw/caso_04_soporte_ti.csv` · aprox. 2 500 incidentes · febrero–junio 2026 · corte: 30/06/2026

| Columna | Nivel | Descripción |
|---|---|---|
| `grupo_asignado` | evento | Grupo de soporte responsable en ese momento |
| `prioridad` | caso | `P1` (crítica) a `P4` (baja) |
| `categoria` | caso | Categoría registrada del incidente |
| `canal` | caso | Canal de reporte |
| `departamento` | caso | Departamento del usuario |
| `sla_horas` | caso | Tiempo máximo comprometido según la prioridad (horas) |

El SLA se mide **en horas calendario desde "Incidente registrado" hasta la primera "Resolución"**.

---

## Caso 05 – Proceso académico universitario

**Archivo:** `raw/caso_05_academico.csv` · 600 estudiantes de un mismo curso · curso del 09/02/2026 al 31/05/2026 · corte: 05/06/2026

| Columna | Nivel | Descripción |
|---|---|---|
| `actividad_ref` | evento | Evaluación a la que corresponde el evento (`Tarea 1`…`Tarea 6`, `Parcial`, `Final`) |
| `nota` | evento | Calificación obtenida (0–100), solo en entregas y evaluaciones |
| `carrera` | caso | Carrera del estudiante |
| `modalidad` | caso | `Presencial` o `Virtual` |
| `trabaja` | caso | `Sí` / `No` |
| `promedio_previo` | caso | Promedio ponderado antes del curso |
| `edad` | caso | Edad en años |
| `condicion_final` | caso | `Aprobado`, `Reprobado` o `Abandono`, según el registro académico |

Aquí un caso es **un estudiante dentro de este curso**. La columna `resource` indica el medio de acceso (web, app o registro académico).

---

## Caso 06 – Mantenimiento de maquinaria

**Archivo:** `raw/caso_06_mantenimiento.csv` · aprox. 900 órdenes de trabajo · enero–junio 2026 · corte: 30/06/2026

| Columna | Nivel | Descripción |
|---|---|---|
| `equipo_id` | caso | Identificador del equipo (un mismo equipo puede tener varias órdenes) |
| `tipo_equipo` | caso | Tipo de máquina |
| `planta` | caso | Planta donde se ubica el equipo |
| `antiguedad_anios` | caso | Años de uso del equipo |
| `tipo_falla` | caso | Tipo de falla reportada |
| `criticidad` | caso | Importancia del equipo para la producción |
| `reparaciones_previas_12m` | caso | Órdenes anteriores del mismo equipo en los últimos 12 meses |
| `costo_total` | caso | Costo final de la orden (USD) |
| `reincidencia_30d` | caso | `Sí` si el mismo equipo volvió a fallar en los 30 días posteriores a su puesta en operación. Queda vacía cuando no hay 30 días de observación antes del corte |

El **tiempo fuera de servicio** corresponde al periodo en que el equipo no está operando. Defina con cuidado qué eventos lo delimitan.
