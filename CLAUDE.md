# eccofs-model-fields-repo

The ECCOFS **scalar** fields — the cheap half of the model. Nothing is built; `README.md` says what the source is and
`PLAN.md` is the founding plan.

<!-- DOC-DOCTRINE v1 begin — identical in all eight repositories; `check:docs` holds them equal. Edit one, sync all. -->
## Where truth lives, and what "update docs" means

Eight repositories carry this project. The engine and the site:
`oceanlet.js`, `oceansensing.github.io` (the site, and every fetch script).
The orchestrator and the observations: `realtime-data-repo`. And the data
repositories, which since 2026-08-30 split **currents from fields** per model:
`espc-model-repo` (the ESPC currents — a legacy name, see below),
`espc-model-fields-repo`, `eccofs-model-currents-repo`,
`eccofs-model-fields-repo`, and `sentinel3-data-repo` (ocean color, which has
no vector half to split). Each document answers exactly one question.

**`espc-model-repo` is the ESPC CURRENTS repository** despite its name — the
one exception to the convention, kept because its URL is a live origin and
GitHub Pages does not reliably redirect a renamed project site. Read it and
`eccofs-model-currents-repo` as the same kind of thing.

*(`eccofs-model-repo` was RENAMED to `eccofs-model-fields-repo` on 2026-08-30,
not superseded — GitHub redirects the old name, which is why a rename was
free there and is not free for `espc-model-repo`: that one has published
bytes behind a Pages URL, and Pages does not redirect what the API does.)*

**All eight carry the same four documents, and since 2026-08-31 a gate holds
them to it** — `check:docs` requires a `DECISIONS.md` tracked in git in every
repository. The last two landed that day, the site's and
`realtime-data-repo`'s, reconstructed from records that already existed:
nothing was missing but the file, which is how the site went seven weeks
without one and `realtime-data-repo` eighteen days. **This block asserted
otherwise from the day it was written** — byte-compared in eight places and
false in two of them, because a gate on a text is a gate on the text. What it
cost is measurable: the engine promotion's own rehearsal listed *"a dated
entry in this repo's decisions and oceanlet's"* as its ninth step, and the
half with nowhere to go was simply not written.

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

**"Update docs" means a sweep of all eight repositories, not the one in hand.**
Docs are part of the change, never a follow-up and never a separate ask. Six
questions, asked of every repository the change touched:

1. Did a command, a path, a script name or a number a reader would type or
   trust move? → `README.md`
2. Did a rule, a trap, or a things-that-must-move-together change or come to
   light? → `CLAUDE.md`
3. Did something *happen* — a measurement, a defect, a yield, a mechanism, an
   open question opened or answered? → `PLAN.md`
4. Did a one-way door close — **or has one already recorded stopped being
   fully true**? → `DECISIONS.md`, in **every** repository the change
   touched. All eight carry one since 2026-08-31, so this is no longer the
   engine's question with seven exemptions; the amendment half is here
   because two entries needed one within a day of being written.
5. Did an interface, a deliberate divergence, or a concept the guide explains
   move? → the matching file under `docs/`
6. **Does a document in another repository now say something false because of
   this change?** → fix it there, in the same sitting.

**Question 6 is the one that gets missed, and it is why this block is
identical in eight places.** Measured 2026-08-28: one tile-tier measurement
falsified `espc-model-repo`'s README, its `products.toml` header and the
site's README at once. Two were found; the third took a reminder from the
owner, who then asked for this doctrine.

**Two repositories are deliberately NOT in the list above, on opposite
grounds, and both are named because an exclusion nobody wrote down is
indistinguishable from an oversight.**

`ocean-now`, the iOS port, **consumes this system** — it mirrors the site's
published contract. It is not swept by these six questions and does not carry
this block; it has a lighter mechanism instead, a pending list in its parity
ledger, and the two repositories whose changes can reach it (the engine and
the site) each say so in their own section. It is named here because "four"
was read as "all of them" for two weeks while that ledger drifted 176 commits
behind with nothing noticing — question 6 failing at the granularity of a
whole repository rather than a document.

