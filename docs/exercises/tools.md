# Tools

## Name / SMILES → XYZ

To calculate your own molecule you need its 3D coordinates. This notebook turns a
molecule **name** (e.g. `aspirin`) or a **SMILES** (e.g. `CC(=O)Oc1ccccc1C(=O)O`) into
coordinates and prints a ready-to-run ORCA input. It runs in your browser on Google
Colab; nothing needs to be installed. You only need a Google account.

<div class="buttons" markdown>
[:simple-googlecolab: Open in Colab](https://colab.research.google.com/github/Svatunek-Lab/TUS_compchem_lecture/blob/main/notebooks/smiles_to_xyz.ipynb){ .md-button .md-button--primary target="_blank" }
</div>

1. Click **Open in Colab**.
2. Run cell 1 (▶) to install the required packages. Colab may warn that the notebook
   is not from Google; click **Run anyway**.
3. Enter your molecule in cell 2 and run it. The ORCA input is printed below the cell.
4. Optional: cell 3 shows the structure in 3D, cell 4 downloads the `.inp` and `.xyz`
   files.

For the next molecule, change the name in cell 2 and run cell 2 again.

!!! warning "This is a start structure"
    The coordinates come from a force field. Always optimise them with ORCA (`Opt`, see
    [Exercise 2](02-opt.md)) before you calculate anything else. For flexible molecules
    this is only one conformer; see [Exercise 6](06-conf-search.md).

!!! info "Stereochemistry"
    A name or SMILES without stereo information gives an arbitrary stereoisomer. Include
    it if it matters: `trans-2-butene`, `(R)-2-butanol`, `C/C=C/C`, `C[C@@H](O)CC`.
