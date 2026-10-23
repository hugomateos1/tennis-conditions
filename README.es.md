# tennis-conditions

**EN (short):** A relational database of ATP tournament conditions (altitude, weather, ball, estimated court speed, workload) joined with match results, to study how players perform under different conditions. Descriptive analysis, not betting predictions. Grand Slams and Masters 1000 first, active players only. Built in Python and SQL as a learning project.

---

## Qué es

Una base de datos de las **condiciones de cada torneo ATP** (altitud, clima, bola, velocidad estimada de la pista, carga de partidos y de viaje de los jugadores) cruzada con los resultados de los partidos, para analizar:

- cómo son las condiciones de cada torneo y cómo han cambiado año a año;
- cómo rinde cada jugador por encima o por debajo de lo esperado según las condiciones;
- cómo llega cada jugador a un torneo (forma reciente en la superficie).

Es un análisis **descriptivo**. No es un proyecto de pronósticos ni de apuestas.

## Estado

Fase 0 (preparación). Este README se irá actualizando con cada fase.

## Alcance

- La base de datos conserva todos los partidos individuales del cuadro principal del circuito ATP (hacen falta para calcular el Elo). El análisis y las publicaciones empiezan por los 4 Grand Slams, los 9 Masters 1000 y las ATP Finals (14 torneos).
- Partidos desde 2000 (para calcular el rendimiento esperado con Elo); condiciones y perfiles desde 2015.

## Cómo se analiza

1. **Ficha de torneo:** altitud, clima por edición, bola y velocidad estimada.
2. **Perfil de jugador por condiciones:** rendimiento sobre lo esperado agrupado por rasgos (pista rápida o lenta, altitud, calor o frío).
3. **Forma reciente:** media del rendimiento sobre lo esperado en los últimos partidos de la superficie, con más peso a los más recientes.

Los resultados se pueden agrupar por torneo, condiciones de pista, temporada, superficie o jugador.

**Reglas estadísticas:** mínimo de partidos por grupo, intervalos de confianza junto a cada cifra, cuidado con las comparaciones múltiples y lenguaje de perfiles, no de causas. "No hay evidencia suficiente" es un resultado válido.

## Datos y licencias

| Dato | Fuente | Licencia / condiciones |
| --- | --- | --- |
| Partidos, jugadores y rankings | Partidos y jugadores | [Base de datos de TennisMyLife (TML)](https://stats.tennismylife.org/tennis-match-database), basada en tennis_atp de Jeff Sackmann | Condiciones indicadas por la fuente: uso educativo, analítico y de investigación; sin redistribución ni uso comercial de los datos en bruto sin permiso. La web también la etiqueta como MIT; se trata como no comercial hasta aclararlo. Los archivos en bruto no se publican aquí. |
| Clima histórico | [Open-Meteo](https://open-meteo.com) | API gratuita solo para uso no comercial; atribución obligatoria |
| Altitud, bolas, cubierta o exterior, notas | Anotado a mano, cada dato con su fuente y fecha de consulta | Propio |



Atribuciones:

-  Datos de partidos: TennisMyLife (stats.tennismylife.org), basados en el trabajo de Jeff Sackmann / Tennis Abstract (CC BY-NC-SA 4.0).
- Weather data by [Open-Meteo.com](https://open-meteo.com).

Este proyecto no está afiliado a la ATP, a la ITF ni a ningún torneo.

## Estructura prevista

```
tennis-conditions/
├── data/
│   ├── raw/          # archivos descargados, sin tocar
│   ├── own/          # tablas propias: sedes, bolas, notas (con fuente y fecha)
│   └── db/           # base de datos SQLite
├── notebooks/        # exploración y gráficos
├── src/              # scripts: carga, limpieza, Elo, clima
└── README.md
```

## Modelo de datos (resumen)

`jugadores`, `sedes`, `torneos`, `ediciones`, `partidos`, `clima`, `rankings`, `elo`, `notas_bola`. Cada tabla incluye las columnas `fuente` y `licencia`.

## Hoja de ruta

1. Preparación: repositorio, entorno y descarga de datos.
2. Partidos y rendimiento esperado (Elo) en SQLite.
3. Sedes, clima y velocidad estimada de los 14 torneos.
4. Perfiles por condiciones y forma reciente.
5. Panel web con selectores para agrupar y mezclar.
6. Ampliar a ATP 500 y 250.

## Licencia del código

Por decidir (por ejemplo MIT). En este repositorio no se publican datos de partidos en bruto.

## Autor

[hugomateos1](https://github.com/hugomateos1) ·
