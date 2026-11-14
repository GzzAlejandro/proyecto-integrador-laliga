# 📊 Datos de FBref (2006/07 – 2026/27)

Datos de La Liga descargados de [FBref](https://fbref.com/), un sitio público de estadísticas de fútbol. Cada carpeta tiene **un archivo CSV por temporada**, y `processed/` tiene todas las temporadas unidas.

> [!NOTE]
> La temporada 2026/27 **está en curso**: sus `260` partidos pendientes aparecen sin marcador en los datos históricos y son los que el notebook 09 simula con Monte Carlo y muestra el dashboard en Power BI.

## 📁 Carpetas

| Carpeta | Contenido | Archivos | Un renglón es... |
| --- | --- | --- | --- |
| `partidos/` | Calendario y resultados | `partidos2006.csv` … `partidos2026.csv` | un partido |
| `equipos/` | Estadísticas generales de cada equipo | `statsgenerales06.csv` … `statsgenerales26.csv` | un equipo en una temporada |
| `jugadores/` | Estadísticas individuales | `jugadores06.csv` … `jugadores26.csv` | un jugador en un equipo y una temporada |
| `tabla general/` | Clasificación de la liga | `tabla0607.csv` … `tabla2627.csv` | un equipo en una temporada |
| `processed/` | Las cuatro tablas anteriores con todas las temporadas unidas (notebook 07) | `*_historico_completo.csv` | igual que su tabla de origen |
| `processed/clean/` | Versión limpia (notebook 08) y predicciones ML (notebook 09) listas para Power BI | `*_historico_limpio.csv`, `partidos_por_equipo_limpio.csv`, `Predicciones_MonteCarlo_2026.csv`, `Predicciones_Partidos_2026.csv` | tablas analíticas y salidas predictivas |

**Nomenclatura de temporadas:** `partidos2006.csv`, `statsgenerales06.csv`, `jugadores06.csv` y `tabla0607.csv` corresponden a la misma temporada, **2006/07**. En los archivos consolidados la columna `Temporada` identifica el año.

## 📈 Volumen del consolidado y limpio

| Archivo | Registros aprox. / exactos |
| --- | --- |
| `partidos_historico_completo.csv` | ~7,980 partidos (incluye `260` pendientes de `2026/27`) |
| `processed/clean/partidos_historico_limpio.csv` | `7,720` partidos jugados |
| `processed/clean/partidos_por_equipo_limpio.csv` | `15,440` filas equipo-partido (`7,720 × 2`) |
| `processed/clean/jugadores_historico_limpio.csv` | `11,765` registros jugador-temporada |
| `processed/clean/equipos_historico_limpio.csv` | `420` registros equipo-temporada |
| `processed/clean/tabla_general_historico_limpio.csv` | `420` registros equipo-temporada |
| `processed/clean/Predicciones_MonteCarlo_2026.csv` | `20` equipos proyectados (`2026/27`) |
| `processed/clean/Predicciones_Partidos_2026.csv` | `260` partidos pendientes (`2026/27`) |

## 📖 Diccionario de columnas

### Partidos

| Columna | Significado |
| --- | --- |
| `Temporada` | Año de inicio de la temporada. |
| `Wk` | Jornada. |
| `Day`, `Date`, `Time` | Día de la semana, fecha y hora. |
| `Home` / `Away` | Equipo local / visitante. |
| `Score` | Marcador (`local–visitante`). Vacío si el partido no se ha jugado. |
| `Attendance` | Asistencia al estadio. |
| `Venue` | Estadio. |
| `Referee` | Árbitro. |
| `Goles_Local`, `Goles_Visita`, `Puntos_Local`, `Puntos_Visita` | *Solo en la versión limpia:* marcador separado y puntos de cada equipo (3 victoria, 1 empate, 0 derrota). |

### Equipos y jugadores

| Columna | Significado |
| --- | --- |
| `Squad` | Equipo. |
| `Player`, `Nation`, `Pos`, `Born` | *Solo jugadores:* nombre, nacionalidad, posición y año de nacimiento (`Pos_Principal` en la versión limpia). |
| `# Pl` | *Solo equipos:* número de jugadores utilizados. |
| `Age` | Edad (promedio en el caso de equipos). |
| `Poss` | *Solo equipos:* posesión promedio (%). |
| `MP` | Partidos jugados. |
| `Starts` | Partidos como titular. |
| `Min` | Minutos jugados. |
| `90s` | Minutos jugados divididos entre 90. |
| `Gls` / `Ast` | Goles / asistencias. |
| `G+A` | Goles + asistencias. |
| `G-PK` | Goles sin contar penales. |
| `PK` / `PKatt` | Penales anotados / intentados. |
| `CrdY` / `CrdR` | Tarjetas amarillas / rojas. |
| `Gls.1`, `Ast.1`, `G+A.1`, `G-PK.1`, `G+A-PK` | Las mismas métricas **por cada 90 minutos**. En la versión limpia de equipos se renombran con el sufijo `_per90`. |

### Tabla general

| Columna | Significado |
| --- | --- |
| `Rk` | Posición en la tabla. |
| `MP`, `W`, `D`, `L` | Partidos jugados, ganados, empatados y perdidos. |
| `GF`, `GA`, `GD` | Goles a favor, en contra y diferencia de goles. |
| `Pts`, `Pts/MP` | Puntos totales y puntos por partido. |
| `Attendance` | Asistencia promedio como local. |
| `Top Team Scorer` | Máximo goleador del equipo. |
| `Goalkeeper` | Portero con más minutos. |
| `Notes` | Clasificación a competiciones europeas o descenso. |
| `Last 5` | Resultados de los últimos 5 partidos. |
