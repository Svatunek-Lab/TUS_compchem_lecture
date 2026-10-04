# 1 · Single point and scan

Calculate the energy of N<sub>2</sub> at one geometry (single point), then scan the N–N bond
length.

## Python (ASE + PySCF)

<div class="nb-buttons" markdown>
[:simple-googlecolab: Open in Colab](https://colab.research.google.com/github/Svatunek-Lab/TUS_compchem_lecture/blob/main/docs/notebooks/01_sp_scan.ipynb){ .md-button .md-button--primary target="_blank" rel="noopener" }
[:material-eye: View on GitHub](https://github.com/Svatunek-Lab/TUS_compchem_lecture/blob/main/docs/notebooks/01_sp_scan.ipynb){ .md-button target="_blank" rel="noopener" }
[:material-download: Download notebook](../notebooks/01_sp_scan.ipynb){ .md-button download="01_sp_scan.ipynb" }
</div>

The structure is entered directly in the notebook as xyz text, so there is no file to
save or upload:

```python
xyz = """2
N2
N   0.000   0.000   0.000
N   0.000   0.000   1.100
"""
atoms = read(StringIO(xyz), format="xyz")
```

- **Single point:** PySCF calculates the energy of this structure.
- **Scan:** ASE changes the N–N distance step by step (`atoms.set_distance`), PySCF
  calculates the energy at each step, and the result is plotted.

## ORCA

!!! tip "First time running ORCA?"
    See [Running ORCA](../orca.md) for how to run an input file from the command line.

### Single point

<div class="nb-buttons" markdown>
[:material-download: n2_sp.inp](../files/ex01/n2_sp.inp){ .md-button download="n2_sp.inp" }
</div>

```text title="n2_sp.inp"
--8<-- "docs/files/ex01/n2_sp.inp"
```

```bash
orca n2_sp.inp > n2_sp.out
```

The energy is on the line `FINAL SINGLE POINT ENERGY` in `n2_sp.out`.

### Scan

<div class="nb-buttons" markdown>
[:material-download: n2_scan.inp](../files/ex01/n2_scan.inp){ .md-button download="n2_scan.inp" }
</div>

```text title="n2_scan.inp"
--8<-- "docs/files/ex01/n2_scan.inp"
```

`B 0 1 = 0.90, 1.60, 15` scans the bond between atoms 0 and 1 (counting starts at 0) from
0.90 to 1.60 Å in 15 steps.

```bash
orca n2_scan.inp > n2_scan.out
```

The energies are listed at the end of `n2_scan.out` and in `n2_scan.relaxscanact.dat`;
all structures are in `n2_scan.allxyz`.

### ORCA documentation

- [Your first ORCA calculation](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/first_calc.html)
- [Single point energies](https://www.faccts.de/docs/orca/6.1/tutorials/prop/single_point.html)
- [Surface scans (manual)](https://www.faccts.de/docs/orca/6.1/manual/contents/structurereactivity/optimizations_scans.html)
