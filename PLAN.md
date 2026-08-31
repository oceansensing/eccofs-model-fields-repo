# eccofs-model-repo — the founding plan and running record

The East Coast Community Ocean Forecast System, published as map data.
Started 2026-08-30, when the repository was created. **Nothing is built.**

## Where the research already is, and why it is not copied here

**The study that justifies this repository was done on 2026-08-05 and lives
in `oceansensing.github.io/PLAN.md`**, under "Queued: ECCOFS". It is measured
rather than sketched — the bucket, the grid dimensions, the three
transformations and their silent failure modes — and copying it here would
create a second copy to keep true. Read it there. This file carries what has
happened *since*, and what this repository decides.

The same convention `espc-model-repo` uses: records made before a repository
had a record of its own stay where they were written.

## What it is, in one table

Taken from that study; every figure was probed, not assumed.

| | |
| --- | --- |
| source | `s3://noaa-nos-eccofs-pds` — us-east-1, public, **no credentials**, via NOAA NODD |
| who | Rutgers, UC Santa Cruz, Fathom Science and NOS |
| model | ROMS 4D-Var — a new 5-day forecast daily off an analysis assimilating three days of observations |
| resolution | **3 km horizontal, 50 vertical levels** |
| extent | Grand Banks to the Orinoco |
| variables | `temp`, `salt`, `u`, `v`, `ubar`, `vbar`, `zeta` — the ESPC set minus the ice |

## The thing that makes this the largest data task queued

**The output is on the model's own curvilinear, terrain-following, staggered
grid, and every reader in this project assumes a regular lat/lon lattice.**
Three transformations stand between the bucket and anything the map can draw,
and the study names a silent failure mode for each:

1. **Regrid curvilinear to regular lat/lon.** `lat_rho`/`lon_rho` are 2-D
   arrays of **1443 × 1667** — 2.4 million points per level. There is no axis
   to index; the resample has to be built.
2. **Interpolate s-levels to fixed depths.** "60 m" is not a level here.
   Getting it needs `h`, `zeta`, `Cs_r`, `hc`, `theta_s`, `theta_b` and the
   right `Vtransform`/`Vstretching` case — all present in the file, and all
   easy to apply to the wrong one of two transforms and get a **plausible**
   answer.
3. **Rotate the velocities.** `u` is on 1443×1666 and `v` on 1442×1667 — an
   Arakawa-C stagger — so both must be averaged to rho points and rotated by
   `angle` to get true east/north. **Get this wrong and the currents look
   entirely plausible and flow the wrong way.**

**Measure against a known feature rather than eyeballing the field.** The
Gulf Stream is inside the domain and its direction is not a matter of
opinion. The site's PLAN keeps a catalog of the four ways particle rendering
has already gone wrong here; this is the same failure class arriving earlier
in the chain.

## What is decided, and what is not

`DECISIONS.md` carries D1 — its own repository — and nothing else. Which
variables to publish, at which depths, on which grid, and whether the
forecast leads are carried are all open, and none of them should be settled
before the regrid is proven against a known feature.

**One naming note.** The 2026-08-05 study proposed `oceansensing/eccofs-data-repo`;
the repository the owner created is **`eccofs-model-repo`**, which matches
`espc-model-repo` and is the better name — both are model output, as against
`realtime-data-repo`'s observations.

## A question this repository is party to but does not own

`espc-model-repo` is planning an **upper-ocean heat content** layer for
hurricane intensity, and ECCOFS is a candidate upstream for it because it
already carries `temp` on 50 levels. The competing consideration is coverage:
**ECCOFS stops well short of the hurricane main development region off West
Africa**, while ESPC is global. That fork is recorded in `espc-model-repo`'s
PLAN and is the owner's to settle; it is noted here so this repository is not
built as though nothing else wanted its temperature field.

## Open

1. Everything. Nothing is built and no product is defined.
2. The three transformations above, each of which needs a positive control
   before anything it produces is believed.
3. Storage, entirely unmeasured, against the 1 GB Pages cap — at 3 km over
   that domain with 50 levels, this is the first repository where the grid
   itself may be the constraint rather than the tile tier.
