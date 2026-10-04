# 2 · Geometry optimisation

**Part A:** optimise N<sub>2</sub>, starting from a stretched bond (1.40 Å).
**Part B:** optimise a larger molecule (aspirin) with a faster method.

!!! tip "First time running ORCA?"
    See [Running ORCA](../orca.md) for how to run an input file from the command line.
    `/path/to/orca` below is the full path to your ORCA program.

## Part A · N<sub>2</sub>

<div class="buttons" markdown>
[:material-download: n2_opt.inp](../files/ex02/n2_opt.inp){ .md-button download="n2_opt.inp" }
</div>

```text title="n2_opt.inp"
--8<-- "docs/files/ex02/n2_opt.inp"
```

The `Opt` keyword turns the single point into a geometry optimisation.

```bash
/path/to/orca n2_opt.inp > n2_opt.out
```

| File | Contents |
| ---- | -------- |
| `n2_opt.out` | Output. `OPTIMIZATION RUN DONE` means it converged; the last `FINAL SINGLE POINT ENERGY` is the energy of the optimised structure |
| `n2_opt.xyz` | Optimised structure |
| `n2_opt_trj.xyz` | All structures along the optimisation |

## Part B · aspirin

<div class="buttons" markdown>
[:material-download: aspirin_opt.inp](../files/ex02/aspirin_opt.inp){ .md-button download="aspirin_opt.inp" }
</div>

```text title="aspirin_opt.inp"
--8<-- "docs/files/ex02/aspirin_opt.inp"
```

HF-3c is HF with a minimal basis set plus corrections for dispersion and basis set
errors. It is fast enough for larger molecules.

```bash
/path/to/orca aspirin_opt.inp > aspirin_opt.out
```

## ORCA documentation

- [Geometry optimization (tutorial)](https://www.faccts.de/docs/orca/6.1/tutorials/prop/geoopt.html)
- [Geometry optimizations (manual)](https://www.faccts.de/docs/orca/6.1/manual/contents/structurereactivity/optimizations.html)
