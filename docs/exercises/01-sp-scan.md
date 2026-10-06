# 1 · Single point and scan

Calculate the energy of N<sub>2</sub> at one geometry (single point), then scan the N–N bond
length.

!!! tip "First time running ORCA?"
    See [Running ORCA](../orca.md) for how to run an input file from the command line.
    `/path/to/orca` below is the full path to your ORCA program.

## Single point

A **single point** is the simplest calculation: the atoms stay where they are, and ORCA
solves the electronic structure for exactly this geometry. The result is the total
energy plus everything that follows from the wavefunction (orbitals, charges, dipole
moment). Every other calculation type (optimisation, scan, frequencies) is built from
many single points.

<div class="buttons" markdown>
[:material-download: n2_sp.inp](../files/ex01/n2_sp.inp){ .md-button download="n2_sp.inp" }
[:material-file-document-outline: n2_sp.out](../files/ex01/n2_sp.out){ .md-button download="n2_sp.out" }
</div>

```text title="n2_sp.inp"
--8<-- "docs/files/ex01/n2_sp.inp"
```

### The input file

Every ORCA input has the same parts:

| Part | Example | Meaning |
| ---- | ------- | ------- |
| Comment | `# N2 single point` | Everything after `#` is ignored. Use it to note what the calculation is |
| Keyword line | `! HF def2-SVP` | Starts with `!`. Lists the **method**, the **basis set** and the **job type**. Order and upper/lower case do not matter, and you can use several `!` lines |
| Blocks | `%geom … end` | Optional. Start with `%` and end with `end`; detailed settings for one part of the program (see the scan below) |
| Structure | `* xyz 0 1` … `*` | Between the two `*`: the coordinates in Å, one atom per line. `0` is the **charge**, `1` the **multiplicity** (2S+1; 1 = all electrons paired) |

The keywords used in the exercises:

| Keyword | Type | Meaning |
| ------- | ---- | ------- |
| `HF`, `B3LYP` | method | Hartree–Fock; the B3LYP density functional |
| `HF-3c`, `Native-XTB2` | method | Fast low-cost methods (exercises 2 and 6) |
| `def2-SVP` | basis set | A small but reasonable basis set |
| *(none)* | job type | Without a job keyword ORCA runs a single point |
| `Opt` | job type | Geometry optimisation |
| `Freq` | job type | Vibrational frequencies |
| `GOAT` | job type | Conformer search |
| `PAL4` | technical | Run on 4 CPU cores (see below) |

