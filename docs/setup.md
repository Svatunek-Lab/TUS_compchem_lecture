# Setup

The hands-on part uses **RDKit**, **ASE** and **PySCF**. You can run the notebooks in two
ways. The [ORCA](orca/index.md) examples need a separate installation, described in
[Running ORCA](orca/running.md).

=== "Google Colab (recommended)"

    Nothing to install. Click **Open in Colab** on any [hands-on page](hands-on/index.md)
    and sign in with a Google account.

    The first cell of each notebook installs the packages it needs, for example:

    ```python
    !pip install -q pyscf geometric
    ```

=== "Local (conda)"

    Install [Miniforge](https://github.com/conda-forge/miniforge), then create an
    environment:

    ```bash
    conda create -n compchem -c conda-forge python=3.12 \
        rdkit ase pyscf geometric py3dmol jupyterlab
    conda activate compchem
    jupyter lab
    ```

    Download the notebooks from the hands-on pages and open them in JupyterLab. Skip
    the `!pip install` cell at the top.

=== "Local (uv / pip)"

    ```bash
    uv venv
    uv pip install rdkit ase pyscf geometric py3Dmol jupyterlab
    uv run jupyter lab
    ```

!!! warning "Windows"
    PySCF does not run natively on Windows. Use Colab, or install
    [WSL](https://learn.microsoft.com/windows/wsl/install) and follow the Linux
    instructions there. RDKit and ASE work fine on Windows.

## Check that it works

Run this in a notebook cell. It should print an energy close to −76 Hartree.

```python
from pyscf import gto, scf

mol = gto.M(atom="O 0 0 0.117; H 0 0.757 -0.467; H 0 -0.757 -0.467", basis="sto-3g")
scf.RHF(mol).run()
```
