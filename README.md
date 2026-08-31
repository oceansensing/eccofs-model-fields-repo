# eccofs-model-fields-repo

The ECCOFS **scalar** fields — the cheap half of the model. A sibling data repository: its own Pages site, its own cron,
its own gigabyte, holding no code of its own.

**Nothing is built.** `PLAN.md` is the founding plan; `CLAUDE.md` carries what
must not be got wrong and the shared doc doctrine.

## What it will publish

`temp`, `salt` and `zeta` from ECCOFS, on fixed depths. **No product is
defined yet.** Its `temp` on 50 vertical levels is a candidate upstream for
the upper-ocean heat content layer `espc-model-repo`'s PLAN describes.

Nothing is built. The measured study behind ECCOFS lives in
`oceansensing.github.io/PLAN.md` under "Queued: ECCOFS" (2026-08-05) and is
deliberately not copied.

## Storage

Unmeasured.

## Why it is separate from `eccofs-model-currents-repo`

**Every model splits two ways along the axis that costs bytes** (decided
2026-08-30): a currents repository for the tiled vector fields, which are
expensive, and a fields repository for the scalars, which are cheap. ESPC's
tile tier is 89% of its repository's bytes — two forecast leads across five
depths — against 44-58 MB for a 2-D scalar field. Splitting gives each half
its own gigabyte.

## How it will run

The orchestrator comes from `realtime-data-repo`, the fetchers and the
published-file contract from `oceansensing.github.io`, both checked out at run
time. This repository will carry `pipeline/products.toml` and nothing else
executable. There are no commands to give yet.

## Structure

```
PLAN.md         the founding plan and running record
CLAUDE.md       what must not be got wrong, and the shared doc doctrine
DECISIONS.md    dated one-way decisions, D1 onward
pipeline/       products.toml — not written yet
```
