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

Fase 1 (partidos y rendimiento esperado): la base de datos de partidos y un Elo básico ya están construidos. Los datos de condiciones (sedes, clima, bola) vienen después. Este README se actualiza con cada fase.

## Alcance

- Solo ATP masculino, individuales, cuadro principal.
- Quedan fuera: Challenger, Futures / ITF, clasificación (qualifying), WTA, dobles, Davis Cup, competiciones por equipos (United Cup, Laver Cup, ATP Cup), las Next Gen Finals y los Juegos Olímpicos.
- La base de datos conserva todos los partidos individuales del cuadro principal del circuito ATP (hacen falta para calcular el Elo). El análisis y las publicaciones empiezan por los 4 Grand Slams, los 9 Masters 1000 y las ATP Finals (14 torneos).
- Solo jugadores activos en lo que se publica (al menos un partido de cuadro principal en las últimas 52 semanas).
- Partidos desde 2000 (para calcular el rendimiento esperado con Elo); condiciones y perfiles desde 2015.

## Cómo se analiza

1. **Ficha de torneo:** altitud, clima por edición, bola y velocidad estimada.
2. **Perfil de jugador por condiciones:** rendimiento sobre lo esperado agrupado por rasgos (pista rápida o lenta, altitud, calor o frío).
3. **Forma reciente:** media del rendimiento sobre lo esperado en los últimos partidos de la superficie, con más peso a los más recientes.

Los resultados se pueden agrupar por torneo, condiciones de pista, temporada, superficie o jugador.

**Los ratings Elo** se calculan con los resultados del cuadro principal del circuito ATP desde 2000. Son ratings propios de este proyecto, no el ranking oficial de la ATP, y no son pronósticos.

**Reglas estadísticas:** mínimo de partidos por grupo, intervalos de confianza junto a cada cifra, cuidado con las comparaciones múltiples y lenguaje de perfiles, no de causas. "No hay evidencia suficiente" es un resultado válido.

## Datos y licencias

| Dato | Fuente | Licencia / condiciones |
| --- | --- | --- |
| Partidos y jugadores | [Base de datos de TennisMyLife (TML)](https://stats.tennismylife.org/tennis-match-database) | Solo uso no comercial; el uso comercial requiere permiso previo y por escrito de TML. TML confirmó por correo (2026-10-27) que se pueden publicar estadísticas agregadas, gráficos y tablas en X y GitHub si el proyecto sigue siendo no comercial, no se redistribuyen los CSV originales ni los registros de partidos individuales, y se acredita a TML. |
| Clima histórico (previsto) | [Open-Meteo](https://open-meteo.com) | API gratuita solo para uso no comercial; atribución obligatoria |
| Altitud, bolas, cubierta o exterior, notas | Anotado a mano, cada dato con su fuente y fecha de consulta | Propio |

Todo lo calculado con los datos de partidos (Elo, velocidad estimada, perfiles) se trata como no comercial. Solo se publican estadísticas agregadas. Los archivos en bruto y los registros de partidos individuales no se publican.

Atribuciones:

- Fuente de datos: TennisMyLife — https://stats.tennismylife.org. TennisMyLife describe su base de datos como basada en el trabajo de Jeff Sackmann / Tennis Abstract (CC BY-NC-SA 4.0).
- Datos meteorológicos de [Open-Meteo.com](https://open-meteo.com).

Este proyecto no está afiliado a la ATP, a la ITF ni a ningún torneo.

## Estructura prevista

```
tennis-conditions/
├── data/
│   ├── raw/          # archivos descargados, sin tocar (no se publican)
│   ├── own/          # tablas propias: torneos, sedes, bolas, notas (con fuente y fecha)
│   └── db/           # base de datos SQLite (no se publica)
├── notebooks/        # exploración, limpieza, SQL y Elo
├── src/              # scripts: carga, limpieza, Elo, clima
└── README.md
```

## Modelo de datos (resumen)

Construidas hasta ahora: `matches`, `tournaments`, `players`, `elo_long`.
Previstas: `venues`, `editions`, `weather`, `rankings`, `ball_notes`.

Cada tabla incluye las columnas `source` y `licence`, y las tablas propias se mantienen separadas de los datos de terceros.

## Hoja de ruta

1. Preparación: repositorio, entorno y descarga de datos.
2. Partidos y rendimiento esperado (Elo) en SQLite. *(en curso)*
3. Sedes, clima y velocidad estimada de los 14 torneos.
4. Perfiles por condiciones y forma reciente.
5. Panel web con selectores para agrupar y mezclar.
6. Ampliar a ATP 500 y 250.

## Licencia del código

Por decidir (por ejemplo MIT). En este repositorio no se publican datos de partidos en bruto.

## Autor

[hugomateos1](https://github.com/hugomateos1)