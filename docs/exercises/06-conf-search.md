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

## Input

<div class="buttons" markdown>
[:material-download: conf_search.inp](../files/ex06/conf_search.inp){ .md-button download="conf_search.inp" }
[:material-file-document-outline: conf_search.out](../files/ex06/conf_search.out){ .md-button download="conf_search.out" }
[:material-molecule: conf_search.finalensemble.xyz](../files/ex06/conf_search.finalensemble.xyz){ .md-button download="conf_search.finalensemble.xyz" }
</div>

```text title="conf_search.inp"
--8<-- "docs/files/ex06/conf_search.inp"
```

| Keyword | Meaning |
| ------- | ------- |
| `GOAT` | ORCA's conformer search (Global Optimizer Algorithm) |
| `Native-XTB2` | GFN2-xTB, built into ORCA. (Plain `XTB` would need the separate xtb program.) |

```bash
/path/to/orca conf_search.inp > conf_search.out
```

!!! warning "Use several cores: add `PAL`"
    The example output was run on **one core and took 50 minutes**. GOAT runs many
    independent optimisations, so it profits a lot from more cores. Add `PAL4` (or
    `PAL8`, up to the number of cores of your computer) to the keyword line:

    ```text
    ! GOAT Native-XTB2 PAL4
    ```

    See [Cores and memory](../orca.md#cores-and-memory). You can also download the
    output files above and look at the results without running it yourself.

## What GOAT does

GOAT works in rounds ("global iterations"). In each round, 8 **workers** each run 27
short simulations: the molecule is pushed uphill along random directions (at a
"temperature" between 363 and 2904 K, i.e. with more or less energy), which lets it
cross barriers, e.g. flip the ring. Each new structure is then optimised. Every
structure found is compared with the ones already known (by RMSD, energy and rotational
constants), duplicates are thrown away, and the new lowest structure becomes the start
for the next round. GOAT stops when several rounds in a row find no lower minimum.

You can follow this in `conf_search.out`:

| Search for | What it is |
| ---------- | ---------- |
| `Global parameters` | Settings: number of workers, optimisations per step, `Number of available CPUs` (here 1) |
| `Filtering criteria` | When two structures count as the same conformer, and the maximum energy (6 kcal/mol) kept in the ensemble |
| `GOAT Global Iter` | Progress: the lowest energy found so far after each round |
| `Global minimum found!` | Search finished |
| `Final ensemble info` | **The result**: all conformers with relative energy and Boltzmann population |

ORCA also writes one `conf_search.goat.X.Y.out` file per worker and round; you can
ignore these.

## Output

```text
		Global minimum found!
		Writing structure to conf_search.globalminimum.xyz

		# Final ensemble info #
		Conformer     Energy     Degen.   % total   % cumul.
		              (kcal/mol)
		------------------------------------------------------
		        0     0.000	     1      86.82      86.82
		        1     1.142	     1      12.64      99.46
		        2     3.034	     1       0.52      99.98
		        3     5.102	     1       0.02      99.99
		        4     5.595	     1       0.01     100.00

		Conformers below 3 kcal/mol: 2
		Lowest energy conformer    : -26.200312 Eh
```

GOAT found 5 conformers within 6 kcal/mol. `Energy` is relative to the lowest one;
`% total` is the Boltzmann population at 298 K, i.e. how much of each conformer is
present at room temperature. Two conformers make up more than 99 %: a difference of
1.1 kcal/mol already means a ratio of about 87 : 13.

| File | Contents |
| ---- | -------- |
| `conf_search.globalminimum.xyz` | The lowest-energy conformer |
| `conf_search.finalensemble.xyz` | All conformers, sorted by energy. Open it in a viewer (e.g. [Avogadro](https://www.openchemistry.org/projects/avogadro2/)) to step through them |

!!! question "Try it"
    Open `conf_search.finalensemble.xyz` and assign each conformer: which chair is it,
    and are Cl and CH<sub>3</sub> axial or equatorial? Does the order of energies match
    what you expect from A-values? (GFN2-xTB is fast but approximate; for reliable
    energies the best conformers would be re-optimised with DFT.)

## ORCA documentation

- [Conformer search with GOAT (tutorial)](https://www.faccts.de/docs/orca/6.1/tutorials/prop/goat.html)
- [GOAT (manual)](https://www.faccts.de/docs/orca/6.1/manual/contents/structurereactivity/goat.html)
- [Semi-empirical methods incl. xTB (manual)](https://www.faccts.de/docs/orca/6.1/manual/contents/modelchemistries/semiempirical.html)
