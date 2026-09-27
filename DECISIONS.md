# Decisions

Dated, irreversible-leaning decisions, one entry each, newest last. The
reasoning lives in `PLAN.md`.

**What counts as one-way in a data repository**: a decision that puts bytes in
readers' hands under a shape they will code against; a decision about which
repository owns a product, since moving one costs a migration in two places;
and a decision that forecloses an upstream.

## D1 — 2026-08-30 — Its own repository, under the currents/fields convention

The ECCOFS **scalar** fields — the cheap half of the model. Created as half of a pair with `eccofs-model-currents-repo`, under the
convention decided the same day: **every model splits two ways along the axis
that costs bytes.** See `espc-model-repo`'s `DECISIONS.md` D2 for the
measurement and the reasoning.

One-way in the ordinary data-repository sense: moving a product between
repositories is cheap in machinery and expensive in everything that points at
it — roots in the contract, origins in the site's config, and the union
`check:docs` holds across origins.

## Open, and not decided here

Everything else: which variables become products, at what depths, on what
grid, and whether forecast leads are carried.

## D2 — 2026-09-27 — The quick-save files, a 0.04 degree regional grid, published not drawn

**Source: the `qck` files of `noaa-nos-eccofs-pds`**, not the averages the
2026-08-05 study planned on. They are NetCDF classic (Range reads, no
library) and carry the model's own depth slices and east/north currents, so
two of the three silent transformations are the model's, not ours. The cost
is what `qck` carries: surface, 2, 50 and 100 m only. A depth outside those
means the 3-D `his` files and the s-level interpolation this repository's
CLAUDE warned about.

**Shape: one regional grid per root at 0.04 degree, `regional: true`**, no
tiles. One-way in the ordinary sense: readers will code against the roots,
the spacing and the flag.

**Published, not drawn** (the owner, 2026-09-27). Drawing a layer later is a
change to the map, which should also decide whether it wants tiles.
