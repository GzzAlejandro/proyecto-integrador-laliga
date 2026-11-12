# ⚽ Proyecto Integrador: Analítica y Predicción de Resultados en La Liga

## 📖 Contexto y Problemática

En el ecosistema del fútbol moderno, la analítica deportiva es fundamental para la toma de decisiones estratégicas. Este proyecto aborda el siguiente reto: **¿Es posible predecir el resultado de un partido (Victoria, Empate, Derrota) basándose en la calidad individual, los resultados recientes y los atributos tácticos históricos de los equipos y sus jugadores?**

El proyecto nació trabajando con una base de datos relacional europea aislada a la liga española (temporadas 2008-2016) y, conforme avanzó, se amplió con una segunda fuente de datos de **FBref** que cubre **20+ temporadas (2006/07 – 2026/27)**. Con ello, el objetivo es construir un modelo matemático y probabilístico que integre ingeniería de datos en Python, inferencia estadística, algoritmos de clasificación, simulación Monte Carlo y una herramienta visual de inteligencia de negocios.

Este proyecto tiene el objetivo de demostrar que la ciencia de datos en el CUGDL no solo debe limitarse a conceptos de IA, sino que puede ser mucho más grande y variado que eso.

## 🗄️ Fuentes de Datos

El proyecto trabaja con dos fuentes complementarias:

### 1. European Soccer Database (2008-2016)

Base de datos relacional en SQLite (`data/raw/database.sqlite.zip`) con partidos, jugadores, equipos, atributos de EA Sports (FIFA) y cuotas de casas de apuestas. Se filtró exclusivamente a La Liga.

| Archivo limpio (`data/processed/clean/`) | Contenido |
| --- | --- |
| `Match_Clean.csv` | Resultados, cuotas y eventos del partido (faltas, tiros, tarjetas, posesión) extraídos del XML. |
| `Player_Clean.csv` | Catálogo de jugadores (peso en kg, año de nacimiento). |
| `Player_Attributes_Clean.csv` | Habilidades históricas de los jugadores con nulos imputados. |
| `Team_Clean.csv` | Catálogo de equipos de La Liga. |
| `Team_Attributes_Clean.csv` | Atributos tácticos de los equipos por temporada. |

> El detalle del proceso de limpieza está en [`data/processed/clean/README.md`](data/processed/clean/README.md).

### 2. FBref (2006/07 – 2026/27)

