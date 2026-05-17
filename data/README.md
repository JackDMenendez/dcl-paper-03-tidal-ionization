# data/

Data files produced and consumed by experiments.

## What goes here

- **`exp_18_tidal_ionization.npy`** -- output of the g-scan
  experiment.  Rows are gradient values; columns are
  $(g, \text{escaped}, T_\text{escape}, x_e\_proj, x_p\_proj,
  r_\text{pdf,first\_window})$.  Tracked in git after the first run
  (small file).
- **`exp_18_rpdf_trajectories.npy`** -- full $r_\text{pdf}(t)$
  trajectory per $g$ value, shape `(len(G_VALUES), max_windows)`.
  Tracked in git.
- `*.log` -- stdout captures from long-running experiments.
  Tracked (the `.gitignore` has `!data/*.log`) so the exact stdout
  of each PASS/FAIL run is part of the project history.
- `*.npy` / `*.csv` -- generic numpy / tabular outputs from any
  future experiment.

## What does NOT go here

- Source code (lives in `src/`).
- Figures (live in `paper/figures/` or the repo-root `figures/`).
- Build artefacts (live in `build/`).

## Naming

`<exp_id>_<descriptor>.{npy,log}` -- e.g.
`exp_18_tidal_ionization.npy`,
`exp_18_rpdf_trajectories.npy`.  The `<exp_id>` prefix lets you
`ls data/exp_18*` and see everything that experiment touched.
