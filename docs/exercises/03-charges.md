# 3 · Atomic charges (Hirshfeld)

Calculate atomic partial charges of acetone.

Partial charges describe how the electrons are distributed over the atoms: which atoms
are electron-rich (negative) and which are electron-poor (positive). They help to
rationalise reactivity, e.g. where a nucleophile attacks. The electron density itself
is not divided into atoms, though; every charge model uses its own rule to split it up.

!!! tip "First time running ORCA?"
    See [Running ORCA](../orca.md) for how to run an input file from the command line.
    `/path/to/orca` below is the full path to your ORCA program.

## Input

<div class="buttons" markdown>
[:material-download: acetone_charges.inp](../files/ex03/acetone_charges.inp){ .md-button download="acetone_charges.inp" }
[:material-file-document-outline: acetone_charges.out](../files/ex03/acetone_charges.out){ .md-button download="acetone_charges.out" }
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

The results from `acetone_charges.out`:

| Atom | Hirshfeld | Mulliken | Löwdin |
| ---- | --------- | -------- | ------ |
| 0 O | −0.271 | −0.217 | −0.165 |
| 1 C (C=O) | +0.186 | −0.051 | +0.083 |
| 2 C (CH<sub>3</sub>) | −0.071 | +0.046 | −0.074 |
| 3 C (CH<sub>3</sub>) | −0.071 | +0.046 | −0.074 |
| 4–9 H | +0.037 to +0.038 | +0.028 to +0.030 | +0.036 to +0.040 |

Atomic charges are not observables: different models give different numbers for the
same molecule. All three agree that the oxygen is negative, but Mulliken even gives the
carbonyl carbon a negative charge. Compare charges of atoms or molecules **within one
model**, never across models.

The two methyl groups have the same charges, as expected from the symmetry of acetone,
and in each model the charges add up to the total charge of the molecule (0).

!!! info "Other charge models"
    `! CHELPG` gives charges fitted to the electrostatic potential, `! MBIS` gives
    MBIS charges. Add them to the `!` line to compare.

## ORCA documentation

- [Population analysis (manual)](https://www.faccts.de/docs/orca/6.1/manual/contents/spectroscopyproperties/population.html)
- [Charge models (tutorial)](https://www.faccts.de/docs/orca/6.1/tutorials/prop/charges.html)
