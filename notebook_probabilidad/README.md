# 📐 Pruebas de hipótesis e inferencia estadística

El notebook [`Pruebas_hipotesis_probabilidad_Beta.ipynb`](Pruebas_hipotesis_probabilidad_Beta.ipynb) valida estadísticamente algunas ideas que usan los modelos predictivos. Cada prueba se calcula **integrando numéricamente la función de densidad (PDF)** de su distribución con `scipy`, en lugar de usar solo funciones ya hechas.

**Datos:** `data/processed/clean/Match_Clean.csv` y `Team_Clean.csv` (European Soccer Database, La Liga 2008-2016).

| # | Prueba | Distribución | Pregunta |
| --- | --- | --- | --- |
| 1 | Observaciones pareadas | T de Student | ¿Un equipo anota más goles como **local** que como visitante? (ventaja de localía) |
| 2 | Análisis de varianza (ANOVA) | F de Fisher | ¿Es distinto el promedio de goles entre **contendientes al título, media tabla y equipos en riesgo de descenso**? |
| 3 | Bondad de ajuste | Chi-cuadrada | ¿Los goles por partido siguen una **distribución de Poisson**? (base para simular partidos) |

En cada prueba se plantean la hipótesis nula y la alternativa, y se analizan los **errores tipo I y tipo II** y su impacto en el modelo predictivo.

> [!NOTE]
> El notebook se desarrolló en Google Colab y lee los datos desde `/content/drive/MyDrive/...`. Para ejecutarlo localmente, cambia las rutas por `../data/processed/clean/Match_Clean.csv` y `../data/processed/clean/Team_Clean.csv`.
