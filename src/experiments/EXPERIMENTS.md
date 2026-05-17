# Experiment Index

## Current experiments

**[`exp_18_tidal_ionization`](exp_18_tidal_ionization.md)** -- STUB
in v0.1.  Gradient $g$ shortens orbital escape time
$T_\text{escape}$; sharp drop expected near $g_\text{crit} \approx
0.016$/node; opposite-sign tidal-displacement signature.  Evidences
audit-table rows "Numerical confirmation..." and "Tidal
displacement diagnostic..." (Table~\ref{tab:audit}).

As more experiments are added, replace this prose with a markdown
table with columns: `ID`, `Status`, `What it claims`,
`Audit rows`, `Companion doc`.

Keep this file in sync with `paper/sections/audit_table.tex` --
the audit table is the public record; this file is the
implementer's index.

## Status legend

- `STUB` -- audit row added, experiment script not yet written or
  not yet producing a clean signal.
- `PART` -- experiment runs and demonstrates the mechanism but the
  quantitative match is incomplete; specific gap noted in the
  companion doc.
- `PASS` -- experiment confirms the audit row to stated precision.
- `FAIL` -- experiment disconfirms the audit row.  Keep the row;
  failure is evidence too.

Status here should equal status in `audit_table.tex`.  If they
disagree, the audit table is the authority and this file is wrong.
The `audit_universe.py` master roll-up uses `audit_table.tex` as
its authority and parses each experiment's most recent
`data/*.log` for the actual cached PASS/FAIL marker -- see
`../../audit_universe.md` for the full model.
