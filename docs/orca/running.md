# Running ORCA

## Installing

1. Register on the [ORCA forum](https://orcaforum.kofo.mpg.de) and download the build for
   your operating system.
2. Unpack it, for example to `~/orca` (Linux/macOS) or `C:\orca` (Windows).
3. Add that folder to your `PATH`.
4. For parallel runs on Linux/macOS, also install the OpenMPI version named in the ORCA
   release notes.

## The input file

An ORCA input is a plain text file, usually ending in `.inp`:

```text title="acetone.inp"
--8<-- "docs/files/orca/acetone.inp"
```

| Line | Meaning |
| ---- | ------- |
| `# ...` | A comment |
| `! r2SCAN-3c Opt Freq` | The **keyword line**: method and what to do. Here: the r²SCAN-3c composite DFT method, geometry optimisation (`Opt`) and frequencies (`Freq`) |
| `%pal nprocs 4 end` | Number of CPU cores to use |
| `%maxcore 2000` | Memory **per core** in MB |
| `* xyzfile 0 1 acetone.xyz` | The structure: read from `acetone.xyz`, with charge 0 and multiplicity 1 (singlet) |

You can also put the coordinates directly into the input:

```text
! r2SCAN-3c Opt Freq
* xyz 0 1
C   0.000   0.000   0.180
O   0.000   0.000   1.395
...
*
```

## Running a job

=== "Linux / macOS"

    ```bash
    $(which orca) acetone.inp > acetone.out
    ```

    For parallel runs ORCA must be called with its **full path**, which is what
    `$(which orca)` gives.

=== "Windows"

    ```powershell
    orca.exe acetone.inp > acetone.out
    ```

Run each job in its own folder. ORCA writes several files next to the input.

## Output files

| File | Contents |
| ---- | -------- |
| `acetone.out` | The main output. Everything is in here. |
| `acetone.xyz` | Final optimised geometry (overwrites the input `.xyz` if they share a name, so keep a copy) |
| `acetone_trj.xyz` | All geometries along the optimisation, for viewing as a movie |
| `acetone.hess` | Hessian and vibrational frequencies |
| `acetone.gbw` | Orbitals (binary); can be reused as a starting guess |

To look at structures, orbitals and vibrations use a viewer such as
[Avogadro](https://www.openchemistry.org/projects/avogadro2/),
[Chemcraft](https://www.chemcraftprog.com) or [IQmol](https://www.iqmol.org).

## Finding results in the output

Useful lines to search for in the `.out` file:

| Search for | What it is |
| ---------- | ---------- |
| `FINAL SINGLE POINT ENERGY` | Electronic energy (Hartree), printed after every step |
| `OPTIMIZATION RUN DONE` | The optimisation converged |
| `VIBRATIONAL FREQUENCIES` | Frequencies in cm⁻¹; imaginary ones are printed as negative |
| `Final Gibbs free energy` | Gibbs free energy (Hartree) at 298.15 K |
| `ORCA TERMINATED NORMALLY` | The job finished without crashing |

```bash
grep "FINAL SINGLE POINT ENERGY" acetone.out | tail -1
```
