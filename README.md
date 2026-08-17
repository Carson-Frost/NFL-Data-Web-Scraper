# NFL Data Pipeline

Four R scripts that pull NFL data from [nflfastR](https://www.nflfastr.com/) and write it to CSV,
one file per season. Between them they cover player season stats, player weekly stats, rosters, and
schedules, back to 1999.

## Requirements

R 4.0 or newer, with two packages installed:

```r
install.packages(c("nflfastR", "dplyr"))
```

## Running a script

Each script runs on its own and takes the 2025 regular season by default.

```bash
Rscript fetch_season_data.R      # player season statistics
Rscript fetch_weekly_data.R      # player weekly statistics
Rscript fetch_roster_data.R      # rosters
Rscript fetch_schedule_data.R    # schedules
```

A script creates its output folder if it is missing, writes one CSV per season into it, and then
prints the files it wrote with their sizes.

| Script | What it fetches | Where it writes |
|---|---|---|
| `fetch_season_data.R` | Player season statistics, from `calculate_stats`. | `output/season_stats/` |
| `fetch_weekly_data.R` | Player weekly statistics, one row per player per week. | `output/weekly_stats/` |
| `fetch_roster_data.R` | Rosters, from `fast_scraper_roster`. | `output/roster_data/` |
| `fetch_schedule_data.R` | Season schedules, from `fast_scraper_schedules`. | `output/schedule_data/` |

## Choosing seasons

`--seasons` takes a single year or an inclusive range. `--season-type` takes `REG`, `POST`, or
`REG+POST`, and applies to the season and weekly scripts; rosters and schedules are not split by
season type and ignore it.

```bash
Rscript fetch_season_data.R --seasons=2022:2025 --season-type=REG+POST
Rscript fetch_weekly_data.R --seasons=1999
Rscript fetch_roster_data.R --seasons=2020:2025
Rscript fetch_schedule_data.R --seasons=2010
```

The environment variables `SEASONS` and `SEASON_TYPE` set the same two values, and they take
precedence over the command line flags when both are given.
