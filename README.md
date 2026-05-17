# dcl-paper-03-tidal-ionization

Paper~III of the A=1 Discrete Causal Lattice series: tidal
ionization of atomic hydrogen and the quantum Roche limit.

The hydrogen atom in the A=1 framework (Paper~I) is a joint
Arnold-tongue attractor between the proton and electron sessions.
An external mass $M$ at distance $d$ imposes a clock-density
gradient across the atom of order $M \cdot R_1 / d^3$.  When this
tidal detuning exceeds the Arnold-tongue width
$\Delta\omega_\text{tongue}$, the joint resonance breaks and the
orbit dissolves -- *gradient ionization*, a third channel beyond
thermal and pressure ionization.  The threshold

$$M_\text{min}(d) \;=\; \Delta\omega_\text{tongue} \cdot d^3 \,/\, R_1$$

is the **quantum Roche limit**: a parameter-free,
temperature-independent prediction with the same $d^3$ scaling as
the classical Roche limit, $\Delta\omega_\text{tongue}$ playing
the role of the classical body density.

The paper's three legs:

1. **Analytical** derivation of $M_\text{min}(d)$ from the
   Arnold-tongue width condition;
2. **Numerical** confirmation via the inherited `exp_18` g-scan
   ($T_\text{escape}(g)$ monotone decrease, sharp drop at
   $g_\text{crit}$, opposite-sign tidal-displacement signature);
3. **Observational** prediction for 21\,cm HI inner-edge
   morphology near compact objects and Fe~K$\alpha$ line profiles
   in accretion disks (the *atomic ISCO*, analogue of the
   geodesic ISCO at $r = 6 GM/c^2$).

## Status: v0.1-DRAFT (2026-05-16)

What is in place:

- Theoretical apparatus (`notes/plasma_as_gravitational_ionization.md`)
  inherited verbatim from Paper~I.
- Numerical experiment (`src/experiments/exp_18_tidal_ionization.py`)
  inherited verbatim from Paper~I.
- Analytical derivation outline (`notes/quantum_roche_limit.md`,
  DRAFT) -- the synthesis the paper's main body extends.
- Paper skeleton with abstract, introduction, audit table, and
  bibliography all Paper~III-specific.

What is pending:

- Engine provisioning decision (see `CLAUDE.md`'s *Engine
  dependency* section).  The experiment depends on Paper~I's
  `src/core/`, ~1450 lines across 6 files; choice between
  copying into this repo, referencing via junction, or deferring.
- First run of the experiment from this repo's environment.
- Phase~2 paper drafting: main-body sections derived from
  `notes/quantum_roche_limit.md`, then observational comparison
  sections.

## Upstream

- **Paper~I, *Geometry First***
  [doi:10.5281/zenodo.20078529](https://doi.org/10.5281/zenodo.20078529)
  -- the framework, the `exp_12` Arnold-tongue lock-in result, the
  `notes/plasma_as_gravitational_ionization.md` theoretical
  apparatus, and the `exp_18_tidal_ionization.py` numerical
  experiment script.
- **Paper~II, *Geometry Forces Physics***
  [doi:10.5281/zenodo.20240736](https://doi.org/10.5281/zenodo.20240736)
  -- companion gauge-derivation paper; orthogonal to Paper~III's
  tidal-ionization focus.
- **[`a1-discrete-causal-lattice`](https://zenodo.org/communities/a1-discrete-causal-lattice/)
  Zenodo community** -- where Paper~III's v1.0 deposit will land
  alongside Paper~I and Paper~II.

## What you get

```text
.
├── paper/                       (LaTeX paper: 3-leg analytical/numerical/observational)
│   ├── main.tex
│   ├── macros/
│   ├── sections/                introduction.tex, audit_table.tex,
│   │                            abstract.tex, code_and_data.tex,
│   │                            reproducibility.tex, ...
│   ├── figures/
│   └── paper-bib/references.bib
├── src/
│   ├── core/                    (engine; provisioning pending --
│   │                             see CLAUDE.md "Engine dependency")
│   └── experiments/             exp_18_tidal_ionization.{py,md}
│                                (inherited verbatim from Paper I)
├── tests/                       pytest scaffolding
├── data/                        exp_18 output lands here (.npy + .log)
├── notes/                       plasma_as_gravitational_ionization.md
│                                (inherited), quantum_roche_limit.md (new)
├── external/                    junctions to dcl, dcl-sm-derivation, etc.
│                                (see CLAUDE.md)
├── release_notes/               per-version change log
├── .claude/agents/claim-auditor.md  read-only audit agent
├── audit_universe.py            master PASS/STUB/FAIL roll-up
├── audit_universe.md            audit-model documentation
├── virtual-env-requirements.txt numpy + scipy + matplotlib + pytest
├── CLAUDE.md                    project memory for Claude Code
├── CITATION.cff                 machine-readable citation
├── LICENSE                      MIT (code) / CC BY 4.0 (paper text)
├── makefile common.mak          root build (paper + tests + experiments)
├── build.{sh,cmd}               platform wrappers around make
└── setup.{sh,cmd}               create venv + install requirements
```

## Quickstart

```sh
# 1. Create the venv and install dependencies (numpy + scipy + dcl_core)
#    dcl_core (the framework engine) installs automatically via the
#    `dcl_core @ git+...` line in virtual-env-requirements.txt.
./setup.sh                       # POSIX / MSYS2 UCRT64 on Windows
setup.cmd                        # Windows cmd / PowerShell

# 2. Sanity-check the toolchain
./build.sh tests                 # pytest against tests/
./build.sh paper                 # pdflatex 3-pass + bibtex

# 3. Run the experiment (~hours)
python -u src/experiments/exp_18_tidal_ionization.py \
    | tee data/exp_18_tidal_ionization.log

# 4. Master audit roll-up
python audit_universe.py
```

## License

Paper text and figures: CC BY 4.0.
Source: MIT (see `LICENSE`).

## Citing this paper

See `CITATION.cff`.  Until the v1.0 Zenodo DOI is assigned, cite
as:

> Menendez, J. (2026).  *Tidal Ionization of Atomic Hydrogen and
> the Quantum Roche Limit*, v0.1-DRAFT.  Paper~III of the A=1
> Discrete Causal Lattice series.  GitHub:
> `JackDMenendez/dcl-paper-03-tidal-ionization`.

Once v1.0 is deposited, the DOI will be added to `CITATION.cff`
and to `paper/main.tex`'s title-page `\thanks{}` block.
