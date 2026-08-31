# eccofs-model-repo

The ECCOFS regional ocean model, published as map data. Nothing is built;
`PLAN.md` is the founding plan and `README.md` says what the source is.

<!-- DOC-DOCTRINE v1 begin — identical in all six repositories; `check:docs` holds them equal. Edit one, sync all. -->
## Where truth lives, and what "update docs" means

Six repositories carry this project: `oceanlet.js` (the engine),
`oceansensing.github.io` (the site, and every fetch script),
`realtime-data-repo` (the orchestrator every data repository runs, and most
products), `espc-model-repo` (the ESPC currents), `sentinel3-data-repo`
(Sentinel-3 ocean color and HAB indicators) and `eccofs-model-repo` (the
ECCOFS regional model) — the last two added 2026-08-30. Each document
answers exactly one question.

**They are MEANT to carry the same four documents and three of them do not**
— `oceanlet.js`, `sentinel3-data-repo` and `eccofs-model-repo` have a
`DECISIONS.md`; the site, `realtime-data-repo` and `espc-model-repo` do not
(measured 2026-08-30). That is a gap in three repositories, not a license to
skip the file: a data repository closes one-way doors too, and it has
nowhere to say so.

| file | answers | tense | it is stale when |
| --- | --- | --- | --- |
| `README.md` | what this is, how to run it | present | a reader types a command or trusts a number and is wrong |
| `CLAUDE.md` | what must not be got wrong here | imperative | the next session is about to repeat a mistake |
| `PLAN.md` | what happened, measured, and what is open | dated past | "why is it like this?" has no answer here |
| `DECISIONS.md` | which one-way door closed, and when | dated | a reversal would cost a migration and nothing says so |
| `docs/` | contracts, ledgers and the guide | present | it describes an interface, a divergence or a concept that has moved on |

**`docs/` is a first-class part of "all docs", not an appendix** — the owner
asked for that explicitly on 2026-08-28, and the reason is that these are the
documents everything else points AT. A frozen contract, a divergence ledger
whose rows are pinned by tests, a guide that introduces the model: each is
the thing a reader is sent to when the short answer will not do, so each is
the worst place for a claim that has quietly stopped being true.

**"Update docs" means a sweep of all six repositories, not the one in hand.**
Docs are part of the change, never a follow-up and never a separate ask. Six
questions, asked of every repository the change touched:

1. Did a command, a path, a script name or a number a reader would type or
   trust move? → `README.md`
2. Did a rule, a trap, or a things-that-must-move-together change or come to
   light? → `CLAUDE.md`
3. Did something *happen* — a measurement, a defect, a yield, a mechanism, an
   open question opened or answered? → `PLAN.md`
4. Did a one-way door close? → `DECISIONS.md`
5. Did an interface, a deliberate divergence, or a concept the guide explains
   move? → the matching file under `docs/`
6. **Does a document in another repository now say something false because of
   this change?** → fix it there, in the same sitting.

**Question 6 is the one that gets missed, and it is why this block is
identical in six places.** Measured 2026-08-28: one tile-tier measurement
falsified `espc-model-repo`'s README, its `products.toml` header and the
site's README at once. Two were found; the third took a reminder from the
owner, who then asked for this doctrine.

**A SEVENTH repository consumes this system and is deliberately NOT in the
six above**: `ocean-now`, the iOS port, which mirrors the site's published
contract. It is not swept by these six questions and does not carry this
block — it has a lighter mechanism instead, a pending list in its parity
ledger, and the two repositories whose changes can reach it (the engine and
the site) each say so in their own section. It is named here because "four"
was read as "all of them" for two weeks while that ledger drifted 176 commits
behind with nothing noticing — which is question 6 failing at the granularity
of a whole repository rather than a document. Adding a repository to the list
above is therefore a real act: it buys the sweep, and leaving one off costs
exactly what that cost.

A number in prose is only as good as its anchor. `check:docs` gates every
claim it can tie to a source constant and nothing else, so when a figure has
no anchor — a measurement, a live reading, a byte count off a build log —
write **where it was measured and when**, or the next reader cannot tell a
fact from a guess that aged.
<!-- DOC-DOCTRINE v1 end -->
### In this repository

- **`PLAN.md`** — the founding plan and running record. **The measured study
  it rests on is in `oceansensing.github.io/PLAN.md`** under "Queued: ECCOFS"
  (2026-08-05) and is not copied here.
- **`DECISIONS.md`** — dated one-way decisions, D1 onward.
- **`pipeline/products.toml`** — not written yet.

## What must not be got wrong here

### The grid is the whole problem, and every failure in it is silent

ECCOFS is ROMS output on a **curvilinear, terrain-following, staggered**
grid. Every reader in this project assumes a regular lat/lon lattice. Three
transformations stand between the bucket and anything drawable, and **none of
them fails loudly**:

1. **Regrid curvilinear to regular lat/lon.** `lat_rho`/`lon_rho` are 2-D
   arrays of 1443 × 1667 — there is no axis to index.
2. **Interpolate s-levels to fixed depths.** "60 m" is not a level. It needs
   `h`, `zeta`, `Cs_r`, `hc`, `theta_s`, `theta_b` and the correct
   `Vtransform`/`Vstretching` case — **two transforms exist and the wrong one
   returns a plausible number.**
3. **Rotate the velocities.** `u` is 1443×1666, `v` is 1442×1667 — an
   Arakawa-C stagger. Average both to rho points, then rotate by `angle`.
   **Get this wrong and the currents look entirely plausible and flow the
   wrong way.**

**Every one of these needs a positive control before anything it produces is
believed.** The Gulf Stream is inside the domain, its direction is not a
matter of opinion, and it is the control for step 3. Do not eyeball a field
and call it right: the site's PLAN keeps a catalog of four ways particle
rendering has already gone wrong here, and this is the same failure class
arriving one stage earlier.

### The source needs no credentials, and that is worth not breaking

`s3://noaa-nos-eccofs-pds` is public via NOAA NODD — no credentials, no
egress charge. Any design that introduces an authenticated path has given
something up; say why in `DECISIONS.md` if it ever happens.

### Two rules inherited from the sibling data repositories

- **Every `roots` entry in `products.toml` must be one the site's
  `test-schema.mjs --roots` publishes**, or the orchestrator exits 2 and
  stops the publish.
- **A product that leaves takes its files with it.** The stage is seeded from
  what is already published.

### This repository's temperature field has another customer

`espc-model-repo` is planning an upper-ocean heat content layer for hurricane
intensity, and ECCOFS is a candidate upstream because it already carries
`temp` on 50 levels. **The competing consideration is coverage** — ECCOFS
stops well short of the hurricane main development region off West Africa,
while ESPC is global. That fork is recorded in `espc-model-repo`'s PLAN and is
the owner's to settle. Do not design the vertical handling as though this
repository were its only reader.

## The working agreement

The same one the sibling repositories keep: a measured constant moves with
its reason in the same commit; new checks are mutation-tested before they are
believed; exit codes are captured before output is read; docs are part of the
change, never a follow-up. **A number in prose without an anchor says where
it was measured and when, or it is a guess that will age.**
