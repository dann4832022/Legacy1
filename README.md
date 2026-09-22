Legacy
Modulo6_Eejercicio_1

1. Orden y Secuencia de Transformaciones Realizadas
Extracción y Carga Inicial: Conexión desde la fuente legacy e ingreso directo a Power Query.
Depuración de Duplicados: Eliminación de registros repetidos utilizando la clave primaria de transacción.
Tratamiento de Nulos: Filtrado de registros incompletos en importes de ventas y reemplazo de valores faltantes en campos descriptivos.
Estandarización de Encabezados: Renombrado de nombres técnicos a estándar snake_case.
Tipificación de Variables: Asignación explícita de tipos de datos por columna.
Normalización (Modelo Estrella): Separación del tablón único en una tabla de hechos (FactVentas) y una tabla de dimensión (DimCliente).
2. Justificación Técnica de Tipos de Datos
id_transaccion / id_cliente (Texto): Aunque contengan números, los identificadores no representan cantidades operables matemáticamente. Definirlos como texto previene agregaciones accidentales (sumas, promedios) y optimiza el filtrado.
fecha_venta (Fecha): Esencial para habilitar inteligencia de tiempo (Time Intelligence en DAX) y la conexión futura con una tabla calendario dedicada.
**`La columna TOT_VENT original presentaba inconsistencias debido a la presencia de valores nulos provenientes del sistema legacy. Para resolverlo, se generó una nueva columna calculada multiplicando la cantidad vendida por el precio unitario (aplicando los descuentos correspondientes), garantizando que el 100% de los registros cuente con un importe correcto y sin vacíos. Se le asignó el tipo de dato Número decimal fijo (Moneda) para evitar errores de redondeo por coma flotante y asegurar precisión matemática exacta en la consolidación financiera del reporte.
3. Estrategia de Gestión de Nulos y Duplicados
Duplicados: Se aplicó eliminación de duplicados en la clave única de transacción. Un registro duplicado altera métricas de volumen transaccional e ingresos reales.
Nulos en Montos: Se duplico la columna TOT_VENT, se cambiò el nombre por TOTAL_VENTA, se genero una columna nueva calculada, cantidad_vendida * precio_unitario - descuento_porcentual.
Nulos en Atributos Secundarios: En campos como la email_cliente y telefono_cleinte, los valores nulos se imputaron con el texto "Sin telefono""Sin eamil", preservando la integridad conceptual sin perder el registro de la transacción.
