# Data sources

Raw files are not published in this repository (see the source terms below).
This file records where each file came from so the work can be reproduced.

## TennisMyLife (TML) match database

- Source: https://stats.tennismylife.org/tennis-match-database
- Also on GitHub: https://github.com/Tennismylife/TML-Database
- Terms: non-commercial use only; commercial use needs prior written permission from TML. The MIT label that appeared on the website was an error and TML corrected it (confirmed by email, 2026-10-27).
- Permission confirmed by email (2026-10-27): aggregate statistics, charts and tables may be published on X and GitHub if the project stays non-commercial, original CSV files and individual match records are not redistributed, and TML is credited.
- Credit line: Data source: TennisMyLife — https://stats.tennismylife.org
- Based on Jeff Sackmann's tennis_atp (CC BY-NC-SA 4.0); data collected from the
  ATP website and other sources, according to TML.

| File | Downloaded | Notes |
| --- | --- | --- |
| 2024.csv | 2026-10-22 | 653,446 bytes. ATP main tour. Includes Davis Cup (tourney_level D), to be filtered out. |
 | 2000.csv to 2026.csv | 2026-10-23 | ATP main tour yearly files, downloaded in one batch (2024.csv on 2026-10-22). 2026 is a partial season. |   | 2000.csv to 2026.csv (re-downloaded) | 2026-10-25 | Corrected versions after reporting issues to TML. TML replied by email (2026-10-25) that four issues were fixed: 2025 missing match numbers, 2026 US Open indoor value, 2026 Winston-Salem level, 2024 Doha and Los Cabos level. The email did not address the licence question. Previous versions kept locally in data/raw/before_tml_fix/ (not published). |
| 2000.csv to 2026.csv (refreshed) | 2026-10-28 | Re-downloaded before first publication. Previous versions kept locally in data/raw/downloaded_2026-10-25/ (not published). SHA256 hashes in data/own/raw_file_hashes_2026-10-28.txt. 2026 is a partial season; tournaments in progress are incomplete. |