!!! tip "Use more cores with the `PAL` keyword"
    By default ORCA uses one CPU core. Adding `PAL4` to the keyword line runs the
    calculation on 4 cores, `PAL8` on 8, and so on:

    ```text
    ! HF def2-SVP PAL4
    ```

    For N<sub>2</sub> this makes no difference, but for larger molecules, frequencies and
    conformer searches it saves a lot of time. Do not ask for more cores than your
    computer has, and note that parallel runs need ORCA to be started with its full path
    and OpenMPI installed. See [Cores and memory](../orca.md#cores-and-memory).

### Run it

```bash
/path/to/orca n2_sp.inp > n2_sp.out
```

### The output file

The output is long (about 800 lines for this tiny molecule), but it always has the same
structure. Download `n2_sp.out` above and look for these sections, from top to bottom:

| Line (approx.) | Search for | What it is |
| -------------- | ---------- | ---------- |
| 1–170 | `O R C A` | Program header, version, authors |
| 170 | `WARNINGS` | Warnings about your input. Read them! |
| 176 | `INPUT FILE` | A copy of your input |
| 195 | `CARTESIAN COORDINATES (ANGSTROEM)` | The structure that was calculated |
| 220 | `BASIS SET INFORMATION` | Basis set per element |
| 334 | `SCF SETTINGS` | Number of electrons, basis functions, convergence settings |
| 438 | `Iteration    Energy (Eh)` | The **SCF iterations**: the energy changes less and less until it is converged |
| 459 | `SCF CONVERGED AFTER` | The SCF converged (here after 8 cycles) |
| 465 | `TOTAL SCF ENERGY` | The energy and its components |
| 494 | `ORBITAL ENERGIES` | Every orbital with occupation and energy (see [Exercise 4](04-orbitals.md)) |
| 523, 559 | `MULLIKEN ATOMIC CHARGES`, `LOEWDIN ATOMIC CHARGES` | Atomic charges (see [Exercise 3](03-charges.md)) |
| 633 | `FINAL SINGLE POINT ENERGY` | **The result**: the total energy in Hartree |
| 690 | `DIPOLE MOMENT` | Dipole moment (zero for N<sub>2</sub> by symmetry) |
| 729 | `SUGGESTED CITATIONS` | Papers to cite if you publish the result |
| 795 | `ORCA TERMINATED NORMALLY` | The calculation finished without errors |

The important lines in `n2_sp.out`:

```text
               *****************************************************
               *                     SUCCESS                       *
               *           SCF CONVERGED AFTER   8 CYCLES          *
               *****************************************************
...
FINAL SINGLE POINT ENERGY      -108.851780980351
...
                             ****ORCA TERMINATED NORMALLY****
TOTAL RUN TIME: 0 days 0 hours 0 minutes 0 seconds 768 msec
```

The energy is in **Hartree** (E<sub>h</sub>; 1 E<sub>h</sub> = 627.5 kcal/mol =
2625.5 kJ/mol). An absolute energy on its own means little; what matters are
**energy differences** between structures calculated with the same method and basis
set, as in the scan below.

## Scan

A **scan** calculates the energy along one coordinate, here the N–N distance. The
result is a slice through the potential energy surface: a curve with a minimum at the
equilibrium bond length.

<div class="buttons" markdown>
[:material-download: n2_scan.inp](../files/ex01/n2_scan.inp){ .md-button download="n2_scan.inp" }
[:material-file-document-outline: n2_scan.out](../files/ex01/n2_scan.out){ .md-button download="n2_scan.out" }
</div>

```text title="n2_scan.inp"
--8<-- "docs/files/ex01/n2_scan.inp"
```

`B 0 1 = 0.90, 1.60, 15` scans the bond between atoms 0 and 1 (counting starts at 0) from
0.90 to 1.60 Å in 15 steps. The scan is defined in the `%geom` block and needs the `Opt`
keyword: at each step the scanned bond is fixed and everything else is optimised
(a *relaxed* scan). For N<sub>2</sub> there is nothing else to optimise, but for larger
molecules this matters.

```bash
/path/to/orca n2_scan.inp > n2_scan.out
```

### Output

The output contains one full optimisation per step, each starting with
`RELAXED SURFACE SCAN STEP`, so it is very long (17 000 lines). The summary is at the
end, under `RELAXED SURFACE SCAN RESULTS`:

```text
RELAXED SURFACE SCAN RESULTS
----------------------------

The Calculated Surface using the 'Actual Energy'
   0.90000000 -108.68732308
   0.95000000 -108.77936209
   1.00000000 -108.83065788
   1.05000000 -108.85207208
   1.10000000 -108.85178098
   1.15000000 -108.83595213
   1.20000000 -108.80923572
   ...
   1.60000000 -108.48256001
```

The first column is the N–N distance in Å, the second the energy in Hartree. The
minimum lies between 1.05 and 1.10 Å. Note that the energy at 1.10 Å is exactly the
single point energy from above.

The same data is in `n2_scan.relaxscanact.dat`, which you can plot directly (e.g. in
Excel or Python). All structures are in `n2_scan.allxyz`.

!!! question "Try it"
    Plot the curve and convert the energies to kcal/mol relative to the minimum. How much
    energy does it cost to stretch the bond by 0.1 Å? By 0.5 Å? In [Exercise 2](02-opt.md)
    you find the exact minimum with an optimisation.

## ORCA documentation

- [Your first ORCA calculation](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/first_calc.html)
- [Input and output](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/input_output.html)
- [Single point energies](https://www.faccts.de/docs/orca/6.1/tutorials/prop/single_point.html)
- [Surface scans (manual)](https://www.faccts.de/docs/orca/6.1/manual/contents/structurereactivity/optimizations_scans.html)
