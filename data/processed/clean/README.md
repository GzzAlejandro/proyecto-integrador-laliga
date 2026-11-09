#  Processed Data - European Soccer Database

Este directorio contiene los datasets procesados y limpios, listos para la fase de modelado predictivo y aprendizaje no supervisado (K-Means). Los datos originales han sido extraídos, transformados y validados enfocándose exclusivamente en los registros de La Liga.

##  Diccionario de Archivos

| Archivo CSV | Descripción de los Datos | Transformaciones Destacadas | 
| ----- | ----- | ----- | 
| `Player_Clean.csv` | Catálogo e identificadores de jugadores. | Conversión métrica de peso (lbs a kg), formateo `datetime` y extracción de `birth_year`. | 
| `Player_Attributes_Clean.csv` | Habilidades y métricas históricas. | Imputación de nulos, remoción de duplicados de sistema y exclusión de variables categóricas. | 
| `Match_Clean.csv` | Resultados, cuotas y métricas de partidos. | Extracción vía Regex de eventos anidados en XML (faltas, tiros, tarjetas, posesión). | 
| `Team_Clean.csv` | Catálogo de equipos de La Liga. | Auditoría de integridad (cero nulos o nombres faltantes). | 
| `Team_Attributes_Clean.csv` | Atributos tácticos por temporada. | Eliminación de variables de texto redundantes (etiquetas terminadas en `Class`). | 

##  Tratamiento de Datos (EDA & Data Cleaning)

Para garantizar la viabilidad matemática de los algoritmos de Machine Learning y evitar la propagación de ruido, se implementaron las siguientes lógicas de limpieza:

* **Manejo de Nulos Históricos (Caso de Estudio: F. Cannavaro):**
  Se detectó una ausencia sistemática de registros (111 filas) en variables como `volleys`, `vision` y `jumping` correspondientes a los años 2007-2010. Para no perder la información de élite de jugadores consolidados, se aplicó una imputación de dos niveles:

  1. Mediana histórica individual del jugador.

  2. Mediana global de la liga (si el jugador no tenía historial previo).

* **Feature Selection:**
* Las columnas de esfuerzo (`attacking_work_rate`, `defensive_work_rate`) fueron eliminadas de la matriz final. Su alta proporción de texto corrupto y naturaleza categórica introducían ruido innecesario para los cálculos de distancias euclidianas requeridos por el modelo K-Means.

* **Integridad Temporal:**
*  Se determinó que las evaluaciones tácticas deben analizarse estrictamente por temporada (evitando promedios globales) debido a la alta volatilidad del simulador de EA Sports.


* \[ \] Implementación y entrenamiento del modelo **K-Means**.

* \[ \] Evaluación de métricas (Método del Codo, Coeficiente de Silueta).
