# eccofs-model-fields-repo — the founding plan and running record

The ECCOFS **scalar** fields — the cheap half of the model. Started 2026-08-30, when the repository was created. **Built 2026-09-27, published but not drawn.**

## What it is for

`temp`, `salt` and `zeta` from ECCOFS, on fixed depths. **No product is
defined yet.** Its `temp` on 50 vertical levels is a candidate upstream for
the upper-ocean heat content layer `espc-model-repo`'s PLAN describes.

The measured study behind ECCOFS lives in
`oceansensing.github.io/PLAN.md` under "Queued: ECCOFS" (2026-08-05) and is
deliberately not copied.

## Why it is its own repository

**Every model splits two ways** (decided 2026-08-30, `espc-model-repo`'s
`DECISIONS.md` D2): `<model>-model-currents-repo` for the tiled vector fields
and `<model>-model-fields-repo` for the scalars. The axis is what costs bytes,
measured rather than chosen — ESPC's tile tier is **89% of its repository**,
two leads across five depths, against 44-58 MB for a 2-D scalar field.

**`espc-model-repo` is the one exception to the naming**, kept knowingly: it
is the ESPC currents repository, and its URL is a live origin that a rename
would 404. Read it as `espc-model-currents-repo`.

## Storage

Unmeasured.

## Open, as founded

*Answered 2026-09-27 — the products, `pipeline/products.toml` and a schedule
offset from the sibling's are the dated entry below; what is still open is
at its end.*

## 2026-09-27 — built from the quick-save files, and rehearsed

The owner asked for ECCOFS to publish without the map drawing it, from the
"cloud-first" bucket (the registry entry the owner named lists exactly one,
`noaa-nos-eccofs-pds`). Surveyed that day: `avg` 7.6 GB HDF5, `his` 4.7 GB,
`qck` 1.6-1.8 GB **NetCDF classic**, eight files a day of eight 3-hourly
records, posted about four days after the run's date and covering from about
three to eight days after it — so the newest posting holds now. The `qck`
files carry surface fields, slices at 2/50/100 m and currents already
east/north, which took the de-staggering and rotation off this repository's
list.

**Built**: the site's `scripts/fetch-eccofs.py` (standard library plus numpy;
Range reads) and `scripts/regrid.py` (bin averaging). Lattice chosen by
measurement: model spacing 0.024 x 0.021 degree; interior holes 36% at 0.025,
13% at 0.03, **0.013% at 0.04**. First live run: 42 s for the seven roots of
both repositories.

**The first live run refused two fields for being real.** Sea level at 7.6 m
is the Bay of Fundy's tide, and salinity at -1.4 is the model's own
undershoot at river inflows. Bounds were widened and each scalar gained a
median band, the check that catches a wrong variable or unit. Spot values
that run: Sargasso Sea 29.3 C and 36.7 psu, Gulf of Maine 16.8 C, Bay of
Fundy 5.25 m.

**It also found a gap in the contract**: the site's checker held every vector
root to the global rules, so a regional model's currents could not publish.
The contract gained `regional: true` (the site's `schema.ts`), and the
checker's own positive control — which fired for an origin whose every pair
is regional — was fixed before any push.

**Rehearsed through the orchestrator** in a throwaway copy of the site with
the roots in its contract: every file matched, every fate `fresh`, and the
status reported `schedule: null`, the dispatch-only state.

## Open

- **Went live 2026-09-27** — the entry below.
- **Re-measure `max_age_hours` (8)** after a week of scheduled runs.

## 2026-09-27 — live

The owner added the secrets; the site's commit `d978a1b` put this
repository's roots in the contract and its origin in `MAP_ORIGINS`; the
dispatched run 36296050779 went green on its first try — build, Pages and R2 — and
`status/status.json` read, at 2026-09-27T05:03:58Z: every product `fresh`
(5 of 5), the nearest frame 2.07 h from the
reader, `contract: 1`. Each root was fetched from Pages
and served. The schedule, `13 1-23/3 * * *`, was then uncommented (longest gap
3 h, so the watchdog's silence budget is 5.5 h); the
first scheduled run is the next reading.

## The workflow's packages come from the site — 2026-09-27

The publish workflow installs `site/scripts/requirements-eccofs.txt`, one file
per fetcher family, instead of naming packages in its own `pip install`
line. Dependabot reads requirements files and never a workflow line: an
inline pin elsewhere had carried `requests` 2.32.3, a version with two
advisories, unflagged. The site's `check:docs` now refuses an inline package
here. Confirmed by a dispatched run, green on build, Pages and R2.
