# Data sources

Raw files are not published in this repository (see the source terms below).
This file records where each file came from so the work can be reproduced.

## TennisMyLife (TML) match database

- Source: https://stats.tennismylife.org/tennis-match-database
- Also on GitHub: https://github.com/Tennismylife/TML-Database
- Terms stated by the source: educational, analytical and research use;
  no redistribution, commercial use or sale of the raw data without permission.
  The site also labels the data MIT, which conflicts with the README above;
  treated as non-commercial until clarified.
- Based on Jeff Sackmann's tennis_atp (CC BY-NC-SA 4.0); data collected from the
  ATP website and other sources, according to TML.

| File | Downloaded | Notes |
| --- | --- | --- |
| 2024.csv | 2026-10-22 | 653,446 bytes. ATP main tour. Includes Davis Cup (tourney_level D), to be filtered out. |
   | 2000.csv to 2026.csv | 2026-10-23 | ATP main tour yearly files, downloaded in one batch (2024.csv on 2026-10-22). 2026 is a partial season. |
   | 2000.csv to 2026.csv (re-downloaded) | 2026-10-25 | Corrected versions after reporting issues to TML. TML replied by email (2026-10-25) that four issues were fixed: 2025 missing match numbers, 2026 US Open indoor value, 2026 Winston-Salem level, 2024 Doha and Los Cabos level. The email did not address the licence question. Previous versions kept locally in data/raw/before_tml_fix/ (not published). |