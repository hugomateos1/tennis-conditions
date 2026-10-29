# tennis-conditions

Una base de datos relacional de las condiciones de los torneos ATP (altitud, clima, bola, velocidad estimada de la pista, carga de partidos de los jugadores) cruzada con los resultados, para estudiar cómo rinden los jugadores en distintas condiciones.

Es un análisis descriptivo, no un proyecto de pronósticos ni de apuestas. Empieza por los Grand Slams, los Masters 1000 y las ATP Finals, solo jugadores activos, y está hecho en Python y SQL como proyecto de aprendizaje.

[English version](README.md)

## Qué es

Una base de datos de las **condiciones de cada torneo ATP** (altitud, clima, bola, velocidad estimada de la pista, carga de partidos y de viaje de los jugadores) cruzada con los resultados de los partidos, para analizar:

- cómo son las condiciones de cada torneo y cómo han cambiado año a año;
- cómo rinde cada jugador por encima o por debajo de lo esperado según las condiciones;
- cómo llega cada jugador a un torneo (forma reciente en la superficie).

## Estado

La Fase 1 (partidos y rendimiento esperado) está terminada: la base de datos de partidos, un Elo ajustado por superficie y las vistas de ranking ya están construidos. La Fase 2 ha empezado con una medida, ajustada por jugador, de cuánto favorece cada torneo al saque. Sedes, altitud, clima y bolas vienen después. Este README se actualiza con cada fase.

## Alcance

- Solo ATP masculino, individuales, cuadro principal.
- Quedan fuera: Challenger, Futures / ITF, clasificación (qualifying), WTA, dobles, Davis Cup, competiciones por equipos (United Cup, Laver Cup, ATP Cup), las Next Gen Finals y los Juegos Olímpicos.
- La base de datos conserva todos los partidos individuales del cuadro principal del circuito ATP (hacen falta para calcular el Elo). El análisis y las publicaciones empiezan por los 4 Grand Slams, los 9 Masters 1000 y las ATP Finals (14 torneos).
- Solo jugadores activos en lo que se publica (al menos un partido de cuadro principal en las últimas 52 semanas).
- Partidos desde 2000 (para calcular el rendimiento esperado con Elo); condiciones y perfiles desde 2015.

## Cómo se analiza

1. **Ficha de torneo:** altitud, clima por edición, bola y cuánto favorece el torneo al saque.
2. **Perfil de jugador por condiciones:** rendimiento sobre lo esperado agrupado por rasgos (pista rápida o lenta, altitud, calor o frío).
3. **Forma reciente:** resultados frente a lo esperado en los últimos partidos de la superficie.

Los resultados se pueden agrupar por torneo, condiciones de pista, temporada, superficie o jugador.

**Reglas estadísticas:** mínimo de partidos por grupo, intervalos de confianza junto a cada cifra, cuidado con las comparaciones múltiples y lenguaje de perfiles, no de causas. "No hay evidencia suficiente" es un resultado válido.

## Métodos (resumen)

**Ratings Elo.** Se calculan con los resultados del cuadro principal del circuito ATP desde 2000. Son ratings propios de este proyecto, no el ranking oficial de la ATP, y no son pronósticos.

- Cada jugador tiene un rating general más un ajuste por superficie (tierra, dura, hierba, moqueta). El resultado esperado de un partido usa la suma de ambos, y los dos se actualizan después de cada partido.
- Rating inicial 1450; K = 40 en los 30 primeros partidos de un jugador y 20 después; los ajustes por superficie usan un paso menor. Después, las probabilidades se calibran con una sola pendiente ajustada en los años anteriores.
- La evaluación usa una separación temporal: los parámetros se eligieron con 2005-2018 y se comprobaron con 2019-2026, con log loss, puntuación de Brier y tablas de calibración. Las variantes que no mejoraron (pesos por nivel de torneo, efectos de inactividad, un tope a las sorpresas, otras constantes de escala) no se adoptaron.
- Límites: en los datos solo hay partidos del cuadro principal ATP, así que no se reflejan los resultados de Challenger, clasificación, Davis Cup ni Juegos Olímpicos; un jugador que llega desde los Challengers entra sin historial.

