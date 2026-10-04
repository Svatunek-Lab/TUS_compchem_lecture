# Setup

You can run the notebooks in two ways.

=== "Google Colab (recommended)"

    Nothing to install. Click the **Open in Colab** button next to a notebook on the
    [Notebooks](notebooks.md) page and sign in with a Google account.

    The first cell of each notebook installs the packages it needs:

    ```python
    !pip install -q rdkit py3Dmol
    ```

=== "Local (conda)"

    Install [Miniforge](https://github.com/conda-forge/miniforge), then create an
    environment:

    ```bash
    conda create -n compchem -c conda-forge python=3.12 rdkit py3dmol jupyterlab
    conda activate compchem
    jupyter lab
    ```

    Download the notebooks from the [Notebooks](notebooks.md) page and open them in
    JupyterLab. Skip the `!pip install` cell at the top.

=== "Local (uv / pip)"

    ```bash
    uv venv
    uv pip install rdkit py3Dmol jupyterlab
    uv run jupyter lab
    ```

## Check that it works

Run this in a notebook cell. You should see a 2D drawing of aspirin.

```python
from rdkit import Chem
Chem.MolFromSmiles("CC(=O)Oc1ccccc1C(=O)O")
```
