# Computational Organic Chemistry: A Practical Introduction

Lecture materials for TUS, 7 October 2026.

**Website:** https://svatunek-lab.github.io/TUS_compchem_lecture/

## Layout

```
docs/
  index.md, setup.md, downloads.md   # general pages
  hands-on/      # one page per notebook (explanation + Colab/View/Download buttons)
  notebooks/     # the .ipynb files (Colab opens them from here)
  orca/          # ORCA explanation and example pages
  files/         # slides and other downloads; ORCA inputs in files/orca/
mkdocs.yml       # site config and navigation (nav:)
```

## Adding material

- **Notebook:** put the `.ipynb` in `docs/notebooks/`, copy a page in `docs/hands-on/`,
  change the file name in its three button links, and add the page to `nav` in `mkdocs.yml`.
- **ORCA example:** put the input files in `docs/files/orca/`, copy `docs/orca/opt-freq.md`
  and add it to `nav`. Use `--8<-- "docs/files/orca/<file>"` inside a code block to show a
  file's contents on the page.
- **Slides / other files:** put them in `docs/files/` and add a row in `docs/downloads.md`.

Pushing to `main` rebuilds and deploys the site (GitHub Actions).

## Preview locally

```bash
pip install -r requirements.txt
mkdocs serve
```

or, with uv: `uvx --with "mkdocs-material" --from "mkdocs<2" mkdocs serve`
