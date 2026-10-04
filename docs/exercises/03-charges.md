# 3 · Atomic charges (Hirshfeld)

Calculate atomic partial charges of acetone.

!!! tip "First time running ORCA?"
    See [Running ORCA](../orca.md) for how to run an input file from the command line.
    `/path/to/orca` below is the full path to your ORCA program.

## Input

<div class="buttons" markdown>
[:material-download: acetone_charges.inp](../files/ex03/acetone_charges.inp){ .md-button download="acetone_charges.inp" }
</div>

```text title="acetone_charges.inp"
--8<-- "docs/files/ex03/acetone_charges.inp"
```

The `Hirshfeld` keyword adds a Hirshfeld population analysis. The structure is taken
from PubChem and not optimised; for real work, optimise first (see
[Exercise 2](02-opt.md)).

```bash
/path/to/orca acetone_charges.inp > acetone_charges.out
```

## Output

Search `acetone_charges.out` for these sections. Each lists one charge per atom, with
atoms numbered from 0 in the order of the input.

| Search for | Charge model |
| ---------- | ------------ |
| `HIRSHFELD ANALYSIS` | Hirshfeld (requested with the `Hirshfeld` keyword) |
| `MULLIKEN ATOMIC CHARGES` | Mulliken (always printed) |
| `LOEWDIN ATOMIC CHARGES` | Löwdin (always printed) |

Atomic charges are not observables: different models give different numbers for the
same molecule. Compare the three.

!!! info "Other charge models"
    `! CHELPG` gives charges fitted to the electrostatic potential, `! MBIS` gives
    MBIS charges. Add them to the `!` line to compare.

## ORCA documentation

- [Population analysis (manual)](https://www.faccts.de/docs/orca/6.1/manual/contents/spectroscopyproperties/population.html)
- [Charge models (tutorial)](https://www.faccts.de/docs/orca/6.1/tutorials/prop/charges.html)
