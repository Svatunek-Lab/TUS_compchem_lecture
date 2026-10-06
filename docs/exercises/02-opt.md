# 2 · Geometry optimisation

**Part A:** optimise N<sub>2</sub>, starting from a stretched bond (1.40 Å).
**Part B:** optimise a larger molecule (aspirin) with a faster method.

!!! tip "First time running ORCA?"
    See [Running ORCA](../orca.md) for how to run an input file from the command line.
    `/path/to/orca` below is the full path to your ORCA program.

A **geometry optimisation** searches for the nearest minimum on the potential energy
surface. In each cycle ORCA calculates the energy and the **gradient** (the forces on the
atoms), moves the atoms downhill, and repeats until the forces are close to zero. In
[Exercise 1](01-sp-scan.md) you found the minimum of N<sub>2</sub> roughly by scanning;
an optimisation finds it exactly, with far fewer calculations.

## Part A · N<sub>2</sub>

<div class="buttons" markdown>
[:material-download: n2_opt.inp](../files/ex02/n2_opt.inp){ .md-button download="n2_opt.inp" }
[:material-file-document-outline: n2_opt.out](../files/ex02/n2_opt.out){ .md-button download="n2_opt.out" }
</div>

```text title="n2_opt.inp"
--8<-- "docs/files/ex02/n2_opt.inp"
```

The `Opt` keyword turns the single point into a geometry optimisation.

```bash
/path/to/orca n2_opt.inp > n2_opt.out
```

### Output

The output is a series of cycles, each starting with `GEOMETRY OPTIMIZATION CYCLE`.
Each cycle contains a full single point (as in Exercise 1) followed by a gradient
calculation and a geometry step. The energy goes down from cycle to cycle:

| Cycle | N–N (Å) | `FINAL SINGLE POINT ENERGY` (E<sub>h</sub>) |
| ----- | ------- | ------------------------------------------- |
| 1 | 1.400 | −108.651556 |
| 2 | 1.241 | −108.781510 |
| 3 | 1.083 | −108.853930 |
| 4 | 1.067 | −108.854060 |
| 5 | 1.074 | −108.854222 |
| 6 | 1.0735 | −108.854222 |

At the end of each cycle ORCA checks five convergence criteria. When all say `YES`, the
optimisation is done:

```text
          ----------------------|Geometry convergence|-------------------------
          Item                value                   Tolerance       Converged
          ---------------------------------------------------------------------
          Energy change      -0.0000001418            0.0000050000      YES
          RMS gradient        0.0000152415            0.0001000000      YES
          MAX gradient        0.0000152415            0.0003000000      YES
          RMS step            0.0000073680            0.0020000000      YES
          MAX step            0.0000073680            0.0040000000      YES
          ...
                    ***        THE OPTIMIZATION HAS CONVERGED     ***
...
     1. B(N   1,N   0)                1.0735  0.000015 -0.0000    1.0735
...
FINAL SINGLE POINT ENERGY      -108.854221747559
                                *** OPTIMIZATION RUN DONE ***
```

The optimised N–N bond is 1.0735 Å, lower in energy than every point of the scan. (The
experimental value is 1.098 Å; Hartree–Fock typically gives bonds that are slightly too
short.)

| File | Contents |
| ---- | -------- |
| `n2_opt.out` | Output. `OPTIMIZATION RUN DONE` means it converged; the last `FINAL SINGLE POINT ENERGY` is the energy of the optimised structure |
| `n2_opt.xyz` | Optimised structure |
| `n2_opt_trj.xyz` | All structures along the optimisation. Open it in a viewer to watch the optimisation as a movie |

## Part B · aspirin

For larger molecules, a cheaper method is often used for the optimisation.

<div class="buttons" markdown>
[:material-download: aspirin_opt.inp](../files/ex02/aspirin_opt.inp){ .md-button download="aspirin_opt.inp" }
[:material-file-document-outline: aspirin_opt.out](../files/ex02/aspirin_opt.out){ .md-button download="aspirin_opt.out" }
</div>

```text title="aspirin_opt.inp"
--8<-- "docs/files/ex02/aspirin_opt.inp"
```

HF-3c is HF with a minimal basis set plus corrections for dispersion and basis set
errors. It is fast enough for larger molecules. Note that there is no basis set on the
keyword line: HF-3c comes with its own.

```bash
/path/to/orca aspirin_opt.inp > aspirin_opt.out
```

### Output

The start structure from PubChem is already close to a minimum, so the energy only
drops by about 9 kcal/mol, from −640.7565 to −640.7712 E<sub>h</sub>. The optimisation
converges after 15 cycles and took 31 seconds on one core. With 21 atoms there are many
more coordinates to optimise than for N<sub>2</sub>, so the list of
`Redundant Internal Coordinates` after convergence contains all bonds, angles and
dihedrals.

!!! tip "Faster with more cores"
    Add `PAL4` to the keyword line (`! HF-3c Opt PAL4`) to run on 4 cores.
    See [Cores and memory](../orca.md#cores-and-memory).

## ORCA documentation

- [Geometry optimization (tutorial)](https://www.faccts.de/docs/orca/6.1/tutorials/prop/geoopt.html)
- [Geometry optimizations (manual)](https://www.faccts.de/docs/orca/6.1/manual/contents/structurereactivity/optimizations.html)
