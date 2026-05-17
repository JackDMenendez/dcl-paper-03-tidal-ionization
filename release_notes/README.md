# release_notes/

One file per released version. Two files per release:

- `vX.Y.md` -- the **change log** for this release. Long-form,
  internal: what changed, why, what's deferred, bibliography
  additions, reproducibility instructions. This is the file the paper
  references from `release_notes/vX.Y.md`.
- `vX.Y-release-message.md` -- the **GitHub Release body**. The
  outward-facing version: headline change, audit-status delta, what's
  out of scope. Posted as the body of the GitHub Release alongside
  the tag.

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
       version: v0.1.0
       doi: 10.5281/zenodo.XXXXXXXX
       year: 2026
       notes: "Engine release Paper III's exp_18 ran against; pinned in virtual-env-requirements.txt to this exact tag."
   ```

4. **Reinstall the venv** to pick up the pinned version:

   ```text
   pip install -r virtual-env-requirements.txt --force-reinstall --no-deps dcl_core
   ```

5. **Re-run `exp_18` end-to-end** as a cheap sanity check that
   nothing broke under the pin change.

6. **Commit "Pre-release: pin dcl_core to vX.Y.Z"** as the paper's
   pre-release checkpoint before continuing.

Do this even if you believe the engine has not changed; the
discipline of pin-then-rerun is what guarantees the deposit's
reproducibility.

## Release flow

Pre-conditions: pre-release pin-bump done (above); final commits
landed on `main`; CI green.

1. **Final build + audit.**  `build.cmd paper` (POSIX:
   `./build.sh paper`) produces a candidate `build/main.pdf`.
   Run `python audit_universe.py` to confirm no rows have
   regressed.  Open the PDF and visually check the title page,
   audit table, and any newly-added sections.  This is the last
   chance to catch issues before the DOI gets locked to a
   snapshot.

2. **[User] Reserve a Zenodo DOI.**  In the Zenodo "New upload"
   form, click *Reserve DOI* to mint a DOI without publishing the
   deposit.  Copy the DOI string (form `10.5281/zenodo.NNNNNNNN`).
   The deposit stays in *Draft* status until step 6.

3. **Insert the DOI and rebuild the PDF.**  Add the DOI to:

   - `CITATION.cff`'s `doi:` field;
   - `paper/main.tex` `\thanks{}` block (replace the placeholder
     URL with `https://doi.org/10.5281/zenodo.NNNNNNNN`);
   - `release_notes/vX.Y.md` and
     `release_notes/vX.Y-release-message.md` header blocks.

   Update `CITATION.cff`'s `version:` and `date-released:` fields
   in the same pass.  Then rebuild:

   ```text
   build.cmd paper
   ```

   The fresh `build/main.pdf` now carries the real DOI on the
   title page -- this is the file that will be deposited.

4. **Snapshot the PDF to `.stage/`.**

   ```text
   mkdir .stage 2>NUL
   copy build\main.pdf .stage\dcl-paper-03-tidal-ionization_vX.Y.pdf
   ```

   `.stage/` is gitignored; it is the durable per-version archive
   that survives `make clean`, distinct from `build/` (disposable
   working area).

5. **Commit the version bump + DOI fill-in.**  Suggested message:

   ```text
   vX.Y release: fill DOI placeholders, snapshot PDF

   - DOI 10.5281/zenodo.NNNNNNNN added to CITATION.cff,
     paper/main.tex \thanks{}, release_notes/vX.Y*.md
   - Rebuilt PDF with the DOI in place
   - Snapshotted build/main.pdf -> .stage/dcl-paper-03-tidal-ionization_vX.Y.pdf
   ```

6. **[User] Upload the snapshotted PDF to Zenodo and publish.**
   Drag `.stage/dcl-paper-03-tidal-ionization_vX.Y.pdf` into the
   reserved-DOI draft deposit; fill in the metadata (title,
   authors, abstract, keywords, related identifiers pointing at
   Paper~I, Paper~II, and the pinned `dcl_core` Zenodo DOI);
   click *Publish*.  This locks in the DOI and the deposit
   becomes immutable.  Confirm the deposit was added to the
   `a1-discrete-causal-lattice` Zenodo community.

7. **Tag and push.**

   ```text
   git tag vX.Y
   git push origin vX.Y
   ```

   Tags are immutable once pushed.  Confirm with yourself before
   pushing.

8. **Create the GitHub Release.**

   ```text
   gh release create vX.Y \
       --title "vX.Y -- <one-line headline>" \
       --notes-file release_notes/vX.Y-release-message.md
   ```

   Confirm via `gh release view vX.Y` that the body rendered
   correctly.

For the *prior* release flow record on the parent project, see the
git history of the source repository this template was derived from.
