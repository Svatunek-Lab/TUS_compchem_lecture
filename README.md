# Computational Organic Chemistry: A Practical Introduction

Lecture materials for TUS, 7 October 2026.

**Website:** https://svatunek-lab.github.io/TUS_compchem_lecture/

## Layout

```
docs/
  index.md, orca.md, downloads.md
  exercises/     # one page per exercise: description, ORCA inputs, links to ORCA docs
  files/         # slides; ORCA inputs per exercise in files/exNN/
mkdocs.yml       # site config and navigation (nav:)
```

## Adding an exercise

1. ORCA inputs → `docs/files/exNN/`
2. Copy `docs/exercises/01-sp-scan.md` and change the file names.
   Input files are shown on the page with `--8<-- "docs/files/exNN/file.inp"` inside a code block.
3. Add the page to `nav` in `mkdocs.yml`, to `docs/exercises/index.md` and to `docs/downloads.md`.

Download links need the file name, otherwise browsers save the file as "download":
`[n2_sp.inp](../files/ex01/n2_sp.inp){ .md-button download="n2_sp.inp" }`

Slides go to `docs/files/slides.pdf`.

Pushing to `main` rebuilds and deploys the site (GitHub Actions).

## Preview locally

```bash
pip install -r requirements.txt
mkdocs serve
```

or, with uv: `uvx --with "mkdocs-material" --from "mkdocs<2" mkdocs serve`
