# 2 · Quantum chemistry with PySCF

<div class="nb-buttons" markdown>
[:simple-googlecolab: Open in Colab](https://colab.research.google.com/github/Svatunek-Lab/TUS_compchem_lecture/blob/main/docs/notebooks/02_pyscf.ipynb){ .md-button .md-button--primary }
[:material-eye: View on GitHub](https://github.com/Svatunek-Lab/TUS_compchem_lecture/blob/main/docs/notebooks/02_pyscf.ipynb){ .md-button }
[:material-download: Download notebook](../notebooks/02_pyscf.ipynb){ .md-button download }
</div>

[PySCF](https://pyscf.org) is a quantum chemistry package written in Python. It lets us
run Hartree–Fock and DFT calculations directly from a notebook and look at every
intermediate result.

## What you will do

- Define a molecule and a basis set
- Run Hartree–Fock and DFT (B3LYP) single-point calculations
- Look at orbital energies, the HOMO–LUMO gap and the dipole moment
- Optimise a geometry

## Key code

```python
from pyscf import gto, dft

mol = gto.M(
    atom="""
    O  0.000  0.000  0.117
    H  0.000  0.757 -0.467
    H  0.000 -0.757 -0.467
    """,
    basis="def2-svp",
)
mf = dft.RKS(mol, xc="b3lyp").run()
print(mf.e_tot)  # total energy in Hartree
```

!!! note "Units"
    PySCF reports energies in Hartree (1 E<sub>h</sub> = 627.5 kcal/mol = 27.21 eV).

!!! warning "Windows"
    PySCF does not run natively on Windows. Use Colab, or WSL on Windows.
