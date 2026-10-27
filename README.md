# tennis-conditions

A relational database of ATP tournament conditions (altitude, weather, ball, estimated court speed, player workload) joined with match results, to study how players perform under different conditions.

This is a descriptive analysis, not a betting or forecasting project. It starts with the Grand Slams, the Masters 1000 events and the ATP Finals, active players only, and is built in Python and SQL as a learning project.

[Versión en español](README.es.md)

## What it is

A database of the **conditions at each ATP tournament** (altitude, weather, ball, estimated court speed, match load and travel for players) combined with match results, to analyse:

- what the conditions at each tournament are like and how they have changed year to year;
- how each player performs above or below expectation depending on the conditions;
- how each player arrives at a tournament (recent form on the surface).

## Status

Phase 1 (matches and expected performance): the match database and a basic Elo rating are built. Condition data (venues, weather, ball) comes next. This README is updated with each phase.

## Scope

- ATP men's singles, main draw only.
- Excluded: Challenger, Futures / ITF, qualifying, WTA, doubles, Davis Cup, team events (United Cup, Laver Cup, ATP Cup), the Next Gen Finals and the Olympics.
- The database keeps all ATP tour-level main-draw matches (needed to compute Elo ratings). Analysis and publications start with the 4 Grand Slams, the 9 Masters 1000 events and the ATP Finals (14 events).
- Active players only in what is published (at least one main-draw match in the last 52 weeks).
- Matches from 2000 (needed to compute expected performance with Elo); conditions and player profiles from 2015.

## How it is analysed

1. **Tournament card:** altitude, weather per edition, ball and estimated court speed.
2. **Player profile by conditions:** performance above expectation, grouped by traits (fast or slow court, altitude, hot or cold).
3. **Recent form:** average performance above expectation over the most recent matches on the surface, with more weight on the latest ones.

Results can be grouped by tournament, court conditions, season, surface or player.

**Elo ratings** are calculated from ATP tour-level main-draw results since 2000. They are this project's own ratings, not the official ATP ranking, and they are not predictions.

**Statistical rules:** minimum number of matches per group, confidence intervals next to every figure, care with multiple comparisons, and the language of profiles rather than causes. "Not enough evidence" is a valid result.

## Data and licences

| Data | Source | Licence / terms |
| --- | --- | --- |
| Matches and players | [TennisMyLife (TML) database](https://stats.tennismylife.org/tennis-match-database) | Non-commercial use only; commercial use needs TML's prior written permission. TML confirmed by email (2026-10-27) that aggregate statistics, charts and tables may be published on X and GitHub if the project stays non-commercial, the original CSV files and individual match records are not redistributed, and TML is credited. |
| Historical weather (planned) | [Open-Meteo](https://open-meteo.com) | Free API for non-commercial use only; attribution required |
| Altitude, balls, indoor or outdoor, notes | Collected by hand, each item with its source and date accessed | Own work |

Everything calculated from the match data (Elo, estimated speed, profiles) is treated as non-commercial. Only aggregate statistics are published. The raw files and individual match records are not.

Attributions:

- Data source: TennisMyLife — https://stats.tennismylife.org. TennisMyLife describes its database as based on the work of Jeff Sackmann / Tennis Abstract (CC BY-NC-SA 4.0).
- Weather data by [Open-Meteo.com](https://open-meteo.com).

This project is not affiliated with the ATP, the ITF or any tournament.

## Planned structure

```
tennis-conditions/
├── data/
│   ├── raw/          # downloaded files, untouched (not published)
│   ├── own/          # my own tables: tournaments, venues, balls, notes (with source and date)
│   └── db/           # SQLite database (not published)
├── notebooks/        # exploration, cleaning, SQL and Elo
├── src/              # scripts: loading, cleaning, Elo, weather
└── README.md
```

## Data model (summary)

Built so far: `matches`, `tournaments`, `players`, `elo_long`.
Planned: `venues`, `editions`, `weather`, `rankings`, `ball_notes`.

Every table includes `source` and `licence` columns, and tables of my own are kept separate from third-party data.

## Roadmap

1. Setup: repository, environment and data download.
2. Matches and expected performance (Elo) in SQLite. *(in progress)*
3. Venues, weather and estimated speed for the 14 tournaments.
4. Profiles by conditions and recent form.
5. Web dashboard with selectors to group and mix.
6. Extend to ATP 500 and 250.

## Code licence

To be decided (for example MIT). No raw match data is published in this repository.

## Author

[hugomateos1](https://github.com/hugomateos1)