<!-- markdownlint-disable MD022 MD025 MD033 MD060 -->
# CLAUDE.md -- Working Brief for Claude Code

> Project: Paper~III of the A=1 Discrete Causal Lattice series --
> Tidal Ionization of Atomic Hydrogen and the Quantum Roche Limit

This file is the project memory for Claude Code.  Keep it updated
so a new conversation can continue work without the full chat
history.

---

## CURRENT STATUS (2026-05-16) -- v0.1-DRAFT

Repository freshly provisioned on 2026-05-16 from
`dcl-paper-experiment-template`.  Takes up follow-on item~\#9
("Tidal Ionization Mass") of Paper~I's
`external/dcl/notes/follow_on_implications.md` catalogue.  The
substantive theoretical apparatus and a complete numerical
experiment script were both introduced in Paper~I and are
reproduced here with Provenance prefixes.  Paper~III's contribution
is the analytical $M_\text{min}(d)$ quantum Roche limit derivation,
the $d^3$ scaling identification, running the experiment from
this repo's environment, and the observational predictions for
21\,cm HI and accretion-disk Fe~K$\alpha$ inner edges near compact
objects.

What is in place (v0.1-DRAFT):

- **Inherited theoretical apparatus.**
  `notes/plasma_as_gravitational_ionization.md` (verbatim from
  Paper~I with Provenance prefix): the Arnold-tongue /
  clock-density-gradient argument, the three-channel ionization
  taxonomy, the $1/n^4$ Arnold-tongue compression result, and the
  cosmological-recombination connection.
- **Inherited numerical experiment.**
  `src/experiments/exp_18_tidal_ionization.py` (verbatim from
  Paper~I with Provenance prefix): the g-scan that applies a
  linear gradient $V_\text{tidal} = g \cdot (x - x_\text{centre})$
  from $t=0$ and measures $T_\text{escape}(g)$ together with the
  tidal-displacement diagnostic.  Companion
  `exp_18_tidal_ionization.md` expanded from Paper~I's stub.
- **New analytical note.**  `notes/quantum_roche_limit.md`
  (DRAFT): synthesises Paper~I's gradient-ionization argument with
  Paper~I follow-on \#9's $M/d^3$ scaling observation into the
  explicit $M_\text{min}(d) = \Delta\omega_\text{tongue} \cdot d^3
  / R_1$ derivation that the paper's main body builds on.
- **Paper skeleton.**  Title page, abstract, introduction, audit
  table, references all rewritten Paper~III-specific.  Conclusion,
  acknowledgements, code-and-data, reproducibility appendices
  scaffolded with project-specific placeholders pending v1.0.

**Next concrete actions:**

1. **Set up the venv** (creates `.venv`, installs requirements
   including `scipy` and the pip-installed `dcl_core` engine):

   ```text
   setup.cmd     REM Windows
   ./setup.sh    # POSIX
   ```

   The engine is provisioned automatically via the
   `dcl_core @ git+https://github.com/JackDMenendez/dcl-core@main`
   line in `virtual-env-requirements.txt`; no manual engine setup
   is required.  See the *Engine* section below for the resolved
   architecture.

2. **Run the experiment** (after step~1 lands the venv):

   ```text
   python -u src/experiments/exp_18_tidal_ionization.py \
       | tee data/exp_18_tidal_ionization.log
   ```

   Expected runtime: a few hours.  Expected signal: monotonic
   $T_\text{escape}(g)$ decrease, sharp drop near
   $g_\text{crit} \approx 0.016$ per node, opposite-sign
   tidal-displacement diagnostic for $g > 0$.

3. **Phase~2 paper drafting:**  fill in the planned sections
   (Arnold-tongue lock-in recap; gradient detuning and critical
   condition; quantum Roche limit and $d^3$ scaling; numerical
   confirmation; observational predictions).  See
   `notes/quantum_roche_limit.md` for the analytical thread.

4. **Flip audit-table rows** from STUB/PART to PASS as the script
   and the paper-text derivation land.

