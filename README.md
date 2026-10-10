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
| `temp-eccofs-<depth>m.json` | the temperature at each of the first 48 of Mercator's depths, 0.494 to 4833.291 m — every one the model's water reaches — interpolated from its 50 terrain-following levels below the datum, as its own 50 and 100 m slices are; one root a depth named for it to the meter (`-0m` … `-4833m`), from the daily 00 UTC snapshot | 0.04 degree, `regional: true` |
| `sal-eccofs-<depth>m.json` | the salinity at the same depths | 0.04 degree, `regional: true` |

## Storage

About 75 MB a tree (measured 2026-09-28; 60 MB before heat content).

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
