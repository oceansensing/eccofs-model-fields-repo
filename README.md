# eccofs-model-repo

The East Coast Community Ocean Forecast System (ECCOFS), published as map
data. A sibling data repository: its own Pages site, its own cron, its own
gigabyte, holding no code of its own.

**Nothing is built.** The repository exists so the work has a home and the
doc doctrine has a place to hold it. `PLAN.md` says what ECCOFS is and what
makes it the largest data task queued; the measured study behind it lives in
`oceansensing.github.io/PLAN.md` under "Queued: ECCOFS" (2026-08-05) and is
deliberately not copied here.

## What it will publish

Undecided. The source carries `temp`, `salt`, `u`, `v`, `ubar`, `vbar` and
`zeta` — the ESPC set minus the ice — at **3 km on 50 vertical levels**, from
Grand Banks to the Orinoco, as a daily 5-day forecast off a 4D-Var analysis.
Which of those become products, at which depths, is open.

| | |
| --- | --- |
| source | `s3://noaa-nos-eccofs-pds` — us-east-1, public, **no credentials**, via NOAA NODD |
| model | ROMS 4D-Var (Rutgers, UC Santa Cruz, Fathom Science, NOS) |
| grid | **curvilinear, terrain-following, staggered** — not a lat/lon lattice |

That last row is the whole difficulty and is why nothing is built yet.

## How it will run

The arrangement every data repository here uses: the orchestrator comes from
`realtime-data-repo`, the fetchers and the published-file contract come from
`oceansensing.github.io`, both checked out at run time. This repository will
carry `pipeline/products.toml` and nothing else executable.

There are no commands to give yet, and inventing them is how a README starts
lying.

## Structure

```
PLAN.md         the founding plan and running record
CLAUDE.md       what must not be got wrong here, and the shared doc doctrine
DECISIONS.md    dated one-way decisions, D1 onward
pipeline/       products.toml — not written yet
```
