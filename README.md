# taller-calidad-datosGestionDatosG3
Maestria Analítica de datos taller-calidad-datos

# Resumen del taller calidad y limpieza de datos
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


#Concluciones
La auditoría de calidad demostró que operar sobre un dataset sin perfilado previo conduce invariablemente a errores analíticos graves, ya que simples inconsistencias de texto (como "ERROR" en campos numéricos) rompen los cálculos agregados. Además, se evidenció empíricamente que métodos agresivos de limpieza como la eliminación de nulos diezman la representatividad de la base de datos con pérdidas superiores al 50%, haciendo de la imputación estructurada y del uso de transformadores robustos como RobustScaler y PowerTransformer, herramientas indispensables para retener el volumen y corregir el comportamiento atípico de las ventas. Finalmente, la integración de "contratos de datos" con herramientas como pandera representa un cambio de paradigma necesario, pasando de revisiones visuales a validaciones automatizadas que evitan futuras regresiones en la calidad.

#Recomendaciones
A partir de esta práctica con el dataset de ejemplo, la principal recomendación es adoptar siempre esta metodología paso a paso antes de analizar cualquier conjunto de datos real. Hemos comprobado que la limpieza no debe hacerse de forma improvisada; es vital iniciar siempre con un diagnóstico visual y estadístico para entender los errores. Además, se sugiere evitar la eliminación masiva de datos usando técnicas de imputación en su lugar y utilizar siempre herramientas de validación como pandera para asegurar que el resultado final sea correcto. Este ejercicio demuestra que el éxito de cualquier análisis de datos o modelo predictivo futuro depende enteramente de la paciencia, el orden y el rigor con los que preparemos la información en esta etapa inicial.

