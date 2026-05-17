# exp_18_tidal_ionization.md

**Status (Paper~III v0.1):** STUB -- script in place (inherited from
Paper~I), runtime evidence pending the first run from this repo's
environment.

**Audit-table rows this experiment evidences** (see
`paper/sections/audit_table.tex`):

1. *Numerical confirmation: $T_\text{escape}(g)$ decreases
   monotonically with $g$, sharp drop near $g_\text{crit}$.*
2. *Tidal displacement diagnostic: electron and proton move in
   opposite directions under gradient.*

Both flip from `STUB` to `PASS` once the run produces the
predicted signal.

## Headline claim

A linear clock-density gradient $g$ applied from $t = 0$ shortens
the orbital escape time $T_\text{escape}$ of the $n=1$ hydrogen
Arnold-tongue lock-in.  As $g$ approaches the predicted critical
gradient $g_\text{crit} = \Delta\omega_\text{tongue} / R_1 \approx
0.016$ per node, $T_\text{escape}$ drops sharply -- the resonance
basin closes before the lock-in can form.

## Parameters

| Parameter | Value | Source |
|---|---|---|
| Grid | $65^3$ | Same as `exp_12` / `exp_16` (Paper~I) |
| `TICKS_TOTAL` | 2500 | Covers the known ~2000-tick orbital lifetime |
| `CHECK_EVERY` | 50 | Diagnostic window length |
| `ESCAPE_RADIUS` | $2.0 \cdot R_1$ | Half the grid radius; well above bound-orbit $r_\text{peak}$ |
| `N_ESCAPE_CONFIRM` | 3 | Consecutive windows above escape radius to confirm escape |
| `OMEGA_E` | 0.1019 | Electron `instruction_frequency` (Paper~I) |
| `OMEGA_P` | $\pi / 2$ | Proton `instruction_frequency` (Paper~I) |
| `STRENGTH` | 30.0 | Coulomb strength (`exp_12` calibration) |
| `R1` | 10.3 | $n=1$ Bohr radius in lattice units (Paper~I) |
| `K_BOHR` | $1/R_1 \approx 0.0971$ | Initial momentum eigenvalue |
| `G_VALUES` | $\{0, 0.004, 0.008, 0.012, 0.016, 0.040, 0.080\}$ | Sweep through $g_\text{crit} \approx 0.016$ |

## Predicted signal

The g-scan should produce four characteristic features:

1. **Baseline at $g = 0$.**  $T_\text{escape}(0)$ either does not
   occur within `TICKS_TOTAL` (orbit stays bound) or escapes near
   the known ~2000-tick lifetime.  Either is consistent; if the
   baseline never escapes, the analysis below uses
   "no-escape-within-`TICKS_TOTAL`" as the reference.

2. **Monotonic $T_\text{escape}$ decrease in $g$.**  For
   $g \in \{0.004, 0.008, 0.012\}$, $T_\text{escape}(g)$ should
   decrease smoothly with $g$ (each step shortens the lifetime).

3. **Sharp drop at $g_\text{crit} \approx 0.016$.**  For
   $g \ge 0.016$, $T_\text{escape}$ should drop sharply -- the
   tongue never forms, and the orbit escapes within a few hundred
   ticks.

4. **Opposite-sign tidal displacement.**  For every $g > 0$ that
   produces escape, the diagnostic columns `x_e_proj` and
   `x_p_proj` (CoM projections relative to system CoM) should
   satisfy $x_e\_proj \cdot x_p\_proj < 0$.  This distinguishes
   gradient ionization (electron and proton drawn apart by the
   gradient) from kinetic-energy injection (isotropic scatter,
   same-sign projections by chance).

## PASS / PART / FAIL criteria

| Outcome | Audit-row implication |
|---|---|
| Monotone $T_\text{escape}$ decrease AND opposite-sign tidal signature for $g > 0$ AND sharp drop near $g_\text{crit}$ | Both audit rows flip to `PASS`. |
| Monotone decrease + opposite-sign signature, but no sharp drop at the predicted $g_\text{crit}$ | Both rows flip to `PART`; the gap is the quantitative $g_\text{crit}$ calibration (the free-particle tongue width is an upper bound on the $n=1$ tongue width). |
| No monotone decrease, or same-sign tidal signature, or $T_\text{escape}(g)$ uncorrelated with $g$ | Both rows flip to `FAIL`.  Investigate: either the gradient mechanism is wrong, or the implementation has a bug. |

## Runtime estimate

Each tick is a 3D wavefunction step on a $65^3$ grid (~274K
complex entries per `psi_R`/`psi_L`); two sessions (electron and
proton) tick alternately.  Per-tick wall-clock on a modern laptop:
$\sim 1$--3 seconds.  Per-$g$-value runtime: up to 2500 ticks
= $\sim 1$--2 hours.  Seven $g$ values total: $\sim 7$--14 hours
wall-clock if run sequentially, or $\sim 1$--2 hours if the seven
runs are parallelised (one per core).

## Outputs

- `data/exp_18_tidal_ionization.npy` -- one row per $g$ value with
  columns: $(g, \text{escaped}, T_\text{escape}, x_e\_proj,
  x_p\_proj, r_\text{pdf,first\_window})$.
- `data/exp_18_rpdf_trajectories.npy` -- full $r_\text{pdf}(t)$
  trajectory per $g$ value, shape `(len(G_VALUES), max_windows)`.

Both are tracked in git after the first run; they are the
load-bearing data for the corresponding paper figures.

## Reproducibility

From the repo root, with venv activated and engine provisioned
(see `CLAUDE.md`):

```text
python -u src/experiments/exp_18_tidal_ionization.py \
    | tee data/exp_18_tidal_ionization.log
```

Or via the makefile target:

```text
make -C src/experiments exp_18_tidal_ionization
```

## Provenance

Both the design (g-scan with linear gradient $V_\text{tidal} = g
\cdot (x - x_\text{centre})$, the $T_\text{escape}$ diagnostic,
the opposite-sign tidal-displacement smoking gun) and the
implementation (`exp_18_tidal_ionization.py`) were introduced in
Paper~I.  Paper~III's contribution is the *analytical*
$M_\text{min}(d)$ derivation (see `notes/quantum_roche_limit.md`)
and the *observational* application to compact-object atmospheres,
plus running the experiment from this repo's environment and
publishing the data alongside the analytical predictions.
