# Computational Organic Chemistry: A Practical Introduction

Lecture materials for TUS, 7 October 2026.

**Website:** https://svatunek-lab.github.io/TUS_compchem_lecture/

## Layout

```
docs/
  index.md, setup.md, notebooks.md, downloads.md   # site pages
  notebooks/   # .ipynb files (linked from notebooks.md, opened in Colab from here)
  files/       # slides and other downloads (linked from downloads.md)
mkdocs.yml     # site config and navigation
```

## Adding material

- **Notebook:** put the `.ipynb` in `docs/notebooks/` and copy a row in `docs/notebooks.md`.
- **Slides / files:** put them in `docs/files/` and add a row in `docs/downloads.md`.

Pushing to `main` rebuilds and deploys the site (GitHub Actions).

## Preview locally

```bash
pip install -r requirements.txt
mkdocs serve
```

or, with uv: `uvx --with "mkdocs-material" --from "mkdocs<2" mkdocs serve`
