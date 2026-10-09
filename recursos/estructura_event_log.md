# Estructura mínima de un Event Log

Un Event Log debe permitir reconstruir el orden de los eventos ocurridos en cada instancia del proceso.

Como mínimo debe contener:

| Campo | Función |
|---|---|
| Case ID | Identifica una instancia del proceso |
| Activity | Identifica la actividad realizada |
| Timestamp | Indica cuándo ocurrió el evento |

Ejemplo:

```csv
case_id,activity,timestamp,recurso
P001,Pedido recibido,2026-01-15 08:10:00,Sistema
P001,Validación,2026-01-15 08:35:00,Usuario01
P001,Pago aprobado,2026-01-15 09:12:00,Sistema
P001,Preparación,2026-01-15 10:25:00,Bodega
```

## Verificaciones recomendadas

Antes del análisis, revise:

- que cada evento tenga un Case ID;
- que las actividades estén estandarizadas;
- que los timestamps sean válidos;
- que los eventos puedan ordenarse cronológicamente;
- que no existan duplicados injustificados;
- que los atributos adicionales tengan sentido para el análisis.
