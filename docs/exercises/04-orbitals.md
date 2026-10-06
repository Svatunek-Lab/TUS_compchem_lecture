# 4 · Orbitals and cube files

**Part A:** look at the orbital energies of acetone and create cube files of the HOMO,
the LUMO and the electron density for viewing in 3D.
**Part B:** plot the six π orbitals of benzene.

!!! tip "First time running ORCA?"
    See [Running ORCA](../orca.md) for how to run an input file from the command line.
    `/path/to/orca` below is the full path to your ORCA program.

## Part A · acetone

<div class="buttons" markdown>
[:material-download: acetone_orbitals.inp](../files/ex04/acetone_orbitals.inp){ .md-button download="acetone_orbitals.inp" }
[:material-file-document-outline: acetone_orbitals.out](../files/ex04/acetone_orbitals.out){ .md-button download="acetone_orbitals.out" }
[:material-cube-outline: acetone_homo.cube](../files/ex04/acetone_homo.cube){ .md-button download="acetone_homo.cube" }
[:material-cube-outline: acetone_lumo.cube](../files/ex04/acetone_lumo.cube){ .md-button download="acetone_lumo.cube" }
</div>

This is a single point (as in [Exercise 1](01-sp-scan.md)) with one addition: the
`%plots` block, which writes the orbitals and the density to files on a 3D grid, so a
viewer can draw them.

```text title="acetone_orbitals.inp"
--8<-- "docs/files/ex04/acetone_orbitals.inp"
```

```bash
/path/to/orca acetone_orbitals.inp > acetone_orbitals.out
```

### Orbital energies

Search `acetone_orbitals.out` for `ORBITAL ENERGIES`. Each orbital is listed with its
number (`NO`), occupation (`OCC`) and energy in Hartree and eV.

- **HOMO:** the last orbital with occupation 2
- **LUMO:** the first orbital with occupation 0

Acetone has 32 electrons, so orbitals 0–15 are occupied: the HOMO is orbital 15, the LUMO
is orbital 16.

```text
  NO   OCC          E(Eh)            E(eV)
   0   2.0000     -19.115932      -520.1710
   1   2.0000     -10.270499      -279.4745
   2   2.0000     -10.189944      -277.2825
   3   2.0000     -10.189920      -277.2818
   4   2.0000      -1.014273       -27.5998
  ...
  14   2.0000      -0.344490        -9.3741
  15   2.0000      -0.244974        -6.6661      <- HOMO
  16   0.0000      -0.015555        -0.4233      <- LUMO
  17   0.0000       0.050499         1.3741
```

Orbitals 0–3 are the **core orbitals** (O 1s and C 1s), far below everything else; the
oxygen 1s is lowest. The chemistry happens near the HOMO and LUMO. In acetone the HOMO
is mainly the oxygen lone pair (n<sub>O</sub>) and the LUMO is the C=O π* orbital, the
orbital a nucleophile attacks. The HOMO–LUMO gap here is 6.24 eV.

### Cube files

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

## Part B · the π orbitals of benzene

Benzene has six π orbitals, built from the six C 2p orbitals perpendicular to the
ring. Three are occupied (6 π electrons) and three are empty. This input writes a cube
file for each of them.

<div class="buttons" markdown>
[:material-download: benzene_orbitals.inp](../files/ex04/benzene_orbitals.inp){ .md-button download="benzene_orbitals.inp" }
[:material-file-document-outline: benzene_orbitals.out](../files/ex04/benzene_orbitals.out){ .md-button download="benzene_orbitals.out" }
[:material-cube-outline: benzene_pi1.cube](../files/ex04/benzene_pi1.cube){ .md-button download="benzene_pi1.cube" }
[:material-cube-outline: benzene_pi2.cube](../files/ex04/benzene_pi2.cube){ .md-button download="benzene_pi2.cube" }
[:material-cube-outline: benzene_pi3.cube](../files/ex04/benzene_pi3.cube){ .md-button download="benzene_pi3.cube" }
[:material-cube-outline: benzene_pi4.cube](../files/ex04/benzene_pi4.cube){ .md-button download="benzene_pi4.cube" }
[:material-cube-outline: benzene_pi5.cube](../files/ex04/benzene_pi5.cube){ .md-button download="benzene_pi5.cube" }
[:material-cube-outline: benzene_pi6.cube](../files/ex04/benzene_pi6.cube){ .md-button download="benzene_pi6.cube" }
</div>

```text title="benzene_orbitals.inp"
--8<-- "docs/files/ex04/benzene_orbitals.inp"
```

```bash
/path/to/orca benzene_orbitals.inp > benzene_orbitals.out
```

Benzene has 42 electrons, so orbitals 0–20 are occupied. The π orbitals are **not** simply
the ones closest to the HOMO and LUMO: σ orbitals lie in between. In `ORBITAL ENERGIES`:

```text
  NO   OCC          E(Eh)            E(eV)
  ...
  16   2.0000      -0.364599        -9.9212      <- pi1
  17   2.0000      -0.343859        -9.3569         (sigma)
  18   2.0000      -0.343853        -9.3567         (sigma)
  19   2.0000      -0.252366        -6.8672      <- pi2  HOMO
  20   2.0000      -0.252352        -6.8668      <- pi3  HOMO
  21   0.0000      -0.005692        -0.1549      <- pi4* LUMO
  22   0.0000      -0.005680        -0.1545      <- pi5* LUMO
  23   0.0000       0.062969         1.7135         (sigma*)
  ...
  28   0.0000       0.151291         4.1168      <- pi6*
```

| Orbital | Number | Nodal planes through the ring |
| ------- | ------ | ----------------------------- |
| π<sub>1</sub> | 16 | 0 |
| π<sub>2</sub>, π<sub>3</sub> (HOMO) | 19, 20 | 1 |
| π<sub>4</sub>\*, π<sub>5</sub>\* (LUMO) | 21, 22 | 2 |
| π<sub>6</sub>\* | 28 | 3 (a node between every pair of C atoms) |

The HOMO and the LUMO are each **degenerate**: two orbitals with the same energy. The
small differences in the last digits come from the start structure not being perfectly
symmetric.

!!! question "Try it"
    Open the six cube files and count the nodal planes of each orbital. Compare with the
    Hückel π-orbital diagram (Frost circle) of benzene.

!!! tip "How do I find out which orbitals are π orbitals?"
    The orbital energies alone do not tell you. Plot a few orbitals around the HOMO and
    LUMO and look at them: π orbitals have a node in the plane of the ring.

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