[FBref](https://fbref.com/) es un sitio de estadísticas de fútbol que publica datos de partidos, equipos y jugadores por temporada. Se exportaron los datos de La Liga temporada por temporada en CSV y después se consolidaron en una base histórica única.

| Carpeta (`data/fbref/`) | Contenido | Archivos |
| --- | --- | --- |
| `partidos/` | Calendario y resultados: jornada, fecha, local, marcador, visitante, asistencia, estadio y árbitro. | `partidos2006.csv` … `partidos2026.csv` |
| `equipos/` | Estadísticas generales por equipo: edad promedio, posesión, minutos, goles, asistencias, penales, tarjetas y métricas por 90 min. | `statsgenerales06.csv` … `statsgenerales26.csv` |
| `jugadores/` | Estadísticas individuales: nacionalidad, posición, edad, partidos, titularidades, minutos, goles, asistencias y tarjetas. | `jugadores06.csv` … `jugadores26.csv` |
| `tabla general/` | Clasificación final por temporada: PJ, G, E, P, GF, GC, DG, puntos, máximo goleador y portero. | `tabla0607.csv` … `tabla2627.csv` |
| `processed/` | Consolidado histórico de las cuatro fuentes anteriores (una columna `Temporada` identifica cada año). | `*_historico_completo.csv` |

**Volumen del consolidado histórico:**

| Archivo | Registros aprox. |
| --- | --- |
| `partidos_historico_completo.csv` | ~7,980 partidos |
| `jugadores_historico_completo.csv` | ~11,760 registros jugador-temporada |
| `equipos_historico_completo.csv` | ~420 registros equipo-temporada |
| `tabla_general_historico_completo.csv` | ~420 registros equipo-temporada |

> La temporada 2026/27 está en curso, por lo que sus partidos pendientes se usan como objetivo de la simulación Monte Carlo.

## 📂 Estructura del Repositorio

```
proyecto-integrador-laliga/
├── data/
│   ├── raw/                     # European Soccer Database (SQLite comprimido)
│   ├── processed/               # Tablas filtradas a La Liga + diccionario de datos
│   │   ├── clean/               # Tablas limpias listas para modelar
│   │   └── analisis de datos/   # Resúmenes de análisis de goles (Excel / Power BI)
│   └── fbref/                   # Datos de FBref por temporada y consolidado histórico
├── notebooks/                   # Flujo principal (limpieza → EDA → modelado)
├── notebook_probabilidad/       # Pruebas de hipótesis e inferencia estadística
└── comparisons/                 # Auditoría de predicciones del modelo vs. casas de apuestas
```

### Notebooks

| Notebook | Descripción |
| --- | --- |
| `01_limpieza_bases_de_datos` | Extracción y limpieza de la European Soccer Database. |
| `02_Bases_de_datos_La_liga` / `02.1_Diccionario_valores_nulos` | Filtrado a La Liga y análisis de valores nulos. |
| `03_validacion_y_diccionario_de_datos` | Validación de integridad y diccionario global de variables. |
| `04_ExploracionDeDatos` | Análisis Exploratorio de Datos (EDA). |
| `05_clasificacion_de_jugadores` | Agrupamiento de jugadores (K-Means). |
| `06_predictor_de_partidos` | Primera versión del predictor y comparación contra cuotas de apuestas. |
| `07_Analisis_de_datos_la_liga_clean` / `07_exploracion_base_de_datos` | Análisis de goles y exploración de las tablas limpias. |
| `08_EDA_limpieza` | Limpieza y EDA de la base de datos de FBref. |
| `09_ML_Final` | Pipeline final de Machine Learning y simulación Monte Carlo. |
| `Pruebas_hipotesis_probabilidad_Beta` | Pruebas de hipótesis (ventaja de localía, etc.) con integración numérica de las PDF. |

## 🤖 Modelado (estado actual)

El pipeline de `09_ML_Final.ipynb` trabaja sobre los datos de FBref:

1. **Variables sin fuga de información (*data leakage*):** promedios móviles de los últimos 5 partidos (goles a favor, goles en contra, puntos y puntos como local/visitante), calculados únicamente con la información disponible **antes** de cada partido.
2. **Validación temporal:** se entrena con las temporadas anteriores a 2025 y se valida con la temporada 2025.
3. **Comparación de modelos:** Naive Bayes, Regresión Logística, Árbol de Decisión, Random Forest y XGBoost, evaluados con Accuracy, Precision, Recall y F1 macro.
4. **Probabilidades calibradas:** clasificación multiclase (Local / Empate / Visitante) con probabilidades calibradas (Log Loss y Brier score).
5. **Simulación Monte Carlo:** se simulan los partidos pendientes de la temporada en curso para estimar puntos esperados y la probabilidad de cada equipo de ser **campeón**, clasificar a **Champions** o **descender**.

Además, en `comparisons/Auditoria_ML_vs_CasasApuestas.csv` se contrastan las probabilidades del modelo con las cuotas de Bet365, Interwetten, William Hill y VC Bet.

## 📌 Fases de Desarrollo y Entregables

- [x] **Fase 1:** Extracción, limpieza y normalización de las bases de datos (European Soccer Database + FBref).
- [x] **Fase 2:** Análisis Exploratorio de Datos (EDA), correlación de variables y pruebas de hipótesis.
- [ ] **Fase 3:** Modelado de Machine Learning y simulación Monte Carlo — *primera versión final lista, sujeta a ajustes.*
- [ ] **Fase 4:** Diseño del Dashboard interactivo en Power BI (visualización de datos).
- [ ] **Fase 5:** Redacción del Reporte Técnico (desarrollo matemático, métricas y evaluaciones).
- [ ] **Fase 6:** Diseño del Póster Académico (justificando la metodología y perspectivas futuras).

## 🛠️ Tecnologías

Python · Pandas · NumPy · SciPy · scikit-learn · XGBoost · Matplotlib · Seaborn · Jupyter · Power BI

## 👥 Equipo de Trabajo

- **Oscar Alejandro González Carrillo** - [@GzzAlejandro](https://github.com/GzzAlejandro)
- **Edgar Yamil Torres Bautista** - [@Yami-Torres](https://github.com/Yami-Torres)
- **De Loza Martinez Alejandro** - [@Alex-De-Loza](https://github.com/Alex-De-Loza)
- **Alberto Jared Ramos Medina** - [@albertoramos7282-eng](https://github.com/albertoramos7282-eng)
