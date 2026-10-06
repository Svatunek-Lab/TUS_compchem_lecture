# 5 · Optimisation and frequencies

Optimise a structure and calculate its vibrational frequencies.
**Part A:** N<sub>2</sub>. **Part B:** acetone.

!!! tip "First time running ORCA?"
    See [Running ORCA](../orca.md) for how to run an input file from the command line.
    `/path/to/orca` below is the full path to your ORCA program.

The frequency calculation tells you:

- whether the optimised structure is a **minimum** (all frequencies positive) or a
  **transition state** (exactly one imaginary frequency, printed as a negative number)
- the **IR spectrum**
- **thermochemistry**: zero-point energy, enthalpy and Gibbs free energy

Frequencies are only meaningful at an optimised structure, calculated with the same
method. That is why `Opt` and `Freq` are run together.

## Part A · N<sub>2</sub>

<div class="buttons" markdown>
[:material-download: n2_freq.inp](../files/ex05/n2_freq.inp){ .md-button download="n2_freq.inp" }
</div>

```text title="n2_freq.inp"
--8<-- "docs/files/ex05/n2_freq.inp"
```

```bash
/path/to/orca n2_freq.inp > n2_freq.out
```

## Part B · acetone

<div class="buttons" markdown>
[:material-download: acetone_freq.inp](../files/ex05/acetone_freq.inp){ .md-button download="acetone_freq.inp" }
</div>

```text title="acetone_freq.inp"
--8<-- "docs/files/ex05/acetone_freq.inp"
```

```bash
/path/to/orca acetone_freq.inp > acetone_freq.out
```

!!! tip "Faster with more cores"
    Frequency calculations are much more expensive than single points. Add `PAL4` to the
    keyword line (`! B3LYP def2-SVP Opt Freq PAL4`) to run on 4 cores.
    See [Cores and memory](../orca.md#cores-and-memory).

## Output

| Search for | What it is |
| ---------- | ---------- |
| `VIBRATIONAL FREQUENCIES` | All frequencies in cm⁻¹. Imaginary frequencies are negative and marked `***imaginary mode***` |
| `IR SPECTRUM` | Frequencies with IR intensities |
| `Zero point energy` | Zero-point vibrational energy |
| `Total Enthalpy` | Enthalpy at 298.15 K |
| `Final Gibbs free energy` | Gibbs free energy at 298.15 K |

The list of frequencies has one entry per coordinate (3 × number of atoms). The first
ones are zero: they are translations and rotations of the whole molecule, not
vibrations.

| Molecule | Entries | Zero | Vibrations |
| -------- | ------- | ---- | ---------- |
| N<sub>2</sub> (linear) | 6 | 5 | 1 |
| acetone | 30 | 6 | 24 |

To animate the vibrations, open the `.out` file in a viewer such as
[Avogadro](https://www.openchemistry.org/projects/avogadro2/) or
[Chemcraft](https://www.chemcraftprog.com). The frequencies are also saved in the `.hess`
file.

## ORCA documentation

- [Vibrational frequencies (tutorial)](https://www.faccts.de/docs/orca/6.1/tutorials/prop/freq.html)
- [Thermodynamics (tutorial)](https://www.faccts.de/docs/orca/6.1/tutorials/prop/thermo.html)
- [IR spectra (tutorial)](https://www.faccts.de/docs/orca/6.1/tutorials/spec/IR.html)
