# 📊 Datos de FBref (2006/07 – 2026/27)

Datos de La Liga descargados de [FBref](https://fbref.com/), un sitio público de estadísticas de fútbol. Cada carpeta tiene **un archivo CSV por temporada**, y `processed/` tiene todas las temporadas unidas.

> [!NOTE]
> La temporada 2026/27 **está en curso**: sus partidos pendientes aparecen sin marcador, y son los que el notebook 09 simula con Monte Carlo.

## 📁 Carpetas

| Carpeta | Contenido | Archivos | Un renglón es... |
| --- | --- | --- | --- |
| `partidos/` | Calendario y resultados | `partidos2006.csv` … `partidos2026.csv` | un partido |
| `equipos/` | Estadísticas generales de cada equipo | `statsgenerales06.csv` … `statsgenerales26.csv` | un equipo en una temporada |
| `jugadores/` | Estadísticas individuales | `jugadores06.csv` … `jugadores26.csv` | un jugador en un equipo y una temporada |
| `tabla general/` | Clasificación de la liga | `tabla0607.csv` … `tabla2627.csv` | un equipo en una temporada |
| `processed/` | Las cuatro tablas anteriores con todas las temporadas unidas (notebook 07) | `*_historico_completo.csv` | igual que su tabla de origen |
| `processed/clean/` | Versión limpia usada por los notebooks 09 y 10 (notebook 08, no está en GitHub) | `*_historico_limpio.csv` | igual que su tabla de origen |

**Nomenclatura de temporadas:** `partidos2006.csv`, `statsgenerales06.csv`, `jugadores06.csv` y `tabla0607.csv` corresponden a la misma temporada, **2006/07**. En los archivos consolidados la columna `Temporada` identifica el año.

## 📈 Volumen del consolidado

| Archivo | Registros aprox. |
| --- | --- |
| `partidos_historico_completo.csv` | ~7,980 partidos |
| `jugadores_historico_completo.csv` | ~11,760 registros jugador-temporada |
| `equipos_historico_completo.csv` | ~420 registros equipo-temporada |
| `tabla_general_historico_completo.csv` | ~420 registros equipo-temporada |

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
| `Player`, `Nation`, `Pos`, `Born` | *Solo jugadores:* nombre, nacionalidad, posición y año de nacimiento. |
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
| `Gls.1`, `Ast.1`, `G+A.1`, `G-PK.1`, `G+A-PK` | Las mismas métricas **por cada 90 minutos**. En la versión limpia se renombran con el sufijo `_per90`. |

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
