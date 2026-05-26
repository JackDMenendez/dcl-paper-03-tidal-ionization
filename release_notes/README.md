# release_notes/

One folder per released version of Paper~III, with these artefacts
per release:

- `vX.Y.md` -- **change log** for the release.  Long-form, internal:
  what changed, why, what's deferred, bibliography additions,
  reproducibility instructions.
- `vX.Y-release-message.md` -- **GitHub Release body**.  Outward-
  facing headline + audit-status delta; posted alongside the tag.
- `vX.Y-zenodo-description.txt` -- text pasted into Zenodo's
  **Description** field at deposit time.  Plain text; format
  modelled on
  `C:\dev\dcl-sm-derivation\release_notes\zenodo_description.txt`.
- `zenodo_references.txt` -- text pasted into Zenodo's
  **References** field (bibliographic citations, one per line).
  Durable: reuse and edit each release.
- `zenodo_related_works.txt` -- entries for Zenodo's
  **Related / alternate identifiers** section.  Durable: reuse
  and edit each release.

Templates:

- `TEMPLATE.md` for the change log
- `TEMPLATE-release-message.md` for the Release body

## Pre-release: bump pinned `dcl_core` to a tagged release

Paper~III depends on the shared `dcl_core` engine via a git URL in
`virtual-env-requirements.txt`.  **Before the release flow below,
the pin MUST point at a `@vX.Y.Z` tag, not `@main`.**  A paper that
ships pinned to `@main` is non-reproducible: a reviewer cloning
the tag in two years' time would get whatever `dcl_core`'s `main`
is at that point, not the engine version the paper was actually
run and deposited against.

Workflow:

1. **Confirm `dcl-core` has a tagged release.**  If not, cut one
   in the `dcl-core` repo first (see
   `dcl-core/release_notes/README.md`), then come back here.

2. **Update `virtual-env-requirements.txt`:** replace the existing
   pin with the tagged version, e.g.

   ```text
   dcl_core @ git+https://github.com/JackDMenendez/dcl-core@v0.1.0
   ```

3. **Add a `references:` entry to `CITATION.cff`** citing the
   `dcl_core` release's Zenodo DOI:

   ```yaml
   references:
     - type: software
       title: "dcl_core: Core library for the A=1 Discrete Causal Lattice framework"
       authors:
         - family-names: Menendez
           given-names: Jack
       version: vX.Y.Z
       doi: 10.5281/zenodo.XXXXXXXX
       year: 2026
       notes: "Engine release Paper III's exp_18 ran against; pinned in virtual-env-requirements.txt to this exact tag."
   ```

4. **Reinstall the venv** to pick up the pinned version.  Use the
   project's `refresh-deps.{sh,cmd}` helper if present (which
   handles PEP 668 on MSYS2 and surgically force-reinstalls only
   `dcl_core`); otherwise:

   ```text
   pip install -r virtual-env-requirements.txt --force-reinstall --no-deps dcl_core
   ```

5. **Re-run `exp_18` end-to-end** as a quick check that nothing
   broke under the pin change.

6. **Commit "Pre-release: pin dcl_core to vX.Y.Z"** as the paper's
   pre-release checkpoint before continuing.

Do this even if you believe the engine has not changed; the
discipline of pin-then-rerun is what guarantees the deposit's
reproducibility.

## Release flow (v2 -- 2026-05-26)

This protocol applies uniformly to every subproject in the
A=1 Discrete Causal Lattice series (dcl-core, dcl-delta-p-min,
dcl-paper-03, future ones).  Each step is owned by either
**Claude** (the agent running inside this repo) or **User**;
do not skip an owner-marked step.  Steps marked **conditional**
apply only if the named artefact exists in this repo.  For
Paper~III, both step 8 and step 14 apply (`paper/main.tex`
exists).

