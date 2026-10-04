# 4 · Orbitals and cube files

Look at the orbital energies of acetone and create cube files of the HOMO, the LUMO and
the electron density for viewing in 3D.

!!! tip "First time running ORCA?"
    See [Running ORCA](../orca.md) for how to run an input file from the command line.
    `/path/to/orca` below is the full path to your ORCA program.

## Input

<div class="buttons" markdown>
[:material-download: acetone_orbitals.inp](../files/ex04/acetone_orbitals.inp){ .md-button download="acetone_orbitals.inp" }
</div>

```text title="acetone_orbitals.inp"
--8<-- "docs/files/ex04/acetone_orbitals.inp"
```

```bash
/path/to/orca acetone_orbitals.inp > acetone_orbitals.out
```

## Orbital energies

Search `acetone_orbitals.out` for `ORBITAL ENERGIES`. Each orbital is listed with its
number (`NO`), occupation (`OCC`) and energy in Hartree and eV.

- **HOMO:** the last orbital with occupation 2
- **LUMO:** the first orbital with occupation 0

Acetone has 32 electrons, so orbitals 0–15 are occupied: the HOMO is orbital 15, the LUMO
is orbital 16.

## Cube files

The `%plots` block writes the cube files at the end of the calculation:

| Line | Meaning |
| ---- | ------- |
| `Format Gaussian_Cube` | Write `.cube` files, which most viewers can open |
| `dim1 80` … `dim3 80` | Number of grid points in x, y, z (more = finer, larger files) |
| `min1 -8.0` … `max3 8.0` | Size of the box in **Bohr** (1 Bohr = 0.529 Å); it must contain the molecule |
| `MO("acetone_homo.cube", 15, 0);` | Orbital 15 (counting starts at 0). The last number is 0 for closed-shell molecules |
| `ElDens("acetone_dens.cube");` | Total electron density |

For another orbital, add a line with its number, e.g. `MO("acetone_homo-1.cube", 14, 0);`.

Open the `.cube` files in a viewer such as
[Avogadro](https://www.openchemistry.org/projects/avogadro2/) and display them as
an isosurface.

??? info "Creating cube files afterwards with orca_plot"
    You can also make cube files after the calculation, from the `.gbw` file:

    ```bash
    /path/to/orca_plot acetone_orbitals.gbw -i
    ```

    This starts an interactive menu where you choose the orbital and the output format.
    `orca_plot` is in the same folder as `orca`.

## ORCA documentation

- [Orbital and density plots (manual)](https://www.faccts.de/docs/orca/6.1/manual/contents/utilitiesvisualization/plots.html)
- [Graphical user interfaces](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/GUI.html)
