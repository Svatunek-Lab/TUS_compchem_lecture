# Setup

The notebooks use **ASE** and **PySCF**. The ORCA parts need a separate
[ORCA installation](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/install.html)
(free for academic use).

=== "Google Colab (recommended)"

    Nothing to install. Click **Open in Colab** on an exercise page and sign in with a
    Google account. The first cell of each notebook installs the packages it needs.

=== "Local (conda)"

    Install [Miniforge](https://github.com/conda-forge/miniforge), then:

    ```bash
    conda create -n compchem -c conda-forge python=3.12 ase pyscf py3dmol matplotlib jupyterlab
    conda activate compchem
    jupyter lab
    ```

    Skip the `!pip install` cell at the top of the notebooks.

=== "Local (uv / pip)"

    ```bash
    uv venv
    uv pip install ase pyscf py3Dmol matplotlib jupyterlab
    uv run jupyter lab
    ```

!!! warning "Windows"
    PySCF does not run natively on Windows. Use Colab, or
    [WSL](https://learn.microsoft.com/windows/wsl/install).
