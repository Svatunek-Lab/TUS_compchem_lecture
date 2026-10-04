# 0 · Molecules with RDKit

<div class="nb-buttons" markdown>
[:simple-googlecolab: Open in Colab](https://colab.research.google.com/github/Svatunek-Lab/TUS_compchem_lecture/blob/main/docs/notebooks/00_getting_started.ipynb){ .md-button .md-button--primary }
[:material-eye: View on GitHub](https://github.com/Svatunek-Lab/TUS_compchem_lecture/blob/main/docs/notebooks/00_getting_started.ipynb){ .md-button }
[:material-download: Download notebook](../notebooks/00_getting_started.ipynb){ .md-button download }
</div>

[RDKit](https://www.rdkit.org) is the standard open-source cheminformatics toolkit. It is
how we get from a name or a SMILES string to a molecule object we can work with in
Python.

## What you will do

- Read molecules from SMILES strings
- Draw 2D structures, one at a time or in a grid
- Calculate simple properties (molecular weight, logP, polar surface area)
- Generate a 3D structure, run a quick force-field optimisation and view it

## Key code

```python
from rdkit import Chem
from rdkit.Chem import Descriptors

mol = Chem.MolFromSmiles("CC(=O)Oc1ccccc1C(=O)O")  # aspirin
print(f"MW = {Descriptors.MolWt(mol):.1f} g/mol")
```

Going from 2D to 3D takes three steps: add hydrogens, embed coordinates, optimise.

```python
from rdkit.Chem import AllChem

mol3d = Chem.AddHs(mol)
AllChem.EmbedMolecule(mol3d, randomSeed=42)
AllChem.MMFFOptimizeMolecule(mol3d)  # (1)!
```

1.  MMFF94 is a classical force field: fast, and good enough for a reasonable starting
    geometry. It knows nothing about electrons, which is why we need quantum chemistry
    later.
