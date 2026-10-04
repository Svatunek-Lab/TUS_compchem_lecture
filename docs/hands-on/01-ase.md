# 1 · Structures with ASE

<div class="nb-buttons" markdown>
[:simple-googlecolab: Open in Colab](https://colab.research.google.com/github/Svatunek-Lab/TUS_compchem_lecture/blob/main/docs/notebooks/01_ase.ipynb){ .md-button .md-button--primary }
[:material-eye: View on GitHub](https://github.com/Svatunek-Lab/TUS_compchem_lecture/blob/main/docs/notebooks/01_ase.ipynb){ .md-button }
[:material-download: Download notebook](../notebooks/01_ase.ipynb){ .md-button download }
</div>

The [Atomic Simulation Environment (ASE)](https://wiki.fysik.dtu.dk/ase/) is a Python
library for working with atomic structures. It reads and writes almost every structure
file format, and it can drive many different calculation programs through one common
interface.

## What you will do

- Build molecules from ASE's built-in database
- Look at atoms, positions, bond lengths and angles
- Read and write `.xyz` files
- Convert an ASE structure into input for PySCF (used in the next notebook)

## Key code

```python
from ase.build import molecule
from ase.io import write

ethanol = molecule("CH3CH2OH")
print(ethanol.get_chemical_formula())
print(ethanol.get_distance(0, 1))  # C–C distance in Å

write("ethanol.xyz", ethanol)
```

!!! note "Units"
    ASE uses Ångström for distances and eV for energies throughout.
