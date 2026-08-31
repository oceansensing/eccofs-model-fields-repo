# Decisions

Dated, irreversible-leaning decisions, one entry each, newest last. The
reasoning lives in `PLAN.md`; this file is the index of what was decided and
when.

**What counts as one-way in a data repository**: a decision that puts bytes
in readers' hands under a shape they will code against; a decision about which
repository owns a product, since moving one costs a migration in two places;
and a decision that forecloses an upstream. Tuning a threshold is none of
those.

## D1 — 2026-08-30 — Its own repository, and named for the model

ECCOFS gets `eccofs-model-repo` rather than becoming products inside an
existing repository.

**One upstream, one fault domain, one gigabyte** — the argument
`espc-model-repo` made and measured. ECCOFS is a different model from a
different provider on a different grid, and its failures have nothing to do
with HYCOM's or with NOAA CoastWatch's.

**Named `-model-` rather than `-data-`.** The 2026-08-05 study proposed
`eccofs-data-repo`; the repository created is `eccofs-model-repo`, matching
`espc-model-repo`. That is the better cut: both hold **model output**, as
against `realtime-data-repo`'s observations. Recorded because the study still
says the old name and a reader will meet both.

## Open, and not decided here

Everything else. Which variables become products, at which depths, on which
output grid, whether forecast leads are carried, and whether this repository
supplies the temperature field for `espc-model-repo`'s planned heat-content
layer. **None of it should be settled before the regrid is proven against a
known feature** — see `CLAUDE.md`.
