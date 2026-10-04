# Notebooks

Click **Colab** to run a notebook in the browser, **View** to read it on GitHub, or
**Download** to get the `.ipynb` file for running locally.

<!--
To add a notebook:
  1. Put the .ipynb file in docs/notebooks/
  2. Copy one of the rows below and change the file name in all three links.
-->

| # | Notebook | Topic | |
| - | -------- | ----- | - |
| 0 | Getting started | Python + RDKit basics: SMILES, drawing, properties, 3D | [:simple-googlecolab: Colab](https://colab.research.google.com/github/Svatunek-Lab/TUS_compchem_lecture/blob/main/docs/notebooks/00_getting_started.ipynb){ .md-button .md-button--primary } [:material-eye: View](https://github.com/Svatunek-Lab/TUS_compchem_lecture/blob/main/docs/notebooks/00_getting_started.ipynb){ .md-button } [:material-download: Download](notebooks/00_getting_started.ipynb){ .md-button download } |

## Code preview

A short taste of what is in notebook 0:

```python
from rdkit import Chem
from rdkit.Chem import Descriptors

mol = Chem.MolFromSmiles("CC(=O)Oc1ccccc1C(=O)O")  # aspirin
print(f"MW   = {Descriptors.MolWt(mol):.1f} g/mol")
print(f"logP = {Descriptors.MolLogP(mol):.2f}")
```