---

## What This Project Is

The hydrogen atom in the A=1 Discrete Causal Lattice framework
(Paper~I) is a joint Arnold-tongue attractor between the proton
and electron sessions.  An external mass $M$ at distance $d$
imposes a clock-density gradient across the atom of order
$\Delta\omega_\text{tidal} \sim M \cdot R_1 / d^3$.  When this
tidal detuning exceeds the Arnold-tongue width
$\Delta\omega_\text{tongue}$, the joint resonance breaks and the
orbit dissolves -- *gradient ionization*, a third channel beyond
thermal (Saha) and pressure (Pauli) ionization.

Paper~III makes this picture quantitative.  Its three legs:

1. **Analytical:** derive the threshold
   $M_\text{min}(d) = \Delta\omega_\text{tongue} \cdot d^3 / R_1$
   -- the *quantum Roche limit*.  Identify the $d^3$ scaling as the
   quantum-mechanical analogue of the classical Roche limit, with
   $\Delta\omega_\text{tongue}$ playing the role of the classical
   body density.
2. **Numerical:** confirm the threshold via `exp_18`, which sweeps
   the gradient $g$ and measures the orbital escape time
   $T_\text{escape}(g)$.  The signal: monotonic decrease in
   $T_\text{escape}$ with $g$, sharp drop near $g_\text{crit} =
   \Delta\omega_\text{tongue} / R_1 \approx 0.016$ per node.
3. **Observational:** predict the temperature-independent inner
   edge of neutral hydrogen near compact objects (the *atomic
   ISCO*, analogue of the geodesic ISCO).  Test against 21\,cm HI
   absorption morphology near neutron stars and Fe~K$\alpha$ line
   profiles in accretion disks.

---

## Paper Title and Theme

**Title:** Tidal Ionization of Atomic Hydrogen and the Quantum
Roche Limit.

**Series:** Paper~III of the A=1 Discrete Causal Lattice series.

**Anchor:** Paper~I's follow-on catalogue entry~\#9 (Tidal
Ionization Mass), in `external/dcl/notes/follow_on_implications.md`.

**Core framing:** the quantum Roche limit is a *parameter-free*
prediction.  Given Paper~I's Arnold-tongue width (measured from
`exp_09`) and Bohr radius (measured from `exp_12`), the threshold
$M_\text{min}(d)$ falls out of one substitution.  The $d^3$ scaling
identifies it as the structural twin of the classical Roche limit
-- same physics seen from two levels of description; in the
lattice they are the same thing.

---

## Audit Table Status (mirrors `paper/sections/audit_table.tex`)

**Inherited from Paper~I (PASS):**

| Row | Status | What it claims |
|---|---|---|
| Hydrogen as joint two-session Arnold-tongue lock-in | PASS | `exp_12` (Paper~I), 4-sig-figs agreement with Bohr |
| Gradient detuning breaks the joint resonance | PASS | `notes/plasma_as_gravitational_ionization.md` (analytical) |

**Paper~III-specific (mostly STUB in v0.1):**

| Row | Status | What it claims |
|---|---|---|
| Quantum Roche limit $M_\text{min}(d) \cdot R_1 / d^3 = \Delta\omega_\text{tongue}$ | STUB | Paper-text derivation pending |
| $d^3$ scaling is the quantum-mechanical Roche analogue | STUB | Paper-text observation pending |
| Three ionization channels (thermal / pressure / gradient) | PART | Inherited from Paper~I; paper-text consolidation pending |
| Numerical confirmation: monotone $T_\text{escape}(g)$ + sharp drop near $g_\text{crit}$ | STUB | `exp_18` script in place; runtime evidence pending first run |
| Opposite-sign tidal-displacement signature | STUB | Same script; same runtime gap |
| Atomic ISCO $r_\text{atomic\_ISCO}(M)$ | STUB | Paper-text derivation pending |
| 21\,cm HI inner-edge observational signature | STUB | Paper-text comparison pending |
| Fe~K$\alpha$ line-profile inner edge in accretion disks | STUB | Paper-text comparison pending |