| # | Step | Owner |
|---|---|---|
| 1 | CI green on `main`. | Claude |
| 2 | Bump `CITATION.cff` (`version`, `date-released`).  (Paper~III ships no Python package; no `_version.py` to bump.) | Claude |
| 3 | Draft `release_notes/vX.Y.md` and `release_notes/vX.Y-release-message.md`. | Claude |
| 4 | Draft `release_notes/vX.Y-zenodo-description.txt` (model: `dcl-sm-derivation/release_notes/zenodo_description.txt`). | Claude |
| 5 | Draft or update `release_notes/zenodo_references.txt`. | Claude |
| 6 | Draft or update `release_notes/zenodo_related_works.txt`. | Claude |
| 7 | Run unit tests if any exist; `python audit_universe.py` must show no PASS rows regressed. | Claude |
| 8 | **`paper/main.tex` exists:** | |
| 8a |   Add version to title in `main.tex`. | Claude |
| 8b |   Review abstract, introduction, conclusion, `References.bib`; make necessary changes. | Claude |
| 8c |   Build `main.tex` to `build/` (`./build.sh paper` or `build.cmd paper`). | Claude |
| 8d |   Review PDF in `build/`. | User |
| 9 | Run `export-vscode-extensions.{cmd,sh}` -> tracked `extensions.txt` at repo root. | Claude |
| 10 | Run `generate-dockerfile.{cmd,sh}` -> tracked `Dockerfile`. | Claude |
| 11 | Reserve a Zenodo DOI (Zenodo "New upload" -> *Reserve DOI*) and supply the DOI string to Claude. | User |
| 12 | DOI lands in `release_notes/vX.Y.md`. | Claude |
| 13 | DOI lands in `CITATION.cff` (`doi:` field). | Claude |
| 14 | **`paper/main.tex` exists:** | |
| 14a |   DOI lands in `main.tex` (`\thanks{}` block, replacing the placeholder URL with `https://doi.org/10.5281/zenodo.NNNNNNNN`). | Claude |
| 14b |   Rebuild PDF. | Claude |
| 14c |   Final document check (title page, audit table, newly-added sections). | User |
| 14d |   Rename PDF to `stage/dcl-paper-03-tidal-ionization-vX.Y.pdf` (durable per-version snapshot). | User |
| 14e |   Upload the snapshotted PDF to Zenodo; confirm the deposit was added to the `a1-discrete-causal-lattice` Zenodo community. | User |
| 15 | Upload software files (data products, source archive, etc.) to Zenodo. | User |
| 16 | Commit generated files + version bump (DOI included). | Claude |
| 17 | Tag `vX.Y` and push the tag.  (Tags are immutable once pushed; Claude must surface what it is about to do before running this.) | Claude |
| 18 | Create the GitHub Release draft using the `vX.Y-release-message.md` body. | User |
| 19 | (Optional) Publish to PyPI -- **N/A for Paper~III** (no Python package). | -- |
| 20 | Publish the GitHub Release (click *Publish* in the GitHub UI). | User |
| 21 | Supply Claude with the project-plan delta needed for the release. | User |
| 22 | Update project plan with release info. | Claude |
| 23 | Update GitHub project board. | User |

## Helper scripts required by steps 9 and 10

The two helper scripts referenced by steps 9 and 10 **do not yet
exist** in this repo (or in any other DCL subproject as of
2026-05-26).  Any release that runs this protocol is blocked at
those steps until the scripts are created.

- `export-vscode-extensions.cmd` / `.sh` should produce
  `extensions.txt` at the repo root, containing one VS Code
  extension ID per line (the output of `code --list-extensions`).
- `generate-dockerfile.cmd` / `.sh` should produce a tracked
  `Dockerfile` that reproduces the development environment
  sufficiently to run this repo's experiments / tests.

When the canonical implementations of these scripts land -- likely
in the user's `wcde` repo (`C:\dev\wcde`) or in
`dcl-sm-derivation`'s `release_notes/` -- copy them into each
subproject.  Until then, Claude must stop at steps 9-10 and ask
the User how to proceed.

## Immutability

Once a tag is pushed and the GitHub Release is published, **the
released version is immutable**.  Do not amend the tagged commit.
Do not re-deposit on Zenodo.  A typo in the release notes gets a
follow-up PATCH release; do not rewrite history.
