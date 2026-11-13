# 🗄️ Datos

Esta carpeta contiene todos los datos del proyecto, organizados según su fuente y su nivel de procesamiento.

```
data/
├── raw/                         → Datos originales, sin modificar
│   └── database.sqlite.zip      → European Soccer Database (Kaggle)
├── processed/                   → European Soccer Database filtrada a La Liga
│   ├── *_LaLiga.csv             → Tablas extraídas (notebook 01)
│   ├── Diccionario_Global.csv   → Diccionario de columnas (notebook 03)
│   ├── Evaluacion_Apuestas_Binario.csv → Modelo vs. casas de apuestas (notebook 06)
│   ├── clean/                   → Tablas limpias listas para modelar (notebook 04)
│   └── analisis de datos/       → Resúmenes de goles en Excel (notebook 07)
└── fbref/                       → Datos de FBref (2006/07 – 2026/27)
    ├── partidos/  equipos/  jugadores/  tabla general/   → Un CSV por temporada
    └── processed/               → Consolidado histórico (notebook 07)
        └── clean/               → Versión limpia usada por el modelo final (notebook 08)*
```

\* *Se genera localmente; no está en GitHub (ver más abajo).*

## 1. European Soccer Database (2008/09 – 2015/16)

Base de datos relacional publicada en [Kaggle](https://www.kaggle.com/datasets/hugomathien/soccer) con más de 25,000 partidos de 11 ligas europeas, atributos de jugadores y equipos tomados del videojuego **FIFA (EA Sports)** y cuotas de varias casas de apuestas. En este proyecto se filtró solo **La Liga**.

| Etapa | Archivos | Notebook que los genera |
| --- | --- | --- |
| Original | `raw/database.sqlite.zip` | — |
| Filtrado a La Liga | `processed/Match_LaLiga.csv`, `Team_LaLiga.csv`, `Team_Attributes_LaLiga.csv`, `Player_LaLiga.csv`, `Player_Attributes_LaLiga.csv` | `01_limpieza_bases_de_datos` |
| Diccionario de datos | `processed/Diccionario_Global.csv` | `03_validacion_y_diccionario_de_datos` |
| Limpio | `processed/clean/*_Clean.csv` | `04_ExploracionDeDatos` |
| Análisis de goles | `processed/analisis de datos/*.xlsx` | `07_Analisis_de_datos_la_liga_clean` |
| Evaluación vs. apuestas | `processed/Evaluacion_Apuestas_Binario.csv` | `06_predictor_de_partidos` |

📄 Los detalles de la limpieza (imputación de nulos, variables eliminadas, etc.) están en [`processed/clean/README.md`](processed/clean/README.md).

## 2. FBref (2006/07 – 2026/27)

[FBref](https://fbref.com/) es un sitio público de estadísticas de fútbol. Se descargaron los datos de La Liga **temporada por temporada** y después se unieron en una base histórica. Incluye la **temporada en curso**, lo que permite proyectar cómo terminará.

| Etapa | Archivos | Notebook que los genera |
| --- | --- | --- |
| Por temporada | `fbref/partidos/`, `fbref/equipos/`, `fbref/jugadores/`, `fbref/tabla general/` | Descarga manual desde FBref |
| Consolidado | `fbref/processed/*_historico_completo.csv` | `07_exploracion_base_de_datos` |
| Limpio | `fbref/processed/clean/*_historico_limpio.csv` | `08_EDA_limpieza` |

📄 La descripción de cada carpeta y sus columnas está en [`fbref/README.md`](fbref/README.md).

## ¿Por qué hay dos fuentes?

El primer predictor de partidos (notebook 06) se construyó con la European Soccer Database, pero esta base **termina en 2016** y no permite predecir temporadas actuales. Por eso el **modelo final** (notebook 09) se construyó con FBref, que tiene 20 temporadas más y la temporada en curso. La European Soccer Database sigue siendo la fuente del **KNN de posiciones** (notebook 05), porque es la única que tiene los atributos FIFA de los jugadores.

## ⚠️ Archivos que no están en GitHub

El `.gitignore` excluye todos los `.csv` excepto los de `data/processed/` y `data/processed/clean/` (y los de FBref que ya estaban en el repositorio). Por eso, al clonar el proyecto, **faltan**:

| Archivo(s) | Cómo obtenerlos |
| --- | --- |
| `raw/Match.csv`, `raw/Player.csv`, `raw/Team.csv`, etc. | Descomprime `raw/database.sqlite.zip` y exporta cada tabla de la base SQLite a CSV. |
| `fbref/processed/clean/*_historico_limpio.csv` | Ejecuta el notebook `08_EDA_limpieza`. |