`hab-data-repo` is excluded on the opposite ground: **it does not touch the
ocean map at all** (the owner's call, 2026-08-31). It publishes the bloom
photographs for a different part of the website, reached through `HAB_DATA`
in `src/config.ts`, and carries no interface anything here codes against
beyond a URL and a filename convention. It needs no mechanism, not even a
lighter one — nothing in these eight can falsify a claim in it, and it cannot
falsify one here. Do not mix it in.

Adding a repository to the list above is therefore a real act: it buys the
sweep, and leaving one off **silently** costs exactly what `ocean-now` cost.

A number in prose is only as good as its anchor. `check:docs` gates every
claim it can tie to a source constant and nothing else, so when a figure has
no anchor — a measurement, a live reading, a byte count off a build log —
write **where it was measured and when**, or the next reader cannot tell a
fact from a guess that aged.
<!-- DOC-DOCTRINE v1 end -->

### In this repository

- **`PLAN.md`** — the founding plan and running record.
- **`DECISIONS.md`** — dated one-way decisions, D1 onward.
- **`pipeline/products.toml`** — not written yet.

## What must not be got wrong here

### The grid is the whole problem, and every failure in it is silent

ECCOFS is ROMS output on a **curvilinear, terrain-following, staggered** grid.
Every reader in this project assumes a regular lat/lon lattice. Three
transformations stand between the bucket and anything drawable, and **none
fails loudly**:

1. **Regrid curvilinear to regular lat/lon.** `lat_rho`/`lon_rho` are 2-D
   arrays of 1443 x 1667 — there is no axis to index.
2. **Interpolate s-levels to fixed depths.** "60 m" is not a level. It needs
   `h`, `zeta`, `Cs_r`, `hc`, `theta_s`, `theta_b` and the correct
   `Vtransform`/`Vstretching` case — **two transforms exist and the wrong one
   returns a plausible number.**
3. **Rotate the velocities.** `u` is 1443x1666, `v` is 1442x1667 — an
   Arakawa-C stagger. Average both to rho points, then rotate by `angle`.
   **Get this wrong and the currents look entirely plausible and flow the
   wrong way.**

**Each needs a positive control before anything it produces is believed.** The
Gulf Stream is inside the domain and its direction is not a matter of opinion.

The source is `s3://noaa-nos-eccofs-pds` — public via NOAA NODD, **no
credentials, no egress charge**. Any design introducing an authenticated path
has given something up; say why in `DECISIONS.md` if it happens.

### Three rules inherited from the sibling data repositories

- **Every `roots` entry in `products.toml` must be one the site's
  `test-schema.mjs --roots` publishes**, or the orchestrator exits 2 and stops
  the publish.
- **A product that leaves takes its files with it.** Measured 2026-08-31:
  undeclaring a whole product self-heals in two runs, because the `published`
  branch is assembled from the declared products and the Pages tree from the
  stage. Renaming a file inside a product that still exists does not heal at
  all — its `writes` glob still matches. That is the case that has cost bytes.
- **A step's scope must match its products', and it is one change, never
  two.** Learned 2026-08-31 by a failed production run, and **this pair is the
  most likely place to meet it again**: a currents repository and a fields
  repository sharing one ECCOFS fetch script is exactly the arrangement where
  a `[steps.*]` `cmd` invoked bare fetches the sibling's families too. Scope it
  with `--only=` naming what this repository's products declare. Too WIDE and
  the write fence refuses the whole run; too NARROW and files somebody
  declared are never written and the previous copies carry forward frozen,
  silently. A *product* is the unit of ownership; a *step* is the unit of
  execution. The site's `check:docs` holds both directions across origins.

### The ESPC hour rule is cross-origin and permanent

The contract requires one model run to publish one hour across every ESPC
member, and those members span more than one repository — so no repository can
enforce it alone. Only the site, reading all origins, can. The arrangement
that would fix it (one repository for all of them) is exactly what storage
forbids.

### A coarser cadence is three changes, never one

**Learned 2026-08-31 in `espc-model-fields-repo`**, whose heat-content layer
went from the shared 3-hourly hour to 6-hourly to halve a tile tier's
bandwidth. It is the closest thing this repository will do to a composite's
cadence, so the shape is worth having before it is needed here.

Publishing less often than your siblings touches three things, and only one of
them is the cadence:

1. **The cadence itself** — and express it against the CLOCK, not against a
   run count. "Every other run" drifts, because GitHub delivers scheduled runs
   45 min to 4 h 19 apart; "only when the hour is a multiple of N" cannot.
2. **`max_age_hours`**, or the currency gate marks the product `behind` on
   every run and fails the workflow after every deploy. A cadence and its
   staleness budget are one decision.
3. **Any cross-product rule in the site's contract that assumes everything
   moves together.** ESPC's hour rule treats a same-run hour mismatch as a
   **quarantine** — it withdraws the layer and ships the previous copy, with
   every gate green. The fix was to teach the rule from a published header
   field rather than exempt the product, so the guarantee got weaker in a
   stated, checkable way instead of silently.

**A composite has this problem by construction**: a 7-day window is a cadence
of days against neighbours on hours, and whatever this repository publishes
beside it will need the same three answers.

### Finder's `.DS_Store` is ignored here, and was tracked until 2026-08-31

This repository had **no `.gitignore` at all** until then, so macOS's
`.DS_Store` was an ordinary versioned file — six of them across the five data
repositories, all removed from the index and ignored that day. The copy on
disk is left alone; Finder owns it and rewrites it on the next visit.

**Every one of the six arrived on a documentation commit**, and none was
deliberate. This repository's rode `e34d0b3` (2026-08-31), one of three that
took theirs under the same message — *A coarser cadence is three changes,
never one*. A cross-repository doc sweep is `git add -A` run in five
repositories in one afternoon, so a file none of them ignored entered four of
them on a single day — **the doctrine's own sweep was the vector.** The three
code repositories were never exposed to it, having ignored `.DS_Store` since
their first commit or the day after.

What it cost is that **`git status --porcelain` stopped being an answer.**
Finder rewrites a tracked `.DS_Store` whenever the directory is opened, so the
tree read dirty from a window rather than from an edit — and "is this tree
clean before I push" is only worth asking when a dirty tree means something.

It is also the file class behind the engine repository's 2026-08-30 fault, one
step earlier. There a `git rm -r` left a `.DS_Store` behind, so the emptied
directory still existed on disk and `existsSync` path claims went on resolving
locally while a fresh clone had nothing — green locally, red on CI, which is
why `check:docs` asks git rather than the filesystem. **A tracked `.DS_Store`
is the same disagreement between a clone and a working tree, in a file nobody
chose to version.**

**An ignore rule never untracks what is already in the index**, which is why
the fix here was `git rm --cached` and not a `.gitignore` line alone. A global
`core.excludesFile` covering the whole Finder family was written on this
machine at 13:13 on 2026-08-31, on the owner's instruction — *always by
default gitignore them and never track them* — and the last of the four
additions of that day landed at 13:02: eleven minutes too late to prevent
them, and structurally unable to reverse them.

**That global file is machine-local, so the `.gitignore` here is the half a
clone gets**, and it is not redundant with it. Blank it under `git -c
core.excludesFile=/dev/null` and the tree goes dirty with exactly the files
this section is about; restore it and the tree is clean. That is how the rule
was checked rather than assumed — with the global rule left on, blanking this
file changes nothing and the test sees nothing.

## The working agreement

The same one the sibling repositories keep: a measured constant moves with its
reason in the same commit; new checks are mutation-tested before they are
believed; exit codes are captured before output is read; docs are part of the
change, never a follow-up. **A number in prose without an anchor says where it
was measured and when, or it is a guess that will age.**