**Ventaja al saque por torneo.** Para cada juego de servicio se estima cuántos puntos de saque debería ganar el sacador según cómo rinden ese sacador y ese restador en otros torneos de la misma superficie y cubierta (a menos de un año del partido, sin usar nunca el torneo que se mide). El valor de un torneo es cuánto superan o no alcanzan los sacadores esa expectativa, en puntos porcentuales de puntos de saque, con un intervalo por bootstrap sobre los partidos.

- Describe cuánto favorece un torneo al saque respecto al resto del circuito. No es velocidad de pista por sí sola: la superficie, la altitud, el calor, la bola y la cubierta influyen en la cifra, y los datos no pueden separarlos.
- Los intervalos tratan los partidos como independientes, así que la incertidumbre real es mayor que la mostrada.

**Forma reciente.** Se probó como predictor sobre el Elo ajustado por superficie y no aportó nada fuera de muestra, así que no se usa para predecir. Aun así puede mostrarse como vista descriptiva con una etiqueta de evidencia.

## Datos y licencias

| Dato | Fuente | Licencia / condiciones |
| --- | --- | --- |
| Partidos y jugadores | [Base de datos de TennisMyLife (TML)](https://stats.tennismylife.org/tennis-match-database) | Solo uso no comercial; el uso comercial requiere permiso previo y por escrito de TML. TML confirmó por correo (2026-10-27) que se pueden publicar estadísticas agregadas, gráficos y tablas en X y GitHub si el proyecto sigue siendo no comercial, no se redistribuyen los CSV originales ni los registros de partidos individuales, y se acredita a TML. |
| Clima histórico (previsto) | [Open-Meteo](https://open-meteo.com) | API gratuita solo para uso no comercial; atribución obligatoria |
| Altitud, bolas, cubierta o exterior, notas | Anotado a mano, cada dato con su fuente y fecha de consulta | Propio |

Todo lo calculado con los datos de partidos (Elo, ventaja al saque, perfiles) se trata como no comercial. Solo se publican estadísticas agregadas. Los archivos en bruto, la base de datos y los registros de partidos individuales no se publican.

Atribuciones:

- Fuente de datos: TennisMyLife — https://stats.tennismylife.org. TennisMyLife describe su base de datos como basada en el trabajo de Jeff Sackmann / Tennis Abstract (CC BY-NC-SA 4.0).
- Datos meteorológicos de [Open-Meteo.com](https://open-meteo.com).

Este proyecto no está afiliado a la ATP, a la ITF ni a ningún torneo.

## Estructura

```
tennis-conditions/
├── data/
│   ├── raw/          # archivos descargados, sin tocar (no se publican)
│   ├── own/          # tablas propias: torneos, sedes, bolas, notas (con fuente y fecha)
│   └── db/           # base de datos SQLite (no se publica)
├── notebooks/        # exploración, limpieza, SQL, Elo, forma y gráficos
├── charts/           # gráficos agregados, se añaden tras revisarlos
├── src/              # scripts (previstos)
└── README.md
```

## Modelo de datos (resumen)
## Modelo de datos (resumen)

Construidas hasta ahora: `matches`, `tournaments`, `players`, `elo_long` y tablas agregadas derivadas: `serve_advantage_by_year`, `serve_advantage_pooled`, `elo_ranking_snapshots` y `player_season_cards`.

Previstas: `venues`, `editions`, `weather`, `rankings`, `ball_notes`.
Cada tabla incluye las columnas `source` y `licence`, y las tablas propias se mantienen separadas de los datos de terceros.

## Hoja de ruta

1. Preparación: repositorio, entorno y descarga de datos. *(hecho)*
2. Partidos y rendimiento esperado (Elo) en SQLite. *(hecho)*
3. Sedes, clima y velocidad estimada de los 14 torneos. *(empezado: ventaja al saque construida; sedes, altitud y clima a continuación)*
4. Perfiles por condiciones y forma reciente.
5. Panel web con selectores para agrupar y mezclar.
6. Ampliar a ATP 500 y 250.

## Licencia del código

Por decidir (por ejemplo MIT). En este repositorio no se publican datos de partidos en bruto.

## Autor

[hugomateos1](https://github.com/hugomateos1)