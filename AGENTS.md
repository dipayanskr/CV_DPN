# Repository Guidelines

## What This Repository Is

A LaTeX-source CV/resume for Dipayan Sarkar (PhD researcher, computational biology). `CV_DPN.tex` is the only actively maintained source file; `CV_DPN.pdf` is published to `https://dipayansarkar.com/CV_DPN/CV_DPN.pdf`.

## Project Structure & Module Organization

- `CV_DPN.tex` — the single-source CV; edit only this file.
- `ref.bib`, `notes.md`, `add.md`, `README.md` — supporting files; `ref.bib` is currently unused (Publications are hand-written with `\cvitem` + `\href`).
- `DPN_CV.docx` — a static Word copy, not generated from the `.tex` source; do not regenerate.
- `Photo/dpn_photo.jpg`, `hf_logo.png` — image assets referenced from the CV.
- `.github/workflows/compile.yml` — CI workflow that compiles the PDF on every push to `main`.
- Build auxiliaries (`.aux`, `.log`, `.out`, `.fls`, `.fdb_latexmk`) are gitignored; clean them up after local compiles.

## Build, Test, and Development Commands

Compile from the repository root with `pdflatex` or `latexmk`:

```sh
pdflatex -interaction=nonstopmode -halt-on-error CV_DPN.tex
```

Preview the first page as an image rather than assuming the LaTeX is correct:

```sh
pdftoppm -png -r 150 -f 1 -l 1 CV_DPN.pdf page_preview
```

There is no automated test suite; visual inspection of the rendered page is the verification step.

## Coding Style & Naming Conventions

- Preserve the exact class, style, and layout settings already declared in `CV_DPN.tex` (e.g., `moderncv`, `banking` style, `black` color); do not reformat the source unnecessarily.
- Header styling (name fonts, spacing patches, `\extrainfo` fields) is hardcoded — follow the existing patterns instead of ad hoc strings or duplicate icons.
- Do not `\usepackage{fontawesome5}` separately; icons come from `moderncv` itself to avoid clashes.
- Section order is fixed: Research Summary, Work Experience, Education, Publications, Skills, Software & Tools, Awards & Certificates, Grants & Computing Resources. Publications are newest-first via `\cvitem{year}{...}`; work and education use `\cventry{date}{title}{institution}{location}{grade}{description}`.

## CI Behavior: Don't Fight the Auto-Compile

On every push to `main`, CI recompiles and commits `CV_DPN.pdf` as `chore: auto-compile CV [skip ci]`. Therefore:

- Never hand-edit or commit `CV_DPN.pdf`; let CI regenerate it after your change.
- On a fresh clone, run `git update-index --skip-worktree CV_DPN.pdf` once so local preview compiles don't dirty `git status` or conflict with CI's binary commits.

## Commit & Pull Request Guidelines

- Existing history mixes generic messages (`update`) with prefixed ones (`chore:`, `docs:`, `ci:`). Prefer Conventional Commits-style prefixes over bare `update`.
- Keep changes focused on `CV_DPN.tex`; describe what changed and why in the commit body when it is not obvious.
- PRs should describe the content change, note whether the CV still fits the intended page count, and confirm the compiled PDF renders correctly.
