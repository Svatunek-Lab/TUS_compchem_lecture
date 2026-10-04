# 2 · Geometry optimisation

**Part A:** optimise N<sub>2</sub>, starting from a stretched bond (1.40 Å).
**Part B:** optimise a larger molecule (aspirin) with a faster method.

## Python (ASE + PySCF)

<div class="nb-buttons" markdown>
[:simple-googlecolab: Open in Colab](https://colab.research.google.com/github/Svatunek-Lab/TUS_compchem_lecture/blob/main/docs/notebooks/02_opt.ipynb){ .md-button .md-button--primary target="_blank" rel="noopener" }
[:material-eye: View on GitHub](https://github.com/Svatunek-Lab/TUS_compchem_lecture/blob/main/docs/notebooks/02_opt.ipynb){ .md-button target="_blank" rel="noopener" }
[:material-download: Download notebook](../notebooks/02_opt.ipynb){ .md-button download="02_opt.ipynb" }
</div>

- PySCF calculates the energy and the forces on the atoms. A small helper class
  (`PySCFCalculator`) passes them to ASE.
- ASE's `BFGS` optimiser moves the atoms until the forces are below `fmax`:

```python
atoms.calc = PySCFCalculator()
opt = BFGS(atoms, trajectory="opt.traj")
opt.run(fmax=0.01)  # eV/Å
```

The start and optimised structures are then shown side by side with py3Dmol.

| Part | Method |
| ---- | ------ |
| A · N<sub>2</sub> | HF/def2-SVP |
| B · aspirin | HF/STO-3G (minimal basis, fast) |

## ORCA

!!! tip "First time running ORCA?"
    See [Running ORCA](../orca.md) for how to run an input file from the command line.

### Part A · N<sub>2</sub>

<div class="nb-buttons" markdown>
[:material-download: n2_opt.inp](../files/ex02/n2_opt.inp){ .md-button download="n2_opt.inp" }
</div>

```text title="n2_opt.inp"
--8<-- "docs/files/ex02/n2_opt.inp"
```

The `Opt` keyword turns the single point into a geometry optimisation.

### Part B · aspirin

<div class="nb-buttons" markdown>
[:material-download: aspirin_opt.inp](../files/ex02/aspirin_opt.inp){ .md-button download="aspirin_opt.inp" }
</div>

```text title="aspirin_opt.inp"
--8<-- "docs/files/ex02/aspirin_opt.inp"
```

HF-3c is HF with a minimal basis set plus corrections for dispersion and basis set
errors. It is fast enough for larger molecules.

### Running and output

```bash
orca n2_opt.inp > n2_opt.out
```

| File | Contents |
| ---- | -------- |
| `n2_opt.out` | Output. `OPTIMIZATION RUN DONE` means it converged; the last `FINAL SINGLE POINT ENERGY` is the energy of the optimised structure |
| `n2_opt.xyz` | Optimised structure |
| `n2_opt_trj.xyz` | All structures along the optimisation |

### ORCA documentation

- [Geometry optimization (tutorial)](https://www.faccts.de/docs/orca/6.1/tutorials/prop/geoopt.html)
- [Geometry optimizations (manual)](https://www.faccts.de/docs/orca/6.1/manual/contents/structurereactivity/optimizations.html)
