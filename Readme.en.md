tennis-conditions

A relational database of ATP tournament conditions (altitude, weather, ball, estimated court speed, player workload) joined with match results, to study how players perform under different conditions.

This is a descriptive analysis, not a betting or forecasting project. It starts with the Grand Slams and Masters 1000 events, active players only, and is built in Python and SQL as a learning project.

Versión en español

What it is

A database of the conditions at each ATP tournament (altitude, weather, ball, estimated court speed, match load and travel for players) combined with match results, to analyse:

what the conditions at each tournament are like and how they have changed year to year;
how each player performs above or below expectation depending on the conditions;
how each player arrives at a tournament (recent form on the surface).
Status

Phase 0 (setup). This README will be updated with each phase.

Scope
ATP men's singles, main draw only.
Excluded: Challenger, Futures / ITF, qualifying, WTA, doubles and Davis Cup.
Starting with the 4 Grand Slams and the 9 Masters 1000 events.
Active players only (at least one main-draw match in the last 52 weeks).
Matches from 2000 (needed to compute expected performance with Elo); conditions and player profiles from 2015.
How it is analysed
Tournament card: altitude, weather per edition, ball and estimated court speed.
Player profile by conditions: performance above expectation, grouped by traits (fast or slow court, altitude, hot or cold).
Recent form: average performance above expectation over the most recent matches on the surface, with more weight on the latest ones.

Results can be grouped by tournament, court conditions, season, surface or player.

Statistical rules: minimum number of matches per group, confidence intervals next to every figure, care with multiple comparisons, and the language of profiles rather than causes. "Not enough evidence" is a valid result.

Data and licences
Data	Source	Licence / terms
Matches, players and rankings	Jeff Sackmann / Tennis Abstract, tennis_atp	CC BY-NC-SA 4.0: attribution, non-commercial use only, share alike
Historical weather	Open-Meteo	Free API for non-commercial use only; attribution required
Altitude, balls, indoor or outdoor, notes	Collected by hand, each item with its source and date accessed	Own work

Everything calculated from Sackmann's data (Elo, estimated speed, profiles) inherits his non-commercial licence.

Attributions:

Match data: Jeff Sackmann / Tennis Abstract, CC BY-NC-SA 4.0.
Weather data by Open-Meteo.com.

This project is not affiliated with the ATP, the ITF or any tournament.

Planned structure
tennis-conditions/
├── data/
│   ├── raw/          # downloaded files, untouched
│   ├── own/          # my own tables: venues, balls, notes (with source and date)
│   └── db/           # SQLite database
├── notebooks/        # exploration and charts
├── src/              # scripts: loading, cleaning, Elo, weather
└── README.md
Data model (summary)

players, venues, tournaments, editions, matches, weather, rankings, elo, ball_notes. Every table includes source and licence columns.

Roadmap
Setup: repository, environment and data download.
Matches and expected performance (Elo) in SQLite.
Venues, weather and estimated speed for the 13 tournaments.
Profiles by conditions and recent form.
Web dashboard with selectors to group and mix.
Extend to ATP 500 and 250.
Code licence

To be decided (for example MIT). Data derived from Sackmann's files is published under CC BY-NC-SA 4.0.

Author

hugomateos1