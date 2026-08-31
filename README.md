# eccofs-model-repo — SUPERSEDED, 2026-08-30

**Do not build here.** ECCOFS is published by a pair of repositories instead:

| | |
| --- | --- |
| [`eccofs-model-currents-repo`](https://github.com/oceansensing/eccofs-model-currents-repo) | the vector fields — `u`, `v`, `ubar`, `vbar` |
| [`eccofs-model-fields-repo`](https://github.com/oceansensing/eccofs-model-fields-repo) | the scalar fields — `temp`, `salt`, `zeta` |

## Why this repository existed for one afternoon

It was created before the **currents/fields convention** was decided, and it
carried the founding plan for a few hours. The convention — every model splits
two ways along the axis that costs bytes, because a tiled vector tier is ~89%
of a model repository's bytes against 44–58 MB for a scalar field — then made
a single ECCOFS repository the wrong shape.

Its documents moved into the pair rather than being rewritten. The measured
study they rest on has always lived in `oceansensing.github.io/PLAN.md` under
"Queued: ECCOFS" (2026-08-05) and never moved.

## Why it was retired rather than renamed

`espc-model-repo` kept a name that does not match the convention, because its
URL is a **live origin** and GitHub Pages does not reliably redirect a renamed
project site. **This repository had no Pages site and no published bytes**, so
none of that applied and there was nothing to protect. Retiring it costs
nothing; keeping it would have meant a second exception with no reason behind
it.

It carries no `CLAUDE.md`, so it is outside the doc doctrine's sweep — which is
correct for a repository nobody should edit. It is left in place rather than
deleted so this note is findable; deleting or archiving it is the owner's call.
