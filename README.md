# eccofs-model-fields-repo

The ECCOFS **scalar** fields — the cheap half of the model. A sibling data repository: its own Pages site, its own cron,
its own gigabyte, holding no code of its own.

**Live since 2026-09-27, published but not drawn** on the website's map
(the owner's call). `PLAN.md` is the founding plan and record; `CLAUDE.md` carries
what must not be got wrong and the shared doc doctrine.

## What it publishes

Six roots, each one regional grid at 0.04 degree (1647 x 1422, `regional:
true`, `source: NOAA NOS ECCOFS`): five the 3-hourly frame at or before now,
from the quick-save files, and heat content, since 2026-09-28, the daily
00 UTC snapshot at or before now, from the history files:

| root | quantity | size |
| --- | --- | --- |
| `sst-eccofs.json` | sea surface temperature | 12.5 MB |
| `sss-eccofs.json` | sea surface salinity | 12.5 MB |
| `ssh-eccofs.json` | sea surface height (`zeta`) | 12.7 MB |
| `temp50-eccofs.json` | temperature at 50 m | 12.4 MB |
| `temp100-eccofs.json` | temperature at 100 m | 12.4 MB |
| `ohc-eccofs.json` | ocean heat content (tropical cyclone heat potential, kJ/cm2) | 12.5 MB |

Heat content is ESPC's and Mercator's quantity: the heat above 26 C from the
26 C isotherm up, absent (not zero) where the surface is 26 C or cooler. A
column still warmer than 26 C at the bed is kept, since shallow water holds
that much: 83,369 of them on the first reading, the shelves in September.

The fetcher is the site's `scripts/fetch-eccofs.py`, shared with the
sibling repository and scoped here with `--only=`.

## Published to R2 alone (since 2026-10-10)

Declared `r2_only` in `pipeline/products.toml`: the same run builds these,
they are left out of this repository's Pages site and its status, and the R2
job publishes them beside the rest (the site pipeline's D13, its note of
2026-10-09). Their roots stay on the `published` branch, as every product's do.

| root | quantity | grid |
| --- | --- | --- |
| `bottomt-eccofs.json` | the bed's temperature — level 0 of the daily snapshot | 0.04 degree, `regional: true` |
| `bottoms-eccofs.json` | the bed's salinity | 0.04 degree, `regional: true` |
| `bottomdepth-eccofs.json` | level 0's depth below the free surface, m | 0.04 degree, `regional: true` |
| `temp-eccofs-<depth>m.json` | the temperature at each of the first 48 of Mercator's depths, 0.494 to 4833.291 m — every one the model's water reaches — interpolated from its 50 terrain-following levels below the datum, as its own 50 and 100 m slices are; one root a depth named for it to the meter (`-0m` … `-4833m`); the day's mean, from the model's daily averages file (since 2026-10-10's night; the 00 UTC snapshot before) | 0.04 degree, `regional: true` |
| `sal-eccofs-<depth>m.json` | the salinity at the same depths | 0.04 degree, `regional: true` |
| `w-eccofs-<depth>m.json` | the vertical velocity at the same depths, m/day, from the model's w-surfaces; over steep slopes it reaches thousands of m/day where the flow follows the bed | 0.04 degree, `regional: true` |
| `qnet-eccofs.json` | the surface's net heat flux into the ocean, W/m2 (`shflux`), the day's mean | 0.04 degree, `regional: true` |
| `emp-eccofs.json` | evaporation less precipitation, mm/day (`evaporation` − `rain`), the day's mean | 0.04 degree, `regional: true` |

The day's means come from the averages file NOAA posts for each day of a
run (`avg/eccofs_avg_<run>_<NNNN>.nc`, NNNN the run's date plus NNNN − 1
days): HDF5, uncompressed and chunked, so each variable is read chunk by chunk
with HTTP ranges rather than downloaded whole. Each is checked before it is
written: the currents' depth mean against the file's own depth-averaged
current, and 50 m against the quick-save files' 50 m slice averaged over the
same day.


**Each grid states the span its values cover** (since 2026-10-10): `validFrom`
and `validTo` in its header, the interval its daily mean covers — the day round its noon stamp, as ROMS averages.
**Temperature, salinity and the daily mean sea level (`ssh-eccofs-daily.json`, the averages' own `zeta`) publish today and the day ahead**: each
such root is today's and lists the days it publishes in its header
(`frames`: each day's time, span, run where there is one, and file); the day
ahead is the root's name with `.v<its time>` before `.json`
(`temp-eccofs-47m.v20261012T1200Z.json`), with its own file. A file names one time for as long as it
exists, and a day no longer published leaves on the next run.

## Storage

About 75 MB a tree (measured 2026-09-28; 60 MB before heat content).

R2's alone beside it: about 3.0 GB a day, almost all the five sets at
Mercator's depths, 585 to 595 MB each (measured 2026-10-10).

## Why it is separate from `eccofs-model-currents-repo`

**Every model splits two ways along the axis that costs bytes** (decided
2026-08-30): a currents repository for the tiled vector fields, which are
expensive, and a fields repository for the scalars, which are cheap. ESPC's
tile tier is 89% of its repository's bytes — two forecast leads across five
depths — against 44-58 MB for a 2-D scalar field. Splitting gives each half
its own gigabyte.

## How it runs

The orchestrator (the site's private `pipeline/`, since 2026-09-26), the
fetchers and the published-file contract all come from
`oceansensing.github.io`, checked out at run time. This repository carries `pipeline/products.toml` and its publish workflow
(`.github/workflows/publish.yml`), and nothing else executable. **Scheduled since 2026-09-27** (`13 1-23/3 * * *`), after its first dispatched run published.

## Structure

```
PLAN.md         the founding plan and running record
CLAUDE.md       what must not be got wrong, and the shared doc doctrine
DECISIONS.md    dated one-way decisions, D1 onward
pipeline/       products.toml
.github/        the publish workflow
```
