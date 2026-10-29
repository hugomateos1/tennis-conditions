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

Phase 1 (matches and expected performance) is done: the match database, a surface-adjusted Elo rating and ranking views are built. Phase 2 has started with a player-adjusted measure of how serve-friendly each tournament is. Venues, altitude, weather and balls come next. This README is updated with each phase.

## Scope

- ATP men's singles, main draw only.
- Excluded: Challenger, Futures / ITF, qualifying, WTA, doubles, Davis Cup, team events (United Cup, Laver Cup, ATP Cup), the Next Gen Finals and the Olympics.
- The database keeps all ATP tour-level main-draw matches (needed to compute Elo ratings). Analysis and publications start with the 4 Grand Slams, the 9 Masters 1000 events and the ATP Finals (14 events).
- Active players only in what is published (at least one main-draw match in the last 52 weeks).
- Matches from 2000 (needed to compute expected performance with Elo); conditions and player profiles from 2015.

## How it is analysed

1. **Tournament card:** altitude, weather per edition, ball and how serve-friendly the tournament is.
2. **Player profile by conditions:** performance above expectation, grouped by traits (fast or slow court, altitude, hot or cold).
3. **Recent form:** results against expectation over the most recent matches on the surface.

Results can be grouped by tournament, court conditions, season, surface or player.

**Statistical rules:** minimum number of matches per group, confidence intervals next to every figure, care with multiple comparisons, and the language of profiles rather than causes. "Not enough evidence" is a valid result.

## Methods (summary)

**Elo ratings.** Calculated from ATP tour-level main-draw results since 2000. They are this project's own ratings, not the official ATP ranking, and they are not predictions.

- Each player has an overall rating plus a surface offset (clay, hard, grass, carpet). The expected result of a match uses the sum of the two, and both are updated after each match.
- Start rating 1450; K = 40 for a player's first 30 matches and 20 afterwards; surface offsets use a smaller step. The probabilities are then calibrated with a single slope fitted on the earlier years.
- Evaluation uses a time split: parameters were chosen on 2005-2018 and checked on 2019-2026, with log loss, Brier score and calibration tables. Variants that did not help (tournament-level weights, inactivity effects, a capped upset term, other scale constants) were not adopted.
- Limits: only ATP main-draw matches are in the data, so a player's Challenger, qualifying, Davis Cup and Olympic results are not reflected; a player coming through the Challengers enters with no history.

**Serve advantage by tournament.** For every service game, the number of service points the server should win is estimated from how that server and that returner perform at other events on the same surface and roof (within one year of the match, never using the event being measured). A tournament's value is how far servers beat or fall short of that expectation, in percentage points of service points, with a bootstrap interval over matches.

- It describes how serve-friendly a tournament is relative to the rest of the tour. It is not court speed on its own: surface, altitude, heat, ball and roof all push the number, and the data cannot separate them.
- The intervals treat matches as independent, so the real uncertainty is larger than shown.

**Recent form.** Tested as a predictor on top of the surface-adjusted Elo and found to add nothing out of sample, so it is not used to predict. It can still be shown as a descriptive view with an evidence label.

## Data and licences

| Data | Source | Licence / terms |
| --- | --- | --- |
| Matches and players | [TennisMyLife (TML) database](https://stats.tennismylife.org/tennis-match-database) | Non-commercial use only; commercial use needs TML's prior written permission. TML confirmed by email (2026-10-27) that aggregate statistics, charts and tables may be published on X and GitHub if the project stays non-commercial, the original CSV files and individual match records are not redistributed, and TML is credited. |
| Historical weather (planned) | [Open-Meteo](https://open-meteo.com) | Free API for non-commercial use only; attribution required |
| Altitude, balls, indoor or outdoor, notes | Collected by hand, each item with its source and date accessed | Own work |

Everything calculated from the match data (Elo, serve advantage, profiles) is treated as non-commercial. Only aggregate statistics are published. The raw files, the database and individual match records are not.

Attributions:

- Data source: TennisMyLife — https://stats.tennismylife.org. TennisMyLife describes its database as based on the work of Jeff Sackmann / Tennis Abstract (CC BY-NC-SA 4.0).
- Weather data by [Open-Meteo.com](https://open-meteo.com).

This project is not affiliated with the ATP, the ITF or any tournament.

## Structure

```
tennis-conditions/
├── data/
│   ├── raw/          # downloaded files, untouched (not published)
│   ├── own/          # my own tables: tournaments, venues, balls, notes (with source and date)
│   └── db/           # SQLite database (not published)
├── notebooks/        # exploration, cleaning, SQL, Elo, form and charts
├── charts/           # aggregate charts, added after review
├── src/              # scripts (planned)
└── README.md
```

## Data model (summary)

Built so far: `matches`, `tournaments`, `players`, `elo_long`, and two derived aggregate tables, `serve_advantage_by_year` and `serve_advantage_pooled`.
Planned: `venues`, `editions`, `weather`, `rankings`, `ball_notes`.

Every table includes `source` and `licence` columns, and tables of my own are kept separate from third-party data.

## Roadmap

1. Setup: repository, environment and data download. *(done)*
2. Matches and expected performance (Elo) in SQLite. *(done)*
3. Venues, weather and estimated speed for the 14 tournaments. *(started: serve advantage built; venues, altitude and weather next)*
4. Profiles by conditions and recent form.
5. Web dashboard with selectors to group and mix.
6. Extend to ATP 500 and 250.

## Code licence

To be decided (for example MIT). No raw match data is published in this repository.

## Author

[hugomateos1](https://github.com/hugomateos1)