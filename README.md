# taller-calidad-datosGestionDatosG3
Maestria Analítica de datos taller-calidad-datos

Durante la fase inicial de auditoría y perfilado de datos (Data Profiling), se diagnosticaron múltiples problemas de calidad que impedían el uso del dataset para análisis fiables o modelos de Machine Learning. Apoyándonos en herramientas como missingno y ydata-profiling, los principales hallazgos fueron:

Inconsistencia en Tipos de Datos: Variables estrictamente métricas como Total Spent, Quantity y Price Per Unit fueron importadas como tipo objeto (cadenas de texto) debido a la presencia de registros corruptos con las palabras "ERROR" y "UNKNOWN".

Valores Faltantes (Nulos): Se identificó una alta densidad de nulos. Al realizar el conteo, se observó que aplicar técnicas de eliminación estricta (como dropna()) resultaría en la pérdida de más del 50% de los registros, lo cual mermaría drásticamente la representatividad de la muestra.

Distribuciones Atípicas: Se observaron variables numéricas con rangos de escala muy diferentes y presencia de valores atípicos (outliers) que requerían tratamiento previo a cualquier modelado.

Decisiones de Limpieza y Transformación Tomadas:
Para solucionar estos hallazgos sin sacrificar información valiosa, se optó por un enfoque de limpieza conservador y reproducible, alineado a las mejores prácticas de la industria:

Coerción Numérica: Se utilizó pd.to_numeric(errors='coerce') para forzar la conversión de las columnas afectadas a tipo flotante. Esta decisión permitió transformar automáticamente el texto inválido en valores NaN, unificando el problema para la siguiente etapa.

Imputación sobre Eliminación: Dado el volumen de pérdida potencial, se descartó la eliminación de filas/columnas. Los valores faltantes se trataron mediante imputación. Para variables transaccionales se utilizó el relleno secuencial (.bfill()), y para variables numéricas generales se aplicó SimpleImputer utilizando la mediana, garantizando resistencia ante los valores atípicos.

Escalado Robusto: Para el tratamiento de las variables numéricas, se evaluaron técnicas como MinMaxScaler y RobustScaler. Se seleccionó esta última debido a su capacidad para escalar los datos utilizando el rango intercuartílico.

Validación Declarativa: Finalmente, el proceso se blindó mediante la definición de un esquema de validación con pandera, comprobando de forma automatizada que el dataset resultante cumple con restricciones de negocio lógicas (ej. tipos de datos correctos y valores mayores a cero).
