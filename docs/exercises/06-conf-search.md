# 6 · Conformer search

Find the conformers of 1-chloro-2-methylcyclohexane, e.g. the chair forms with Cl and
CH<sub>3</sub> axial or equatorial, and identify the lowest-energy one.

!!! tip "First time running ORCA?"
    See [Running ORCA](../orca.md) for how to run an input file from the command line.
    `/path/to/orca` below is the full path to your ORCA program.

A geometry optimisation only finds the nearest minimum to the start structure. A
conformer search runs many optimisations from different starting points to find the other
minima as well. This needs a lot of calculations, so a fast method is used: here the
semi-empirical method GFN2-xTB.

## Step 1 · Conformer search with GOAT

<div class="buttons" markdown>
[:material-download: conf_search.inp](../files/ex06/conf_search.inp){ .md-button download="conf_search.inp" }
</div>

```text title="conf_search.inp"
--8<-- "docs/files/ex06/conf_search.inp"
```

| Keyword | Meaning |
| ------- | ------- |
| `GOAT` | ORCA's conformer search (Global Optimizer Algorithm) |
| `Native-GFN2-xTB` | GFN2-xTB, built into ORCA. (Plain `XTB` would need the separate xtb program.) |

```bash
/path/to/orca conf_search.inp > conf_search.out
```

This runs many optimisations and takes longer than the other exercises. On several cores
it is much faster, see [Cores and memory](../orca.md#cores-and-memory).

### Output

| File / section | Contents |
| -------------- | -------- |
| `conf_search.globalminimum.xyz` | The lowest-energy conformer |
| `conf_search.finalensemble.xyz` | All conformers found, sorted by energy. Open it in a viewer to step through them |
| Table at the end of `conf_search.out` | Each conformer with its relative energy (kcal/mol) and Boltzmann population (%) at room temperature |

## Step 2 · Refine the best conformer (optional)

GFN2-xTB is fast but approximate. For reliable structures and energies, re-optimise the
best conformer(s) with DFT and check them with a frequency calculation
([Exercise 5](05-freq.md)).

<div class="buttons" markdown>
[:material-download: best_conf_opt.inp](../files/ex06/best_conf_opt.inp){ .md-button download="best_conf_opt.inp" }
</div>

```text title="best_conf_opt.inp"
--8<-- "docs/files/ex06/best_conf_opt.inp"
```

`* xyzfile` reads the structure from the GOAT result, so run this in the same folder.
`D3BJ` adds a dispersion correction, which matters for comparing conformers.

```bash
/path/to/orca best_conf_opt.inp > best_conf_opt.out
```

## ORCA documentation

- [Conformer search with GOAT (tutorial)](https://www.faccts.de/docs/orca/6.1/tutorials/prop/goat.html)
- [GOAT (manual)](https://www.faccts.de/docs/orca/6.1/manual/contents/structurereactivity/goat.html)
- [Semi-empirical methods incl. xTB (manual)](https://www.faccts.de/docs/orca/6.1/manual/contents/modelchemistries/semiempirical.html)