The claim-auditor agent (`.claude/agents/claim-auditor.md`)
treats `audit_table.tex` as the authority; this section is for
quick orientation only.

---

## Engine

`src/experiments/exp_18_tidal_ionization.py` imports framework
primitives (`OctahedralLattice`, `CausalSession`,
`enforce_unity_spinor`) from `dcl_core.core` -- the
continuous-amplitude submodule of the shared `dcl_core` package
([github.com/JackDMenendez/dcl-core](https://github.com/JackDMenendez/dcl-core)),
which contains a verbatim port of Paper~I's original
`src/core/`.

The dependency is declared in `virtual-env-requirements.txt`:

```text
dcl_core @ git+https://github.com/JackDMenendez/dcl-core@main
```

Pinned to `@main` during dcl_core's pre-v0.1.0 phase.  At Paper~III's
v1.0 release, update the pin to a `@vX.Y.Z` tag so the Zenodo
deposit is bound to an immutable engine version (and that engine
version itself is Zenodo-deposited for archival reproducibility).

**Pre-release rule (READ THIS BEFORE EVERY PAPER III RELEASE).**

Before depositing Paper~III on Zenodo, the `dcl_core` pin in
`virtual-env-requirements.txt` MUST point at a `@vX.Y.Z` tag, not
`@main`.  A paper that ships pinned to `@main` is non-reproducible:
a reviewer cloning the tag in two years' time would get whatever
`dcl_core`'s `main` is at that point, not the engine version this
paper was actually run against.

The pre-release pin-bump workflow lives in `release_notes/README.md`
(*Pre-release: bump pinned dcl_core to a tagged release* section).
The co-released order is fixed: deposit `dcl-core` first to get
its Zenodo DOI, then bump Paper~III's pin and `CITATION.cff`
reference, then deposit Paper~III.  Do not reverse the order.

No manual engine setup is required: `setup.cmd` / `setup.sh`
installs `dcl_core` into `.venv` automatically.  This Paper~III
repo's own `src/core/` is empty (template default) and is
reserved for paper-specific engine extensions if any ever land
here; the engine *itself* lives upstream in `dcl_core`.

**Why pip-install rather than vendor** (the architectural decision
recorded here for the future-self / fresh Claude session).  The
A=1 series originally vendored copies of the engine in each paper
repo (Paper~I, Paper~II's audit-table scripts, the zoo).  For
Paper~III the user chose to introduce a shared `dcl_core` package
so that:

- subsequent papers in the series (#1 plasma, #2 recombination,
  proton internals, ...) share one engine source-of-truth;
- engine bug fixes propagate by version bump, not by N-paper
  copy-paste;
- `dcl_core` itself can be Zenodo-deposited and DOI-cited as a
  standalone software artifact.

The cost is a runtime dependency on an external git URL (or PyPI,
once dcl_core ships there).  The mitigation is to pin paper
releases to immutable tags and Zenodo-deposit the engine at the
same cadence as the papers.

For the alternative architecture (copy `dcl_core.core` into
Paper~III verbatim, vendor-style) see `dcl_core`'s own `CLAUDE.md`
"Downstream papers" section, which records the trade-off.

---

## Conventions

- **Status legend.** `PASS` / `PART` / `STUB` / `FAIL` (defined in
  the front-matter of `paper/main.tex`).
- **File naming.** Sections: `paper/sections/<topic>.tex`.
  Figures: `paper/figures/<name>.{tex,pdf,png}` with `.tex`
  fragment + binary pair.  Notes: `notes/<topic>.md`.  Experiments:
  `src/experiments/exp_NN_<name>.{py,md}`.
- **Cross-references.** Always `\label{}` + `\ref{}` /
  `\autoref{}`, never hard-coded numbers.
- **Bibliography.** All cites flow through
  `paper/paper-bib/references.bib`.  Style:
  `\bibliographystyle{unsrt}` (numeric, in citation order).
- **LaTeX layout idioms.** `\nolinkurl{}` for paths, `\url{}` for
  URLs inside `\href{}`.  `longtable` for tables that may span
  pages.

## Documentation convention for code

Every non-trivial line of physics/framework code should say what
it **is** in the theory, not just what it does in the program.
Name the mathematical object, cite the paper section/equation,
and use "IS" for exact correspondences, "approximates" for
continuum limits.  This convention is inherited from Paper~I and
Paper~II.

---

## Release flow

See `release_notes/README.md` for the full procedure (template
default applies; same flow as Paper~I, Paper~II, and the
generator-zoo).  Short version: deposit on Zenodo first to get
the DOI, commit the version bump after the DOI is in hand, build
the final PDF, tag, push, GitHub Release.  Add the deposit to the
`a1-discrete-causal-lattice` Zenodo community.

---

## What NOT to Change

- **`src/experiments/exp_18_tidal_ionization.py`:** copy of the
  Paper~I v1.0-released version.  Edit *only* if extending the
  calculation; do not refactor without justification.  The
  Provenance block in the script's docstring documents this.
- **`notes/plasma_as_gravitational_ionization.md`:** copy of the
  Paper~I version with Provenance prefix.  Edit only to add
  Paper~III-specific commentary at the bottom.
- **`paper/sections/audit_table.tex`:** once Paper~III's v1.0 is
  deposited on Zenodo, the audit table is part of the released
  artifact.

---

## Cross-references to Paper~I

Paper~I is the upstream of record (v1.0 at
[doi:10.5281/zenodo.20078529](https://doi.org/10.5281/zenodo.20078529)).
Expose as a junction:

```text
external/dcl  ->  C:\dev\dcl
```

To (re)create on Windows:

```bat
mkdir external
mklink /J external\dcl C:\dev\dcl
```

The junction is essential for option (b) of *Engine dependency*
above; even under option (a), having Paper~I available locally
lets Claude / agents cross-reference the framework's origin notes
and the upstream `exp_18` source-of-truth.

---

## Cross-references to Paper~II (dcl-sm-derivation)

Paper~II is largely orthogonal to Paper~III's tidal-ionization
focus, but the series identity benefits from cross-citing.
Expose as:

```text
external/dcl-sm-derivation  ->  C:\dev\dcl-sm-derivation
```

---

## Cross-references to the generator zoo

Not directly relevant to Paper~III's calculation, but useful for
cross-checks on the framework's algebraic structure.  Expose as:

```text
external/dcl-generator-zoo  ->  C:\dev\dcl-zoo
```

---

## Cross-references to physics-research (notation / formalization)

The parallel formalization effort lives in:

```text
external/research  ->  C:\dev\physics-research
```

Highlights for Paper~III work:

- `external/research/Notes/balanced_equations/` -- symbol-meaning
  catalogues; should record the $M_\text{min}(d)$ symbol and its
  scaling once the paper-text derivation lands.

---

## Notes Index

- `notes/README.md` -- conventions for `notes/`.
- `notes/plasma_as_gravitational_ionization.md` -- inherited from
  Paper~I with Provenance prefix.  The Arnold-tongue /
  clock-density-gradient theoretical apparatus this paper builds
  on.  Includes the three-channel ionization taxonomy and the
  $1/n^4$ Arnold-tongue compression result for higher orbitals.
- `notes/quantum_roche_limit.md` -- DRAFT, new in Paper~III.
  Synthesises Paper~I's gradient-ionization argument and Paper~I
  follow-on \#9's $M/d^3$ scaling observation into the explicit
  $M_\text{min}(d) = \Delta\omega_\text{tongue} \cdot d^3 / R_1$
  derivation that the paper's main body extends.  Includes the
  atomic-ISCO outline ($r_\text{atomic\_ISCO}(M) = (M R_1 /
  \Delta\omega_\text{tongue})^{1/3}$) and the open question of
  whether it coincides with the photon sphere.

(List additional notes here as they accumulate.)
